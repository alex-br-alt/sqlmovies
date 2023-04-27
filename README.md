# sqlmovies
SQL queries to search for results from a movie database

-- list the titles of all movies released in 2008 

SELECT title FROM movies WHERE year = 2008;

-- query to determine the birth year of Emma Stone

 SELECT birth FROM people WHERE name = 'Emma Stone';
 
--  list the titles of all movies with a release date on or after 2018, in alphabetical order.

SELECT title FROM movies WHERE year >= 2018 ORDER BY title ASC;

-- determine the number of movies with an IMDb rating of 10.0.

SELECT COUNT (rating) FROM ratings WHERE rating = 10.0;

--write a SQL query to list the titles and release years of all Harry Potter movies, in chronological order.

SELECT title, year FROM movies WHERE title  LIKE 'Harry Potter%' ORDER BY title ASC;

--write a SQL query to determine the average rating of all movies released in 2012.

SELECT AVG(rating) FROM ratings WHERE movie_id IN (SELECT id FROM movies WHERE year = 2012);

--query to list all movies released in 2010 and their ratings, in descending order by rating. For movies with the same rating, order them alphabetically by title.

SELECT title, rating FROM movies INNER JOIN ratings ON (id = movie_id) WHERE year = 2010 ORDER BY rating DESC, title ASC;

-- list the names of all people who starred in Toy Story.

SELECT name FROM people WHERE id IN (SELECT person_id FROM stars WHERE movie_id = (SELECT id FROM movies WHERE title = 'Toy Story'));

--list the names of all people who starred in a movie released in 2004, ordered by birth year.

 SELECT name FROM people WHERE id IN (SELECT person_id FROM stars WHERE movie_id IN (SELECT id FROM movies WHERE year = 2004)) ORDER BY birth ASC;
 
--list the names of all people who have directed a movie that received a rating of at least 9.0

SELECT name FROM people WHERE id IN (SELECT person_id FROM directors WHERE movie_id IN (SELECT movie_id FROM ratings WHERE rating >= 9.0));

--list the titles of the five highest rated movies (in order) that Chadwick Boseman starred in, starting with the highest rated.

SELECT title FROM movies JOIN ratings ON ratings.movie_id = movies.id JOIN stars ON stars.movie_id = movies.id JOIN people ON stars.person_id = people.id WHERE people.name = "Chadwick Boseman" ORDER BY ratings.rating DESC Limit 5;

--list the titles of all movies in which both Johnny Depp and Helena Bonham Carter starred.

SElECT title FROM movies
JOIN stars ON stars.movie_id = movies.id
JOIN people ON people.id = stars.person_id
WHERE people.name = 'Johnny Depp'
AND title IN
(SELECT title FROM movies
JOIN stars ON stars.movie_id = movies.id
JOIN people ON people.id = stars.person_id
WHERE people.name = 'Helena Bonham Carter'
);

--query to list the names of all people who starred in a movie in which Kevin Bacon also starred.

SELECT DISTINCT name FROM people
JOIN stars ON stars.person_id = people.id
JOIN movies ON movies.id = stars.movie_id
WHERE movies.id IN
(SELECT movies.id FROM movies
JOIN stars ON stars.movie_id = movies.id
JOIN people ON people.id = stars.person_id
WHERE people.name = 'Kevin Bacon' AND people.birth = '1958')
AND people.name != 'Kevin Bacon';

Assignment via CS50: https://pll.harvard.edu/course/cs50-introduction-computer-science?delta=0

origional database provided to course via https://www.imdb.com/
