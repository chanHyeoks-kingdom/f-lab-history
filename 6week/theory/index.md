# index

<br>

#### Q1. 인데스란 뭘까?

> ###### 데이터 검색 속도를 높이기 위해 '데이터'나 '별도의 
```
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    last_name VARCHAR(50),
    first_name VARCHAR(50)
);


CREATE INDEX idx_last_name ON users(last_name);
INSERT INTO users (user_id, last_name, first_name) VALUES (100, 'Smith', 'fuck');

SELECT * FROM users WHERE last_name = 'Smith';
```

