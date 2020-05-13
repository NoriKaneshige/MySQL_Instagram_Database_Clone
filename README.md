# MySQL_Instagram_Database_Clone
![instagram_schema_components](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/instagram_schema_components.png)
## Users
![users](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/users.png)
```
DROP DATABASE ig_clone;
CREATE DATABASE ig_clone;
USE ig_clone

CREATE TABLE users (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

mysql> DESC users;
+------------+--------------+------+-----+-------------------+----------------+
| Field      | Type         | Null | Key | Default           | Extra          |
+------------+--------------+------+-----+-------------------+----------------+
| id         | int(11)      | NO   | PRI | NULL              | auto_increment |
| username   | varchar(255) | NO   | UNI | NULL              |                |
| created_at | timestamp    | NO   |     | CURRENT_TIMESTAMP |                |
+------------+--------------+------+-----+-------------------+----------------+
3 rows in set (0.03 sec)
```

## Photos, IG Clone Photos Schema
![photos](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/photos.png)
```
CREATE TABLE photos (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    image_url VARCHAR(255) NOT NULL,
    user_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(user_id) REFERENCES users(id)
);

mysql> DESC photos;
+------------+--------------+------+-----+-------------------+----------------+
| Field      | Type         | Null | Key | Default           | Extra          |
+------------+--------------+------+-----+-------------------+----------------+
| id         | int(11)      | NO   | PRI | NULL              | auto_increment |
| image_url  | varchar(255) | NO   |     | NULL              |                |
| user_id    | int(11)      | NO   | MUL | NULL              |                |
| created_at | timestamp    | NO   |     | CURRENT_TIMESTAMP |                |
+------------+--------------+------+-----+-------------------+----------------+
4 rows in set (0.01 sec)
```
## Comments, IG Clone Comments Schema
![comments](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/comments.png)
```
CREATE TABLE comments (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    comment_text VARCHAR(255) NOT NULL,
    photo_id INTEGER NOT NULL,
    user_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    FOREIGN KEY(user_id) REFERENCES users(id)
);

mysql> DESC comments;
+--------------+--------------+------+-----+-------------------+----------------+
| Field        | Type         | Null | Key | Default           | Extra          |
+--------------+--------------+------+-----+-------------------+----------------+
| id           | int(11)      | NO   | PRI | NULL              | auto_increment |
| comment_text | varchar(255) | NO   |     | NULL              |                |
| photo_id     | int(11)      | NO   | MUL | NULL              |                |
| user_id      | int(11)      | NO   | MUL | NULL              |                |
| created_at   | timestamp    | NO   |     | CURRENT_TIMESTAMP |                |
+--------------+--------------+------+-----+-------------------+----------------+
5 rows in set (0.00 sec)
```
## Likes, IG Clone Likes Schema
![likes](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/likes.png)
```
CREATE TABLE likes (
    user_id INTEGER NOT NULL,
    photo_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(user_id) REFERENCES users(id),
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    PRIMARY KEY(user_id, photo_id)
);
# Note: PRIMARY KEY(user_id, photo_id)
# This is to prohibit multiple Likes per photo

mysql> DESC likes;
+------------+-----------+------+-----+-------------------+-------+
| Field      | Type      | Null | Key | Default           | Extra |
+------------+-----------+------+-----+-------------------+-------+
| user_id    | int(11)   | NO   | PRI | NULL              |       |
| photo_id   | int(11)   | NO   | PRI | NULL              |       |
| created_at | timestamp | NO   |     | CURRENT_TIMESTAMP |       |
+------------+-----------+------+-----+-------------------+-------+
3 rows in set (0.00 sec)
```
## Followers, IG Clone Followers Schema
![followers](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/followers.png)
![followers_ex](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/followers_ex.png)
```
CREATE TABLE follows (
    follower_id INTEGER NOT NULL,
    followee_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(follower_id) REFERENCES users(id),
    FOREIGN KEY(followee_id) REFERENCES users(id),
    PRIMARY KEY(follower_id, followee_id)
);

mysql> DESC follows;
+-------------+-----------+------+-----+-------------------+-------+
| Field       | Type      | Null | Key | Default           | Extra |
+-------------+-----------+------+-----+-------------------+-------+
| follower_id | int(11)   | NO   | PRI | NULL              |       |
| followee_id | int(11)   | NO   | PRI | NULL              |       |
| created_at  | timestamp | NO   |     | CURRENT_TIMESTAMP |       |
+-------------+-----------+------+-----+-------------------+-------+
3 rows in set (0.00 sec)
```
## Hashtags, IG Clone Hashtags Schema
![hashtags](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/hashtags.png)
```
CREATE TABLE tags (
  id INTEGER AUTO_INCREMENT PRIMARY KEY,
  tag_name VARCHAR(255) UNIQUE,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE photo_tags (
    photo_id INTEGER NOT NULL,
    tag_id INTEGER NOT NULL,
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    FOREIGN KEY(tag_id) REFERENCES tags(id),
    PRIMARY KEY(photo_id, tag_id)
);

mysql> DESC tags;
+------------+--------------+------+-----+-------------------+----------------+
| Field      | Type         | Null | Key | Default           | Extra          |
+------------+--------------+------+-----+-------------------+----------------+
| id         | int(11)      | NO   | PRI | NULL              | auto_increment |
| tag_name   | varchar(255) | YES  | UNI | NULL              |                |
| created_at | timestamp    | NO   |     | CURRENT_TIMESTAMP |                |
+------------+--------------+------+-----+-------------------+----------------+
3 rows in set (0.00 sec)

mysql> DESC photo_tags;
+----------+---------+------+-----+---------+-------+
| Field    | Type    | Null | Key | Default | Extra |
+----------+---------+------+-----+---------+-------+
| photo_id | int(11) | NO   | PRI | NULL    |       |
| tag_id   | int(11) | NO   | PRI | NULL    |       |
+----------+---------+------+-----+---------+-------+
2 rows in set (0.03 sec)
```
## Loading Dataset, look at ig_clone_data.sql
![ig_clone_data](ig_clone_data.gif)
## Instagram Database Challenge (1)
![instagram_challenge_1](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/instagram_challenge_1.png)
```
mysql> SELECT *
    -> FROM users
    -> ORDER BY created_at
    -> LIMIT 5;
+----+------------------+---------------------+
| id | username         | created_at          |
+----+------------------+---------------------+
| 80 | Darby_Herzog     | 2016-05-06 00:14:21 |
| 67 | Emilio_Bernier52 | 2016-05-06 13:04:30 |
| 63 | Elenor88         | 2016-05-08 01:30:41 |
| 95 | Nicole71         | 2016-05-09 17:30:22 |
| 38 | Jordyn.Jacobson2 | 2016-05-14 07:56:26 |
+----+------------------+---------------------+
5 rows in set (0.12 sec)
```
## Instagram Database Challenge (2)
![instagram_challenge_2](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/instagram_challenge_2.png)
```
mysql> SELECT
    ->     DAYNAME(created_at) AS day,
    ->     COUNT(*) AS total
    -> FROM users
    -> GROUP BY day
    -> ORDER BY total DESC
    -> LIMIT 2;
+----------+-------+
| day      | total |
+----------+-------+
| Thursday |    16 |
| Sunday   |    16 |
+----------+-------+
2 rows in set (0.03 sec)
```
## Instagram Database Challenge (3)
![instagram_challenge_3](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/instagram_challenge_3.png)
```
mysql> SELECT username
    -> FROM users
    -> LEFT JOIN photos
    ->     ON users.id = photos.user_id
    -> WHERE photos.id IS NULL;
+---------------------+
| username            |
+---------------------+
| Aniya_Hackett       |
| Bartholome.Bernhard |
| Bethany20           |
| Darby_Herzog        |
| David.Osinski47     |
| Duane60             |
| Esmeralda.Mraz57    |
| Esther.Zulauf61     |
| Franco_Keebler64    |
| Hulda.Macejkovic    |
| Jaclyn81            |
| Janelle.Nikolaus81  |
| Jessyca_West        |
| Julien_Schmidt      |
| Kasandra_Homenick   |
| Leslie67            |
| Linnea59            |
| Maxwell.Halvorson   |
| Mckenna17           |
| Mike.Auer39         |
| Morgan.Kassulke     |
| Nia_Haag            |
| Ollie_Ledner37      |
| Pearl7              |
| Rocio33             |
| Tierra.Trantow      |
+---------------------+
26 rows in set (0.00 sec)
```
## Instagram Database Challenge (4)
![instagram_challenge_4](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/instagram_challenge_4.png)
```
mysql> SELECT
    ->     username,
    ->     photos.id,
    ->     photos.image_url,
    ->     COUNT(*) AS total
    -> FROM photos
    -> INNER JOIN likes
    ->     ON likes.photo_id = photos.id
    -> INNER JOIN users
    ->     ON photos.user_id = users.id
    -> GROUP BY photos.id
    -> ORDER BY total DESC
    -> LIMIT 1;
+---------------+-----+---------------------+-------+
| username      | id  | image_url           | total |
+---------------+-----+---------------------+-------+
| Zack_Kemmer93 | 145 | https://jarret.name |    48 |
+---------------+-----+---------------------+-------+
1 row in set (0.16 sec)
```
## Instagram Database Challenge (5)
![instagram_challenge_5](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/instagram_challenge_5.png)
```
mysql> SELECT (SELECT
    ->     COUNT(*)
    ->   FROM photos)
    ->   / (SELECT
    ->     COUNT(*)
    ->   FROM users)
    ->   AS avg;
+--------+
| avg    |
+--------+
| 2.5700 |
+--------+
1 row in set (0.06 sec)
```
## Instagram Database Challenge (6)
![instagram_challenge_6](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/instagram_challenge_6.png)
```
mysql> SELECT tags.tag_name,
    ->        Count(*) AS total
    -> FROM   photo_tags
    ->        JOIN tags
    ->          ON photo_tags.tag_id = tags.id
    -> GROUP  BY tags.id
    -> ORDER  BY total DESC
    -> LIMIT  5;
+----------+-------+
| tag_name | total |
+----------+-------+
| smile    |    59 |
| beach    |    42 |
| party    |    39 |
| fun      |    38 |
| lol      |    24 |
+----------+-------+
5 rows in set (0.00 sec)
```
## Instagram Database Challenge (7)
![instagram_challenge_7](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/instagram_challenge_7.png)
```
mysql> SELECT username,
    ->        Count(*) AS num_likes
    -> FROM   users
    ->        INNER JOIN likes
    ->                ON users.id = likes.user_id
    -> GROUP  BY likes.user_id
    -> HAVING num_likes = (SELECT Count(*)
    ->                     FROM   photos);
+--------------------+-----------+
| username           | num_likes |
+--------------------+-----------+
| Aniya_Hackett      |       257 |
| Jaclyn81           |       257 |
| Rocio33            |       257 |
| Maxwell.Halvorson  |       257 |
| Ollie_Ledner37     |       257 |
| Mckenna17          |       257 |
| Duane60            |       257 |
| Julien_Schmidt     |       257 |
| Mike.Auer39        |       257 |
| Nia_Haag           |       257 |
| Leslie67           |       257 |
| Janelle.Nikolaus81 |       257 |
| Bethany20          |       257 |
+--------------------+-----------+
13 rows in set (0.01 sec)
```

