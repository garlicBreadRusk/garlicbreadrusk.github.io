---
layout: post
title: 메모로그
subtitle: 나중에 공부할 게시물
categories: read-later
tags: [Spring]
---



**Here is some bold text**

## Here is a secondary heading

1. 현황
1.1. DB pool 설정이 적용되지 않고 있음
spring boot1방식의 DB pool설정으로 되어 있어서 설정이 무시되었음(그래서 모든 설정이 기본값으로 적용되고 있었음. 아래 '3.참고')
1.2. 죽은 connection 접근 문제
Mysql 서버상의 유휴 커넥션 최대 유지 시간은 10분으로 설정되어 있으나
SHOW VARIABLES LIKE 'wait_timeout'; 👉 600(10분)

HikariPool의 유휴 커넥션 최대 유지 시간은 기본값 30분으로 적용 되어 있어서 10분 동안 DB사용이 없으면 죽은 connection 접근 문제 발생
이 때 자동으로 재접속 하지만 아래와 같은 오류메시지가 많이 쌓이고(2000라인 정도) 응답지연(0.5초 정도) 발생
HikariPool-2 - Failed to validate connection net.sf.log4jdbc.sql.jdbcapi.ConnectionSpy@6bd39c6c (No operations allowed after connection closed.). Possibly consider using a shorter maxLifetime value.

2. 조치내용
application-*.yml 파일들의 pool설정 추가
idle-timeout을 9분으로 함 : Mysql서버쪽에서 접속을 끊기 전에 pool에서 종료하므로 죽은 connection 접근 문제예방
idle-timeout: 540000

사용자쪽 수정한 파일들 : https://gitlab.com/oz-z/beamz2/-/merge_requests/39/diffs
3. 참고
spring boot2 + hikari ( 참고 : https://stackoverflow.com/a/52326856/4766882 ) 
설정에서 hikari라는 예약어를 사용하지 않음
DB주소 설정 이름으로 jdbcUrl를 사용
 1. 현황
1.1. DB pool 설정이 적용되지 않고 있음
spring boot1방식의 DB pool설정으로 되어 있어서 설정이 무시되었음(그래서 모든 설정이 기본값으로 적용되고 있었음. 아래 '3.참고')
1.2. 죽은 connection 접근 문제
Mysql 서버상의 유휴 커넥션 최대 유지 시간은 10분으로 설정되어 있으나
SHOW VARIABLES LIKE 'wait_timeout'; 👉 600(10분)

HikariPool의 유휴 커넥션 최대 유지 시간은 기본값 30분으로 적용 되어 있어서 10분 동안 DB사용이 없으면 죽은 connection 접근 문제 발생
이 때 자동으로 재접속 하지만 아래와 같은 오류메시지가 많이 쌓이고(2000라인 정도) 응답지연(0.5초 정도) 발생
HikariPool-2 - Failed to validate connection net.sf.log4jdbc.sql.jdbcapi.ConnectionSpy@6bd39c6c (No operations allowed after connection closed.). Possibly consider using a shorter maxLifetime value.

2. 조치내용
application-*.yml 파일들의 pool설정 추가
idle-timeout을 9분으로 함 : Mysql서버쪽에서 접속을 끊기 전에 pool에서 종료하므로 죽은 connection 접근 문제예방
idle-timeout: 540000

사용자쪽 수정한 파일들 : https://gitlab.com/oz-z/beamz2/-/merge_requests/39/diffs
3. 참고
spring boot2 + hikari ( 참고 : https://stackoverflow.com/a/52326856/4766882 ) 
설정에서 hikari라는 예약어를 사용하지 않음
DB주소 설정 이름으로 jdbcUrl를 사용
 

 옳은 판단이라고 생각합니다, 설정 변경 및 적용 부탁 드립니다.

추가로 ( @이 금평 ),
DB connection 추가 옵션에 대한 설명이 잘 나온 사이트 있어서 공유합니다.
https://freedeveloper.tistory.com/250

yml 설정 관련 사이트도 공유합니다.
https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html

[참고]
**max-lifetime (Default : 1800000) (30분)**커넥션의 최대 유지 시간을 밀리초 단위로 설정한다.
이 시간이 지난 커넥션 중에서 미사용 커넥션은 즉시 종료하고, 사용 중인 커넥션은 종료된 이후에 풀에서 제거한다.

MySQL은 자신에게 맺어진 커넥션 중일정 시간 이상 사용하지 않은 커넥션을 종료 하는 프로세스가 존재합니다. (MySQL의 wait_timeout이라는 값을 통해 확인할 수 있고 default 8시간)

기존 DBCP들은 연결을 맺은 커넥션들이 끊기는 것을 방지하기 위해 SELECT 1 등의 쿼리를 주기적으로 날려 이 문제를 회피하는 반면 HikariCP는 maxLifetime 설정값에 따라 스스로 커넥션을 유지하고, maxLifetime 이 지난 커넥션은 종료시키고 새로 커넥션을 생성하는 사이클이 반복되는 방식이다.

HikariCP 개발자의 의도는 유휴 상태를 체크하는 쿼리 자체도 불필요한 리소스 낭비라 생각을 하고, 유휴 또는 수명 제한을 초과하여 폐기된 연결을 교체하는 데 드는 비용은 일반적으로 두 자릿수 밀리 초 단위로 측정되기 때문에 maxLifetime 개념을 뒀다고 한다.

possibly consider using a shorter maxlifetime value
간혹 위와 같은 오류가 나오게 된다면, maxlifetime 시간을 mysql의 wait_timeout 시간보다 2~3초 짧게 설정하면 된다.
(mysql은 global, session wait_timeout으로 2개의 종류에 있음에 주의한다.)
## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.
