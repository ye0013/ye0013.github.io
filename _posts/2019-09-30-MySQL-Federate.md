---
title: Federate(Federate)
description: GROUP_CONCAT(MySQL)
categories:
 - MySQL
tags: MySQL
---

> 원본 테이블 -> 중간 테이블 -> Federated 테이블

## Federate 대상이 될 테이블 생성 및 유저 생성
### Federate 대상이 될 테이블 생성
```sql
SHOW CREATE TABLE `원본 테이블`;
```
로 테이블 생성 SQL을 확인한다.

<br/>

### 중간 테이블 생성
```sql
CREATE TABLE `중간 테이블` {
    ...
}
```

<br/>

### Federate용 유저 생성 후 권한 부여
```sql
CREATE USER '사용자'@'IP주소' IDENTIFIED BY '비밀번호';
GRANT ALL PRIVILEGES ON `DB`.`중간 테이블` TO '사용자'.'IP주소' IDENTIFIED BY '비밀번호';
```


<br/>

## 원본 테이블에서 중간 테이블로 데이터 옮기기
### 원본 테이블에서 중간 테이블로 데이터를 옮기는 프로시저 생성
```sql
DROP PROCEDURE IF EXISTS `프로시저명`;

DELIMITER //

CREATE PROCEDURE `프로시저명`()
BEGIN
    DECLARE today DATE DEFAULT SUBDATE(NOW(), 1);
    DECLARE yesterday DATE DEFAULT SUBDATE(today, 1);

    DELETE FROM `DB명`.`중간 테이블` WHERE `컬럼명`=DATE_FORMAT(yesterday, "%Y%m%d") OR `컬럼명`=DATE_FORMAT(today, "%Y%m%d");

    INSERT INTO `DB명`.`중간 테이블`
    SELECT *
    FROM `DB명`.`원본 테이블`
    WHERE `컬럼명`=DATE_FORMAT(today, "%Y%m%d");

    UPDATE `DB명`.`중간 테이블`
    SET `컬럼명`=`새로운 값` WHERE `컬럼명`=`기존값`;
END //

DELIMITER ;
```

<br/>

### 프로시저를 매일 스케줄링하는 이벤트 생성
```sql
# SET GLOBAL event_scheduler = ON;			// super만 가능

DROP EVENT IF EXISTS `이벤트명`;

DELIMITER $$

CREATE EVENT `이벤트명`
ON SCHEDULE EVERY 1 DAY START 'YYYY-MM-DD HH:mm:ss'
DO BEGIN
    SET SQL_SAFE_UPDATES = 0;
    CALL `프로시저명`();
    SET SQL_SAFE_UPDATES=1;
END $$

DELIMITER ;
```

<br/>

## Federated 테이블 생성
```sql
CREATE TABLE `Federated 테이블 명` {
    ...
}
ENGINE=FEDERATED 
CONNECTION='mysql://사용자명:비밀번호@원본DB의IP주소:포트/DB명/중간테이블명'
```