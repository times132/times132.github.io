---
layout: post
title: "[MongoDB] Aggregate"
subtitle: "Aggregate란?"
category: db
tags: db
---
* this unordered seed list will be replaced by the toc
{:toc}

## [MongoDB] Aggregate

- find로 처리하기 어려운 데이터를 가공하기 위한 기능
- pipeline 형식으로 동작하며 **`match, group, sort 등`**다양한 스테이지 값 지원 ([MongoDB Aggregate Stage](https://www.mongodb.com/docs/manual/reference/operator/aggregation-pipeline/))

### RDB와 비교
  
  |   RDB   |  MongoDB  |
  |---------|-----------|
  |where    |$match     |
  |group by |$group     |
  |having   |$match     |
  |select   |$project   |
  |order by |$sort      |
  |limit    |$limit     |
  |sum      |$sum       |
  |count    |$sum       |
  |join     |$lookup    |

### 예시
- data
    ```json
    [{
        "food": {
          "name": "바나나",
          "price": 10000
        },
        "food": {
          "name": "사과",
          "price": 5000
        },
        "food": {
          "name": "딸기",
          "price": 7000
        },
        ----------------------------
        "food": {
          "name": "바나나",
          "price": 9000
        },
        "food": {
          "name": "사과",
          "price": 3000
        },
        "food": {
          "name": "딸기",
          "price": 8000
        },
        -----------------------------
        "food": {
          "name": "사과",
          "price": 5500
        },
        "food": {
          "name": "딸기",
          "price": 7500
        },
        "food": {
          "name": "바나나",
          "price": 7700
        }
    }]
    ```

- $match
  ```sql
  db.blog.aggregate([
    { $match: {'food.name': {$eq: '바나나'}}}
  ])
  ```
  ![match_result](/assets/img/post/2023-02-20/match_result.png)

- $sort
  ```sql
  db.blog.aggregate([
    { $match: {'food.name': {$eq: '바나나'}}},
    { $sort: {'food.price': 1}}                 // 1: Asc -1: Desc
  ]);
  ```
  ![sort_result](/assets/img/post/2023-02-20/sort_result.png)
