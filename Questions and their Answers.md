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

