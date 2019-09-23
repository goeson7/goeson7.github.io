---
layout: post
title:  "SQLiteCurruptException: Database disk image is malformed(code 11) 에러"
date:   2019-09-16 21:14:00 +0530
---

안드로이드 앱 배포 시 업데이트할 DB 데이터 양이 많은 경우 기존 데이터를 삽입한 Sqlite DB 파일을 APK에 포함하여 배포하기도 한다.
이렇게 앱을 배포하여 앱을 업데이트한 다음 DB파일 복사하고 DB를 열 때 가끔 SQLiteException을 뿜어내면서 앱이 죽어버리는 경우가 발생되었다.

> SQLiteCurruptException: Database disk image is malformed(code 11)

#
&nbsp;&nbsp;malformed(기형인, 흉하게 일그러진)인 것으로 보아 DB가 깨져버려서 발생한 에러로 아래와 같은 경우 발생될 수 있다.

#### 1. SQLite 버그
 &nbsp;&nbsp;&nbsp;&nbsp;SQLite에 Curruption 이 발생되는 시나리오에 대해서 패치가 되었으나 Android에 해당 버전이 반영되지 않은 경우.

#### 2. SQLite 버전 호환성 문제
 &nbsp;&nbsp;&nbsp;&nbsp;상위 SQLite 버전으로 생성된 DB를 하위 SQLite 버전에서 열려고 할 경우.

#### 3. 잘못된 SQLite API 사용
 &nbsp;&nbsp;&nbsp;&nbsp;사용 중인 데이터베이스에 대한 잘못된 접근, 특히 Insert Transaction 중에 DB파일을 복사하는 경우 재앙이 발생될 수 있음.

#### 4. 하드웨어/파일시스템 오류

#
&nbsp;&nbsp;필자의 경우에는 동일한 오류 발생 시 다음과 같은 현상을 발견하였다.

  * DB 데이터 업데이트 시 Update 쿼리를 수행 중 홈 화면에서 Current Task를 통해 앱을 종료시킨 후에 앱 버전을 업데이트할 경우 종종 재현됨
  * Activity의 OnDestroy()에서 DB를 close하고 있는데 Current Task를 통해 앱을 종료시킬 경우 호출되지 않음
  * 앱 버전 업데이트 없이 재실행 시 정상동작함

&nbsp;&nbsp;Transaction 중 앱이 종료되었는데 DB를 close하기 위한 코드가 수행되지 않았고, 정상적으로 close 되지 않은 상태에서 앱 버전 업데이트 후 DB 파일을 복사하여 발생된 것으로 보인다.
&nbsp;&nbsp;이에 앱 버전 업데이트 이후 DB 파일을 복사하기 전에 기존 DB 파일이 있을 경우 명시적으로 close하도록 처리하는 방법으로 해결하였다.


## 참고
 * [http://stackoverflow.com/a/2960439][stackoverflow-2960439]
 * [http://stackoverflow.com/a/5275022][reason-wrong-sqlite-api]
 * [http://goo.gl/Eiv0EA][reason-version-migration]
 * [http://goo.gl/RIkCtS][solution-backup-file]
 
 
 
[reason-sqlite-bug]: http://stackoverflow.com/a/2960439
[reason-version-migration]: http://goo.gl/Eiv0EA
[reason-wrong-sqlite-api]: http://stackoverflow.com/a/5275022
[solution-backup-file]: http://goo.gl/RIkCtS
