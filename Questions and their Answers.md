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

