---
layout: post
title:  "Android M 호환 시 알아두어야 할 것"
date:   2019-05-23 21:03:36 +0530
--

 Android 6.0 부터 사용자 데이터 보호와 같은 보안적인 이유로 기기의 WiFi Mac Address 를 코드 상으로 가져올 수 없도록 변경되었다. 은행권에서는 기기에 MDM 적용이나 기기인증을 위한 키 값으로 WiFi Mac Address를 쓰는 경우가 많은데, Android 6.0으로 업데이트 시 문제가 발생될 수 있다. 대체 식별 값으로 Android ID, IMEI 값이 있지만 해당 값도 사용하기에는 제약사항이 존재하기 때문에 사용함에 있어 유의해야 한다.

2019-09-16
 Android 10 부터는 IMEI 같은 기기의 재설정할 수 없는 고유식별자는 사용이 불가하다. 대체 사용가능한 식별자에 대해서는 [Android Developer][android-10-identifier-guide]에서 가이드하고 있다.


#### 참고
  * [안드로이드 6.0 마시멜로 무엇을 테스트 할까요? - OneStore 개발자센터 블로그][onestore-dev-blog]
  * [ Android M에서 삭제/추가 된 Permission - Android Developer][android-m-permission-changes]
  * [안드로이드 Q 기기 고유 식별자(IMEI 등) 제한][android-q-brunch]

[onestore-dev-blog]: https://blog.tstore.co.kr/168
[android-m-permission-changes]: http://developer.android.com/intl/ko/sdk/api_diff/23/changes/android.Manifest.permission.html
[android-10-identifier-guide]: https://developer.android.com/training/articles/user-data-ids
[android-q-brunch]: https://brunch.co.kr/@huewu/9
