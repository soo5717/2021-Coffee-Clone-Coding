# spring-clone-coding

## 1. 프로젝트 소개

> ☕ 매일 전날 오후 2시부터 당일 오후 2시까지의 커피 패키지 주문을 모아서 한 번에 처리하는 쇼핑몰의 **커피 주문 및 커피 주문 관리 API**를 구현하는 프로젝트입니다.

- 회원의 경우 별도로 관리하지 않고 주문할 때 입력 받는 이메일을 통해서 고객을 구분합니다.
- 동일한 이메일로 하루에 여러번 주문을 받더라도 주문을 하나로 합쳐서 다음날 배송을 보냅니다.

<br>

## 2. 주요 기능

[**관리자] 주문 관리**

- 상품 조회, 추가, 수정, 삭제 `CRUD`

[**고객] 주문**

- 상품 조회, 주문 `CR`

<br>

## 3. 개발 환경

- Window 10
- IntelliJ 2021.2.2
- Open JDK 17
- Spring Boot 2.5.5
- MySQL 8.0.26

<br>

## 4. 시스템 구성도

<p align="center"><img src="https://user-images.githubusercontent.com/54765850/141698518-7893ffd1-9328-4557-8007-93d0994a570c.png" height="80%" width="80%"></p>

<br>

## 5. 데이터베이스 설계

총 3개의 `products`, `order`, `order_items` 테이블로 구성되어 있습니다. 아래의 ERD에서 볼 수 있듯이 각 테이블 간에는 1:N, N:1의 관계를 가지고 있습니다.

<p align="center"><img src="https://user-images.githubusercontent.com/54765850/141698526-9f3b86c5-89f8-408e-be39-0d213a1e8749.png"></p>

- `Products` - 판매할 상품에 대한 정보를 담고 있는 테이블
- `Order` - 고객의 주문 정보를 담고 있는 테이블
- `Order Item` - 고객이 주문한 상품에 대한 정보를 담고 있는 테이블

```sql
CREATE TABLE products
(
    product_id   BINARY(16) PRIMARY KEY,
    product_name VARCHAR(20) NOT NULL,
    category     VARCHAR(50) NOT NULL,
    price        bigint      NOT NULL,
    description  VARCHAR(500) DEFAULT NULL,
    created_at   datetime(6) NOT NULL,
    updated_at   datetime(6)  DEFAULT NULL
);

CREATE TABLE orders
(
    order_id     binary(16) PRIMARY KEY,
    email        VARCHAR(50)  NOT NULL,
    address      VARCHAR(200) NOT NULL,
    postcode     VARCHAR(200) NOT NULL,
    order_status VARCHAR(50)  NOT NULL,
    created_at   datetime(6)  NOT NULL,
    updated_at   datetime(6) DEFAULT NULL
);

CREATE TABLE order_items
(
    seq        bigint      NOT NULL PRIMARY KEY AUTO_INCREMENT,
    order_id   binary(16)  NOT NULL,
    product_id binary(16)  NOT NULL,
    category   VARCHAR(50) NOT NULL,
    price      bigint      NOT NULL,
    quantity   int         NOT NULL,
    created_at datetime(6) NOT NULL,
    updated_at datetime(6) DEFAULT NULL,
    INDEX (order_id),
    CONSTRAINT fk_order_items_to_order FOREIGN KEY (order_id) REFERENCES orders (order_id) ON DELETE CASCADE,
    CONSTRAINT fk_order_items_to_product FOREIGN KEY (product_id) REFERENCES products (product_id)
);
```

<br>

## 6. 프로젝트 구조

`Configuration`, `Model`, `Controller`, `Service`, `Repository`로 패키지를 구분하였습니다. 

```bash
├─main
│  ├─java
│  │  └─com
│  │      └─example
│  │          └─gccoffee
│  │              ├─configuration
│  │              ├─controller
│  │              │  └─api
│  │              ├─model
│  │              ├─repository
│  │              └─service
│  └─resources
│      ├─static
│      └─templates
└─test
    ├─java
    │  └─com
    │      └─example
    │          └─gccoffee
    │              ├─model
    │              └─repository
    └─resources
```

<br>

## 7. 향후 계획

- 관리자, 고객 웹 사이트 `CRUD` 구현 완료
- `swagger`를 활용한 문서화
- 코드와 구조적인 개선
