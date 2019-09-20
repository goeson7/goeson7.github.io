---
layout: post
title:  "Nexus 5X 에서 카메라 프리뷰 상하좌우 반전되는 현상"
date:   2019-09-20 21:20:00 +0530
---

Nexus 5X 단말에서 (구)Camera API를 사용하여 카메라 구현 시 카메라 프리뷰가 상하반전이 되는 현상이 있다.
카메라 앱에서 Landscape 고정화면으로 카메라를 사용하고 있는데 
카메라의 orientaion을 0으로 설정하니 기존 단말들은 정상, Nexus 5X는 180도로 회전되어 보이고
orientaion을 180으로 설정하니 기존 단말들은 180도로 회전되고, Nexus 5X는 정상으로 보여지고 있었다.

해당 현상의 원인 대해서는 [여기][nexus-5-camera-orientation-issue-blog]에서 잘 설명하고 있다.

해결방법은 안드로이드 개발자 사이트에서 [setDisplayOrientation][android-setdisplayorientation]을 검색해보면 
아래의 코드를 찾을 수 있는데 이 코드를 이용하여 단말 화면의 orientation과 카메라 프리뷰의 orientation을 일치시킬 수 있다.
카메라 프리뷰를 시작하기 전에 camera.setDisplayOrientaion()으로 카메라 프리뷰 방향을 설정할 때 아래의 코드를 사용하면 해결이 가능하다.

~~~java
public static void setCameraDisplayOrientation(Activity activity,
         int cameraId, android.hardware.Camera camera) {
     android.hardware.Camera.CameraInfo info =
             new android.hardware.Camera.CameraInfo();
     android.hardware.Camera.getCameraInfo(cameraId, info);
     int rotation = activity.getWindowManager().getDefaultDisplay()
             .getRotation();
     int degrees = 0;
     switch (rotation) {
         case Surface.ROTATION_0: degrees = 0; break;
         case Surface.ROTATION_90: degrees = 90; break;
         case Surface.ROTATION_180: degrees = 180; break;
         case Surface.ROTATION_270: degrees = 270; break;
     }

     int result;
     if (info.facing == Camera.CameraInfo.CAMERA_FACING_FRONT) {
         result = (info.orientation + degrees) % 360;
         result = (360 - result) % 360;  // compensate the mirror
     } else {  // back-facing
         result = (info.orientation - degrees + 360) % 360;
     }
     camera.setDisplayOrientation(result);
  }
~~~
[nexus-5-camera-orientation-issue-blog]: https://ryueyes11.tistory.com/6267
[android-setdisplayorientation]: https://developer.android.com/reference/android/hardware/Camera.html?hl=ko#setDisplayOrientation(int)
