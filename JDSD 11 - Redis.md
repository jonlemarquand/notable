---
title: JDSD 11 - Redis
created: '2020-05-03T07:36:04.700Z'
modified: '2020-05-03T08:29:02.502Z'
---

# JDSD 11 - Redis

## Section Overview

* Redis is a NoSQL, in-memory database
* Key-Value storage
* In memory makes it really, really fast.
  * Its often used for sessions and web page hit counts.
  * Small pieces of data which allows us to keep it in the machine's memory rather than disk.
  * You also don't care if you lose some of that data
* In most companies, they will be using different databases depending on the needs.

## Installing Redis

* Download from redis.io
* run redis with `$ src/redis-server`
* `cd redis-4.0.9` then `src/redis-cli` to connect to the running server in terminal.

## Redis Commands

* You want to look at what kind of datatypes your database can support.
* For Redis, it is a key-value storage.
  * We give it a key and a value and then can access that value from the key.
  * `SET name "Godzilla"` and you can then say `GET name` to return Godzilla.
  * You can also use `EXISTS name` and `DEL name`
* `SET session "Jenny"` and then `EXPIRE session 10` this will make the session expire in 10 seconds (be deleted from the database).
* `SET counter 1000` and then `INCRBY counter 33` - Our counter will now be at 1033.
  * There is also the `DECR counter` which will decrement by 1.

## Redis Data Types

* `MSET a 2 b 5` - This will be a multiple set where a = 2 and b = 5.
* `MGET a b` - Will get both a and b.
* Even if you set a number, redis will return a string.
* 5 Datatypes:
  * Strings
  * Hashes
  * Listed
  * Sets
  * Sorted Sets

### Redis Hashes

* Hashes are maps between string fields and string values.
  * Think of them like javascript objects.
* `HMSET user id 45 name "John"`
  * We have just created a hash with key user and id of 45 and name of John.
  * `HGET user id` would return 45.
  * `HGETALL user`

### Redis Lists

* Lists are useful for long lists of data.
  * `LPUSH ourlist 10` and then `RPUSH ourlist "hello"` and then `LPUSH ourlist 55`.
  * If we then `LRANGE ourlist 0 1`: It would return "55" and "10"
  * Can use `LTRIM` or 
  * `RPOP ourlist` will remove "hello" from the list.

### Redis Sets + Sorted Sets

* Sets are collections of strings.
  * You don't allow repeated elements in this datatype.
    * It would give you a single copy.
* `SADD ourset 1 2 3 4 5`
  * `SMEMBERS ourset` will return the values.
  * `SADD ourset 1 2 3 4` and then output would just return the original values.
  * `SISMEMBER ourset 5` - Will return 1 for true, 0 for false.
* Sorted sets
  * No repeating collection of strings, but every member of a sorted set is associated with a score.
  * `ZADD team 50 "Wizards"` and then `ZADD team 40 "Cavaliers"`
  * If we do `ZRANGE team 0 1` it will return Cavaliers first because the team number is smaller.
  * `ZRANK team "Wizards"` will return their 'rank'.


