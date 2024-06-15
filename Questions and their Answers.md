# Famous Paintings and their Artist
## Questions and Answers


**1.** Fetch all the paintings which are not displayed on any museum.

````sql
select w.name
from work as w
where w.museum_id is null;
````

**Results:**
Paintings                                 |
------------------------------------------|
A Lane Near Arles, Side of a Country Lane |
A Pair of Leather Clogs                   |
A Path Through a Ravine                   |
A Woman Walking in a Garden               |
Apples                                    |

**2.** Select artists with non-null middle names
````sql
select first_name,middle_names,last_name
from artist
where middle_names IS  NOT NULL ;
````

**Results:**
first_name |middle_name|last_name  |
-----------|-----------|-----------|
Pierre	   |Auguste	   |Renoir     |
Lemuel	   |Francis	   |Abbott     |
Eugene	   |Louis	     |Boudin     |
Asher	     |Brown	     |Durand     |
George     |Henry	     |Laporte    |

**3.** Show first and last names of artists, ordered by first name in ascending order:
````sql
select concat(a.first_name,' ', a.last_name) as 'Name'
from artist as a
order by a.first_name asc;
````
**Results:**
name_of_artist    |
------------------|
Adelaide Guiard   |
Adele Romany      |
Adolph Wertmuller |
Adriaan De Lelie  |
Adriaen Key       |

**4.** Show first names of artists that start with 'p':
````sql
select distinct a.first_name
from artist as a
where a.first_name like 'p%';
````
**Results:**
name_of_artist |
---------------|
Pierre         |
Piet           |
Peter          |
Pieter         |
Pompeo         |

**5.** Count the number of artists by nationality, ordered by the number of artists in descending order

````sql
select a.nationality, count(a.artist_id) as 'no of artists'
from artist as a
group by a.nationality
order by count(a.artist_id) desc;
````

**Results:**
nationality | no_of_artists |
------------|---------------|
French	    |115            |
American	  |83             |
Dutch	      |68             |
English   	|44             |
Italian   	|26             |
German      |20             |
Russian    	|14             |
Swiss	      |11             |

**6.** Show the age of artists whose age is between 40 and 50

````sql
select a.full_name, (a.death- a.birth) as age
from artist as a
where (a.death- a.birth) between 40 and 50;
````

**Results:**
Artist               |age |
---------------------|----|
Lemuel Francis Abbott|43  |
Thomas Cole	         |47  |
Pieter Quast	       |41  |
George Wesley Bellows|43  |
Theo Van Doesburg	   |48  |

**7.** Find the average number of years artists lived:
````sql
select avg(a.death - a.birth) as avg_age
from artist as a
where a.death is not null and a.birth is not null;
````
**Results:**
avg_age|
-------|
66.3325|

**8.** Count the number of museums by country, ordered by the number of museums in descending order

````sql
select m.country, count(m.name) as 'no of museum'
from museum as m
group by m.country
order by count(m.name) desc;
````

**Results:**
Country     |no_of_museum |
------------|-------------|
USA	        |25           |
UK	        |5            |
France    	|4            |
Netherlands	|3            |
Russia    	|2            |
Switzerland	|2            |


**9.** Count the number of works by style, ordered by the number of styles in descending order:

````sql
select w.style, count(w.style) as 'no of styles'
from work as w 
group by w.style
order by count(w.style) desc;
````

**Results:**
style              |no_of_style |
-------------------|------------|
Impressionism	     |3078        |
Post-Impressionism |1672        |
Realism	           |1179        |
Baroque	           |972         |
Expressionism	     |673         |
Fauvism	           |653         |
Rococo           	 |535         |

**10.** Count the number of paintings by subject, ordered by the number of paintings in descending order, limited to 3 results:

````sql
select s.subject, count(s.subject) as 'no of paintings'
from subject as s 
group by s.subject
order by count(s.subject) desc
limit 3;
````
**Results:**
subject       |no_of_aintings|
--------------|--------------|
Portraits   	|2140          |
Nude	        |1050          |
Landscape Art	|990           |

**11.** Count the number of paintings where the sale price equals the regular price
````sql
select count(p.work_id) as 'no of paintings'
from product_size as p 
where p.sale_price = p.regular_price ;
````
**Results:**
no of paintings|
---------------|
15080          |

