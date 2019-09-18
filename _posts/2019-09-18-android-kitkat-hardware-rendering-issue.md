---
layout: post
title:  "Android 4.4.2 에서 View 하드웨어가속 설정 시 OOM 현상"
date:   2019-09-18 11:22:00 +0530
---

Android에서는 API Level 11 부터 뷰에 대해 [개별적으로 하드웨어가속 설정이 가능][android-setlayertype]하다.

~~~java
view.setLayerType(View.LAYER_TYPE_HARDWARE);
~~~

View의 Background에 이미지를 설정하고 Zoom을 할 때 버벅임이 있어 해당 API를 사용한 적이 있었는데
삼성 갤럭시 노트 Pro 12.2 (SM-P900, 4.4.2) 단말에서 시간이 지날 때마다 Native Heap 이 누적되어 OOM(Out Of Memory)이 발생하는 현상이 있었다.

테스트를 통해 아래의 현상을 발견하였지만, 끝내 정확한 원인을 찾지 못했다.

  * Target API Level 14 이상일 경우 하드웨어가속 기능을 기본적으로 사용한다.(디폴트가 LAYER_TYPE_HARDWARE)
  * 삼성 갤럭시 노트 Pro 12.2 에서 디폴트로 지정되어 있음에도 코드를 통해 명시적으로 하드웨어가속 설정하는 API를 호출할 경우,
    Native Heap 이 누적되어 OOM(Out Of Memory)이 발생하는 현상이 재현된다.

이에 명시적인 하드웨어가속 설정이 필요한 경우, 다음의 경우에만 API를 호출하도록 처리하였다.
  * Android API Level 11 미만인 경우 설정하지 않는다.
  * Android API Level 11 이상이고, DecorView의 Layer Type이 LAYER_TYPE_HARDWARE가 아닌 경우에만 설정한다.


## 참고
 * [http://stackoverflow.com/q/20915639][stack-overflow-layer-type-oom]

[android-setlayertype]: https://developer.android.com/reference/android/view/View.html#setLayerType(int,%20android.graphics.Paint)
[stack-overflow-layer-type-oom]: http://stackoverflow.com/q/20915639
