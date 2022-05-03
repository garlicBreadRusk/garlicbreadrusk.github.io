---
layout: post
title: 메모로그
subtitle: 나중에 공부할 게시물
categories: read-later
tags: [Spring]
---



**Here is some bold text**

## Here is a secondary heading
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