**12.** Count the number of paintings where the sale price is less than the regular price:
````sql
select count(p.work_id) as 'no of paintings'
from product_size as p
where p.sale_price < p.regular_price ;
````
**Results:**
no of paintings|
---------------|
205613         |

**13.** Count the number of paintings where the sale price is greater than the regular price:
````sql
select count(p.work_id) as 'no of paintings'
from product_size as p
where p.sale_price > p.regular_price ;
````
**Results:**
no of paintings|
---------------|
0              |

**14.** Retrieve all paintings by Pierre-Auguste Renoir:
````sql
select w.name as paintings
from work as w
join artist as a
on a.artist_id=w.artist_id;
````
**Results:**
paintings                                  |
-------------------------------------------|
Starry Night                               |
Blossoming Almond Tree                     |
Cafe´ Terrace at Night                     |
A Group of Cottages                        |
A Lane Near Arles, Side of a Country Lane  |

**15.** Count the number of paintings per artist:
````sql
select a.full_name as artist, count(w.style) as 'number of paintings '
from work as w
join artist as a
on a.artist_id=w.artist_id
group by a.full_name;
````
**Results:**
artist             |number of paintings|
-------------------|-------------------|
Vincent Van Gogh	 |302                |
Johannes Vermeer	 |32                 |
Paul Cézanne	     |146                |
Leonardo Da Vinci  |17                 |
Claude Monet	     |371                |

**16.** Find all artist who born in the 19th century:
````sql
select a.full_name artist, a.birth
from artist as a
where a.birth between 1801 and 1900;
````
**Results:**
artist                 |birth |
-----------------------|------|
Pierre-Auguste Renoir	 |1841  |
Alexandre Cabanel	     |1823  |
James Ensor	           |1860  |
Maximilien Luce	       |1858  |
August Macke	         |1887  |

**17.** List artists who have more than 60 paintings in the database:
````sql
select a.full_name as artist, count(w.artist_id) as no_of_paintings
from artist as a
join work as w
on a.artist_id=w.artist_id
group by w.artist_id
having count(w.artist_id) > 60
order by count(w.artist_id);
````
**Results:**
artist              |no_of_paintings|
--------------------|---------------|
George Inness	      |62             |
El Greco	          |62             |
Edvard Munch     	  |64             |
Juan Gris	          |66             |
Sir Joshua Reynolds	|66             |

**18.** Are there museuems without any paintings?
````sql
select m.name as museum
from museum as m
left join work as w
on w.museum_id=m.museum_id
where w.work_id is null;
````
**Results:**
museum|
------|
0     |

**19.** Identify the paintings whose asking price is less than 50% of its regular price
````sql
select distinct w.name as painting, p.sale_price, p.regular_price, 0.5 * regular_price as '50% of regularprice' 
from work as w
join product_size as p
on w.work_id=p.work_id
where p.sale_price < (0.5 * p.regular_price);
````
**Results:**
paintings                                               | sale_price| regular_price | 50% of its regular price |
--------------------------------------------------------|-----------|---------------|--------------------------|
Ducks Resting in Sunshine	                              |285	      |575	          |287.5                     |
Ducks Resting in Sunshine	                              |305	      |645	          |322.5                     |
Portrait of Mr. and Mrs. Thomas Mifflin (Sarah Morris)	|20	        |95	            |47.5                      |
Portrait of Mr. and Mrs. Thomas Mifflin (Sarah Morris)	|20	        |85	            |42.5                      |
Passion Flowers and Hummingbirds	                      |585	      |1245	          |622.5                     |
A Street in Paris in May 1871	                          |1025       |2235	          |1117.5                    |

**20.** Which canva size costs the most?
````sql
select c.label, p.sale_price 
from product_size as p
join canvas_size as c
on c.size_id = p.size_id
order by p.sale_price desc 
limit 1;
````
**Results:**
canva_size                 |sale_price|
---------------------------|----------|
48" x 96"(122 cm x 244 cm) |1115      |

