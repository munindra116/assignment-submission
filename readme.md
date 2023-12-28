# Full Stack Application with Spring Boot and React

![Architecture](react_00_architecture.png)



## Running the Application

-
## Deploying Front End to AWS

### Change /frontend/todo-app/src/Constants.js

```

//export const API_URL = 'http://localhost:5000'
export const API_URL = 'http://restfulwebservices-env.uhpev7xzpb.us-east-1.elasticbeanstalk.com'
```

### Create Production Build

`npm run build`


### S3 Access for Static Content

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AddPerm",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::fullstack-frontend/*"
        }
    ]
}
```

## Authentication

All REST API are protected by JWT Authentication with Spring Security. 

POST to http://localhost:5000/authenticate

```
{
  "username":"munindra",
  "password":"dummy"
}
```

Response
```
{
"token": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJpbjI4bWludXRlcyIsImV4cCI6MTU2MjIzNDM1OSwiaWF0IjoxNTYxNjI5NTU5fQ.yvkFtYAp8yGClDo7D5wtXyPSnUPtxu8A7A9YCl9FJdjR0di_yAaPcSTR6liN5bIu1SnOJuSZp94pYSYzU_BNEw"
}
```

Use the token in the headers for all subsequent requests.

`Authorization` : `Bearer ${token}`

Example 

`Authorization` : `Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJpbjI4bWludXRlcyIsImV4cCI6MTU2MjIzNDM1OSwiaWF0IjoxNTYxNjI5NTU5fQ.yvkFtYAp8yGClDo7D5wtXyPSnUPtxu8A7A9YCl9FJdjR0di_yAaPcSTR6liN5bIu1SnOJuSZp94pYSYzU_BNEw`


## Creating New Users

Look at /src/main/resources/data.sql for existing users.

You can create new users by encrypting password with Bcrypt - Use Rounds 10 - https://www.browserling.com/tools/bcrypt 

```
INSERT INTO USER (ID, USERNAME, PASSWORD, ROLE) 
VALUES (3, 'USERNAME', 'BCRYPT_ENCRyPTED_PASSWORD','ROLE_USER');
```


## Hello World URLS

- http://localhost:5000/hello-world

```txt
Hello World
```

- http://localhost:5000/hello-world-bean

```json
{"message":"Hello World - Changed"}
```

- http://localhost:5000/hello-world/path-variable/munindra

```json
{"message":"Hello World, munindra"}
```

## TODO Resource Details

- GET - http://localhost:5000/jpa/users/munindra/todos

```
[
  {
    "id": 10001,
    "username": "munindra",
    "description": "Learn JPA",
    "targetDate": "2019-06-27T06:30:30.696+0000",
    "done": false
  },
  {
    "id": 10002,
    "username": "munindra",
    "description": "Learn Data JPA",
    "targetDate": "2019-06-27T06:30:30.700+0000",
    "done": false
  },
  {
    "id": 10003,
    "username": "munindra",
    "description": "Learn Microservices",
    "targetDate": "2019-06-27T06:30:30.701+0000",
    "done": false
  }
]
```

#### Retrieve a specific todo

- GET - http://localhost:5000/jpa/users/munindra/todos/10001

```
{
  "id": 10001,
  "username": "munindra",
  "description": "Learn JPA",
  "targetDate": "2019-06-27T06:30:30.696+0000",
  "done": false
}
```

#### Creating a new todo

- POST to http://localhost:5000/jpa/users/munindra/todos with BODY of Request given below

```
{
  "username": "munindra",
  "description": "Learn to Drive a Car",
  "targetDate": "2030-11-09T10:49:23.566+0000",
  "done": false
}
```

#### Updating a new todo

- http://localhost:5000/jpa/users/munindra/todos/10001 with BODY of Request given below

```
{
  "id": 10001,
  "username": "munindra",
  "description": "Learn to Drive a Car",
  "targetDate": "2045-11-09T10:49:23.566+0000",
  "done": false
}
```

#### Delete todo

- DELETE to http://localhost:5000/jpa/users/munindra/todos/10001

## H2 Schema - Created by Spring Boot Auto Configuration

```
Hibernate: drop table todo if exists
Hibernate: drop table user if exists
Hibernate: drop sequence if exists hibernate_sequence
Hibernate: drop sequence if exists user_seq
Hibernate: create sequence hibernate_sequence start with 1 increment by 1
Hibernate: create sequence user_seq start with 1 increment by 1
Hibernate: create table todo (id bigint not null, description varchar(255), is_done boolean not null, target_date timestamp, username varchar(255), primary key (id))
Hibernate: create table user (id bigint not null, password varchar(100) not null, role varchar(100) not null, username varchar(50) not null, primary key (id))
Hibernate: alter table user add constraint UK_sb8bbouer5wak8vyiiy4pf2bx unique (username)
```


## H2 Console

- http://localhost:5000/h2-console
- Use `jdbc:h2:mem:testdb` as JDBC URL 

