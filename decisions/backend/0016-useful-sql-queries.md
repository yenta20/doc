# 16. Useful SQL queries.

Date: 2021-04-22

## Status

In progress

## Context

Useful analytical queries.

## SQL queries

- Top 100 followers per active user:
```postgresql
WITH aUsers AS (
    SELECT follower_id AS u_id
    FROM user_follower
    WHERE status = 'accepted'
    UNION
    SELECT user_id AS u_id
    FROM review
    WHERE ready
)
SELECT u.id, u.nick, u.name, count(*) AS followers
FROM user_follower f
         JOIN user_profile u on f.user_id = u.id
WHERE f.user_id IN (SELECT u_id FROM aUsers)
GROUP BY u.id, u.nick
ORDER BY count(*) DESC
LIMIT 100;
```

- Top 100 reviews (POI + Kitchen) per active user:
```postgresql
WITH aUsers AS (
    SELECT follower_id AS u_id
    FROM user_follower
    WHERE status = 'accepted'
    UNION
    SELECT user_id AS u_id
    FROM review
    WHERE ready
)
SELECT u.id,
       u.nick,
       sum(case r.is_kitchen when false then 1 else 0 end) AS poi_count,
       sum(case r.is_kitchen when true then 1 else 0 end)  AS kitchen_count
FROM review r
         JOIN user_profile u on r.user_id = u.id
WHERE r.user_id IN (SELECT u_id FROM aUsers)
GROUP BY u.id, u.nick
ORDER BY poi_count DESC, kitchen_count DESC
LIMIT 100;
```