**21.** Identify the museums with invalid city information in the given dataset
````sql
select *
from museum
where city regexp '^[0-9]';
````
**Results:**
museum_id name                       address city       state  postal country phone url
34        The State Hermitage Museum 2       Sankt-Peterburg 190000 Russia 7 812 710-90-79 https://www.hermitagemuseum.org/wps/portal/hermitage/
**22.** Fetch the top 10 most famous painting subject
````sql
select s.subject as famous_paintings_subject, count(s.subject) as no_of_sub
from subject as s
group by s.subject
order by count(s.subject) desc
limit 10;
````

**23.** Identify the museums which are open on both Sunday and Monday. Display museum name, city.
````sql
select m.name as museum_name, m.city
from museum as m
join museum_hours as mh
on m.museum_id= mh.museum_id
where mh.day in ( 'Sunday', 'Monday')
group by m.name, m.city
having count(mh.day) = 2
order by m.name;
````

**24.** How many museums are open every single day?
````sql
with cte as (select count(mh.museum_id) as total_museum
from museum_hours as mh
group by mh.museum_id
having count(mh.day) = 7)
select count(*) as 'no. of museum' from cte;
````

**25.** Which are the top 5 most popular museum? (Popularity is defined based on most no of paintings in a museum)
````sql
select m.name, count(w.museum_id) as 'no_of_paintings'
from museum as m
join work as w
on w.museum_id= m.museum_id
group by w.museum_id
order by count(w.museum_id) desc
limit 5;
````

**26.** Who are the top 5 most popular artist? (Popularity is defined based on most no of paintings done by an artist)
````sql
select a.full_name as artist, count(w.artist_id) as no_of_painting
from artist as a
join work as w
on w.artist_id= a.artist_id
group by w.artist_id
order by count(w.artist_id) desc
limit 5;
````

**27.** Display the 3 least popular canva sizes
````sql
select c.label, count(p.size_id)
from canvas_size as c
join product_size as p
on c.size_id = p.size_id
group by p.size_id
order by count(p.size_id)
limit 3;
````

**28.** Which museum has the most no of most popular painting style?
````sql
select m.name, w.style, count(w.style) as no_of_paintings
from work as w
join museum as m
on m.museum_id = w.museum_id 
group by m.name, w.style
order by count(w.style) desc
limit 1;
````

**29.** Identify the artists whose paintings are displayed in multiple countries
````sql
select a.full_name, count(m.country) as no_of_country
from artist as a 
join work as w
on w.artist_id = a.artist_id
join museum as m
on m.museum_id = w.museum_id
group by a.full_name
having count(m.country) >2;
````



**30.** Identify the artist and the museum where the most expensive and least expensive painting is placed. Display the artist name, sale_price, painting name, museum name, museum city and canvas label
````sql
(select a.full_name as artist, m.name as museum , max(sale_price) as sale_price, m.city, w.name as painting, 'most_expensive' as remark
from artist as a 
join work as w
on w.artist_id = a.artist_id
join product_size as p
on w.work_id = p.work_id
join museum as m
on m.museum_id = w.museum_id
group by a.full_name, m.name, m.city, w.name
order by max(sale_price)desc
limit 1)
union
(select a.full_name as artist, m.name as museum , min(sale_price) as sale_price, m.city, w.name as painting, 'least_expensive' as remark
from artist as a 
join work as w
on w.artist_id = a.artist_id
join product_size as p
on w.work_id = p.work_id
join museum as m
on m.museum_id = w.museum_id
group by a.full_name, m.name, m.city, w.name
order by min(sale_price) asc
limit 1);
````

**31.** Which country has the 5th highest no of paintings?
````sql
select m.country, count(w.name)as 'no of paintings'
from work as w
join museum as m
on w.museum_id= m.museum_id
group by m.country
order by count(w.name) desc
limit 1 offset 4;
````

**32.** Which are the 3 most popular and 3 least popular painting styles?
````sql
(select w.style as Style, count(w.style) as style_count, 'most_popular' as popularity
from work as w
where w.style is not null
group by w.style
order by count(w.style) asc
limit 3)
union
(select w.style as Style, count(w.style) as style_count, 'least_popular' as popularity
from work as w
where w.style is not null
group by style
order by count(w.style) desc
limit 3);
````


