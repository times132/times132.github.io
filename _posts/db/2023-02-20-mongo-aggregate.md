---
layout: post
title: "[MongoDB] Aggregate"
subtitle: "Aggregate란?"
category: db
tags: db
---

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
<details open>
<summary>data</summary>

<div markdown="1">

  ```json
  [{
      "food": {
        "name": "바나나",
        "price": 10000,
        "location": {
          "address": "강남",
          "zip": "12345"
        }
      },
      "food": {
        "name": "사과",
        "price": 5000,
        "location": {
          "address": "교대",
          "zip": "23454"
        }
      },
      "food": {
        "name": "딸기",
        "price": 7000,
        "location": {
          "address": "사당",
          "zip": "42231"
        }
      },
      ----------------------------
      "food": {
        "name": "바나나",
        "price": 9000,
        "location": {
          "address": "논현",
          "zip": "49893"
        }
      },
      "food": {
        "name": "사과",
        "price": 3000,
        "location": {
          "address": "강남",
          "zip": "23431"
        }
      },
      "food": {
        "name": "딸기",
        "price": 8000,
        "location": {
          "address": "강남",
          "zip": "23431"
        }
      }
  }]
  ```
</div>
</details>

#### $project
  - 필드 포함/제외, 새 필드 추가, 기존 필드 재설정 등 기존의 값으로 출력을 재설정


#### $match
  - 지정한 조건과 일치하는 값이 나오도록 필터링 역할을 함
  - $eq, $in, $and, $gt 등 다양한 비교연산자 지원
  ```sql
  db.blog.aggregate([
    { $match: {'food.name': {$eq: '바나나'}}}
  ])
  ```
  ![match_result](/assets/img/post/2023-02-20/match_result.png)

#### $sort

  ```sql
  db.blog.aggregate([
    { $match: {'food.name': {$eq: '바나나'}}},
    { $sort: {'food.price': 1}}                 // 1: Asc -1: Desc
  ]);
  ```
  ![sort_result](/assets/img/post/2023-02-20/sort_result.png)
