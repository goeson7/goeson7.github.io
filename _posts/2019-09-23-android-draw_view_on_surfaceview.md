---
layout: post
title:  "SurfaceView에 overlay된 View의 onDraw가 호출되지 않는다면?"
date:   2019-09-23 23:07:00 +0530
---

&nbsp;&nbsp;카메라를 이용한 앱 개발 등의 SurfaceView 위의 View를 overlay해야 할 경우,
onDraw()가 호출되지 않아서 해당 View가 정상적으로 그려지지 않는 경우가 발생될 수 있다.

&nbsp;&nbsp;이 경우 아래와 같이 설정하여 해결이 가능하다.

~~~java
mSurfaceView.setWillNotDraw(false);
~~~
 
