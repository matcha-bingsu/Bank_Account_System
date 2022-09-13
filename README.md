# Bank Account System

# 개요

- C + mysql 를 사용한 은행 관리 시스템을 구현
- 로그인, 회원가입, 입금, 출금, 이체 기능 구현
- 프로그램을 동작하면 로그인을 하고 관리자면 관리자 페이지를, 고객이면 은행 서비스 페이지를 보여준다.

![Untitled](https://user-images.githubusercontent.com/78288539/189816381-8f9fad38-78e3-4bb8-aa14-ce664481e4d4.png)

## 동작 화면
<p align="center"><img src="https://user-images.githubusercontent.com/78288539/189819643-52f30690-ab77-47ed-ac28-cbbc6d85da3c.gif" width="60%" height="60%"/></p>

### 메뉴

<p align="center"><img src="https://user-images.githubusercontent.com/78288539/189817365-c98bd932-2101-4a04-b402-9466123f8f1c.png" width="70%" height="70%"/></p>

### 로그인

<p align="center"><img src="https://user-images.githubusercontent.com/78288539/189817948-114b90c6-1248-4e82-8784-fa5978d88d22.png" width="70%" height="70%"/></p>

### 회원가입

<p align="center"><img src="https://user-images.githubusercontent.com/78288539/189818109-72176552-1616-41fc-a826-e08edd4833a3.png" width="70%" height="70%"/></p>

위와 같이 회원가입이 DB 회원 정보에 새로운 계정이 추가됨

<p align="center"><img src="https://user-images.githubusercontent.com/78288539/189818120-406b2097-178f-49b4-b335-7081238f844f.png" width="70%" height="70%"/></p>

# DB

- 데이터는 `mysql` 를 통해 데이터를 저장하고 다운받는다.
- 클라이언트와 서버가 구분되어 있지 않기 때문에 프로그램 시작 시 `mysql` 를 `root` 계정으로 로그인하고 프로그램 사용자가 데이터를 요구하면 `root` 계정 권한으로 DB에서 데이터를 가져온다.
- 초기설정
    - DB 생성
    
    ```sql
    CREATE DATABASE bank;
    ```
    
    - User 테이블 생성
    
    ```sql
      CREATE TABLE User (
    	userNum int not null AUTO_INCREMENT,
            id varchar(20) not null,
    	password varchar(20) not null,
    	name varchar(20),
    	age int,
    	email varchar(45),
    	manager tinyint(1) default 0,
            PRIMARY KEY (userNum)
    );
    ```
    
    - Account 테이블 생성
    
    ```sql
    CREATE TABLE Account 
    (
    	account_num    varchar(20) NOT NULL,
            balance INT DEFAULT 0,
    	update_date DATETIME default NOW(),
    	create_date DATETIME default NOW(),
            user_num INT,
    	PRIMARY KEY (account_num),
    	FOREIGN KEY (user_num) REFERENCES User(user_num)
    )
    ```
    
    - Transaction 테이블 생성
    
    ```sql
    CREATE TABLE Transaction 
    (
    	transaction_num INT not null AUTO_INCREMENT,
    	type varchar(20) NOT NULL,
            amount INT DEFAULT 0,
            balance INT NOT NULL,
    	client varchar(20),
    	date DATETIME default NOW(),
    	detail varchar(45),
            account_num varchar(20) NOT NULL,
    	PRIMARY KEY (transaction_num),
    	FOREIGN KEY (account_num) REFERENCES Account(account_num)
    )
    ```
    

### User

<p align="center"><img src="https://user-images.githubusercontent.com/78288539/189818167-10d6dc5d-546e-4597-b770-d8e9869e2dbe.png" width="70%" height="70%"/></p>

> 회원 정보를 가지고 있는 테이블
> 
- 칼럼
    - `user_num` : 회원번호 (PK)
    - `id` : 아이디 (로그인시 사용하는 아이디)
    - `password` : 비밀번호
    - `name` : 이름
    - `age` : 나이
    - `email` : 이메일
    - `manager` : 매니저인지 (매니저이면 1 / 아니면 0)

### Account

<p align="center"><img src="https://user-images.githubusercontent.com/78288539/189818177-751e742d-e1ae-4205-8ef9-88cb94c3e307.png" width="70%" height="70%"/></p>

> 통장 정보로 한 회원이 여러 통장을 가지고 있을 수 있다.
> 
- 칼럼
    - `account_num` : 통장번호 (PK : 우선키)
    - `user_num` : 회원번호 (FK : 외래키)
    - `balance` : 잔고
    - `update_date` : 최근 수정 날짜
    - `create_date` : 통장 생성 날짜

### Transaction

<p align="center"><img src="https://user-images.githubusercontent.com/78288539/189818189-a3326baf-531d-4db5-a229-8b2e706bb30f.png" width="70%" height="70%"/></p>

> 거래 정보로 하나의 통장에 하나의 거래 정보 테이블이 존재한다.
> 
- 칼럼
    - `transaction_num` : 거래 번호 (PK)
    - `type` : 입금인지 출금이지의 대한 타입c
    - `amount` : 금액
    - `client` : 거래처
    - `date` : 거래일자
    - `detail` : 세부사항
    - `account_num` : 통장번호 (FK : 외래키)
