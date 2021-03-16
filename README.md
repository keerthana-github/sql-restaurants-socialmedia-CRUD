# SQL CRUD

## Part 1: Restaurants and Reviews

### To create the table

the SQL code to create each of the required tables

CREATE TABLE restaurants (
id INTEGER PRIMARY KEY, category TEXT, name TEXT, price_tier TEXT, neighborhood TEXT, opening_hours TEXT, average_rating TEXT, good_for_kids BOOELAN);
CREATE TABLE temp (
id TEXT, rest_id TEXT, reviews TEXT);
CREATE TABLE reviews (
id INTEGER PRIMARY KEY,
rest_id INTEGER,
reviews TEXT,
FOREIGN KEY (rest_id) REFERENCES restaurants(id));

id TEXT,category TEXT, name TEXT, price_tier TEXT, neighborhood TEXT, opening_hours TEXT, average_rating TEXT, good_for_kids TEXT);
(id,category,name,price_tier,neighborhood,opening_hours,average_rating,good_for_kids)
id INTEGER PRIMARY KEY, category TEXT, name TEXT, price_tier TEXT, neighborhood TEXT, opening_hours TEXT, average_rating TEXT, good_for_kids BOOELAN);
FOREIGN KEY (rest_id) REFERENCES restaurant(id));
a link to each of the practice CSV data files in the data directory.
the SQLite code to import the practice CSV data files into the tables.
the SQL queries that solve each of the tasks you were asked to do. Make it clear which task each query is intended to solve - include the task number and text on the line above the SQL code solution
Write a single SQL query to perform each of the following tasks:
    Find all cheap restaurants in a particular neighborhood (pick any neighborhood as an example).
select * from restaurants where price_tier = '$' and neighborhood = 'MANHATTAN';
    Find all restaurants in a particular genre (pick any genre as an example) with 3 stars or more, ordered by the number of stars in descending order.
select * from restaurants where price_tier = '$' and average_rating>2 order by average_rating desc;
    Find all restaurants that are open now (see hint below).
select * from restaurants where cast(opening_hours as INT)  = cast(strftime('%H', 'now') as INT);
    Leave a review for a restaurant (pick any restaurant as an example).
insert into reviews (rest_id, reviews) values (18, "This sucks");
    Delete all restaurants that are not good for kids.
delete from restaurants where good_for_kids = "FALSE";
    Find the number of restaurants in each NYC neighborhood.
select neighborhood, count(neighborhood) from restaurants group by neighborhood;

