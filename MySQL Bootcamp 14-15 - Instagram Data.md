---
title: MySQL Bootcamp 14-15 - Instagram Data
created: '2020-02-06T18:58:34.948Z'
modified: '2020-02-06T19:17:59.506Z'
---

# MySQL Bootcamp 14-15 - Instagram Data

* How do you design a schema for something massive like Instagram?

## Creating the Tables

```SQL
CREATE TABLE likes (
    user_id INTEGER NOT NULL,
    photo_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(user_id) REFERENCES users(id),
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    PRIMARY KEY(user_id, photo_id)
);

```
* Good to note that for the 'Likes' to stop people liking it multiple times, you can use `PRIMARY KEY(user_id, photo_id)` so that the user id and photo id combo can only be used once.
