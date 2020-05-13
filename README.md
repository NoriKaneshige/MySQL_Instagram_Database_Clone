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
## Instagram Challenge (1)
![](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/.png)
```
```
## Instagram Challenge (2)
![](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/.png)
```
```
## Instagram Challenge (3)
![](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/.png)
```
```
## Instagram Challenge (4)
![](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/.png)
```
```
## Instagram Challenge (5)
![](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/.png)
```
```
## Instagram Challenge (6)
![](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/.png)
```
```
## Instagram Challenge (7)
![](https://github.com/NoriKaneshige/MySQL_Instagram_Database_Clone/blob/master/.png)
```
```

