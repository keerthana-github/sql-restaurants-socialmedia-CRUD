# SQL CRUD

## Part 1: Restaurants and Reviews

### To create the tables:

To create the Restaurants table which includes type of food (category), the price tier ($ is cheapest and $$$ is priciest), location (neighborhood), opening hours, average rating (based out of five stars), and whether or not it's good for kids, I used the following code:

    CREATE TABLE restaurants (
    id INTEGER PRIMARY KEY, 
    category TEXT, 
    name TEXT, 
    price_tier TEXT, 
    neighborhood TEXT, 
    opening_hours TEXT, 
    average_rating TEXT, 
    good_for_kids BOOELAN);

To create the Reviews table which includes the restaurant's ID in relation to the values provided in the Restaurants table and the reviews for each restaurant (if there are any), I used the following code:
    CREATE TABLE reviews (
    id INTEGER PRIMARY KEY,
    rest_id INTEGER,
    reviews TEXT,
    FOREIGN KEY (rest_id) REFERENCES restaurants(id));

Then I imported the [Restaurant mock data CSV](data/restaurant.csv) and the [Review mock data CSV](data/reviews.csv) into the database by changing the mode to CSV, turning headers on, and creating a temporary table for each CSV.

    CREATE TABLE temp_restaurants (
    id TEXT, 
    category TEXT, 
    name TEXT, 
    price_tier TEXT, 
    neighborhood TEXT, 
    opening_hours TEXT, 
    average_rating TEXT, 
    good_for_kids TEXT);

    CREATE TABLE temp_reviews (
    id TEXT,
    rest_id TEXT,
    reviews TEXT,
    FOREIGN KEY (rest_id) REFERENCES restaurants(id));

Once I created those tables, I used the following code to import the CSV into my temporary tables:
        .import --skip 1 data/restaurant.csv temp_restaurants
        .import --skip 1 data/reviews.csv temp_reviews

I then used the following code snippet to insert the CSV properly into the actual tables, then dropping the temporary ones:
    INSERT INTO restaurants
    (id, category, name, price_tier, neighborhood, opening_hours, average_rating, good_for_kids)
    SELECT * FROM temp_restaurants;

    INSERT INTO reviews
    (id, rest_id, reviews)
    SELECT * FROM temp_reviews;

    DROP TABLE temp_restaurants;
    DROP TABLE temp_reviews;

### Answering the queries:

1. Find all cheap restaurants in a particular neighborhood (pick any neighborhood as an example).
        select * from restaurants where price_tier = '$' and neighborhood = 'MANHATTAN';
2. Find all restaurants in a particular genre (pick any genre as an example) with 3 stars or more, ordered by the number of stars in descending order.
        select * from restaurants where price_tier = '$' and average_rating>2 order by average_rating desc;
3. Find all restaurants that are open now (see hint below).
        select * from restaurants where cast(opening_hours as INT)  = cast(strftime('%H', 'now') as INT);
4. Leave a review for a restaurant (pick any restaurant as an example).
        insert into reviews (rest_id, reviews) values (18, "This sucks");
5. Delete all restaurants that are not good for kids.
        delete from restaurants where good_for_kids = "FALSE";
6. Find the number of restaurants in each NYC neighborhood.
        select neighborhood, count(neighborhood) from restaurants group by neighborhood;

## Part 2: Social Media



    the SQL code to create each of the required tables
    a link to each of the practice CSV data files in the data directory.
    the SQLite code to import the practice CSV data files into the tables.
    the SQL queries that solve each of the tasks you were asked to do. Make it clear which task each query is intended to solve - include the task number and text on the line above the SQL code solution.

CREATE TABLE users (
id INTEGER PRIMARY KEY,
email TEXT,
password TEXT,
handle TEXT);

CREATE TABLE posts (
id INTEGER PRIMARY KEY,
type TEXT,
user TEXT,
sent_to TEXT,
viewed TEXT,
status TEXT,
date DATETIME,
text_shared TEXT,
FOREIGN KEY (user) REFERENCES users(handle));

Write a single SQL query to perform each of the followin tasks:

    Register a new User.
insert into users (email, password, handle) values ("km4855@nyu.edu", "sqlassign", "keerthana")
    Create a new Message sent by a particular User to a particular User (pick any two Users for example).
insert into posts (type, user, sent_to, viewed, status, date, text_shared) values ("messages", "keerthana", "bgillimghamqe", "unviewed", "visible", strftime('%Y-%m-%d %H:%M:%S', datetime('now')), "hello");
    Create a new Story by a particular User (pick any User for example).
insert into posts (type, user, sent_to, status, date, text_shared) values ("stories", "keerthana", "everyone", "visible", strftime('%Y-%m-%d %H:%M:%S', datetime('now')), "this is my story");
    Show the 10 most recent visible Messages and Stories, in order of recency.
select * from posts order by date desc limit 10;
    Show the 10 most recent visible Messages sent by a particular User to a particular User (pick any two Users for example), in order of recency.
select * from posts where type = "messages" and user ="bgillimghamqe" and sent_to = "pstrasse2p"  order by date desc limit 10;
    Make all Stories that are more than 24 hours old invisible.
UPDATE posts set status = "invisible" where type = "stories" and ROUND((JULIANDAY('now') - JULIANDAY(date)) * 60) >= 24;
    Show all invisible Messages and Stories, in order of recency.
select * from posts where status = "invisible" order by date desc;
    Show the number of posts by each User.
select user, count(user) from posts group by user order by count(user) desc;
    Show the post text and email address of all posts and the User who made them within the last 24 hours.
select users.handle, users.email, posts.text_shared from posts left join users on users.handle=posts.user where ROUND((JULIANDAY('now') - JULIANDAY(date)) * 60) <= 24;
    Show the email addresses of all Users who have not posted anything yet.
select users.handle, users.email from users left join posts on users.handle=posts.user where posts.user IS NULL;