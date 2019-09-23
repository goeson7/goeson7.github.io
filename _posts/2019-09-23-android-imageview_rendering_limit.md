---
layout: post
title:  "특정 기기에서만 ImageView에 Bitmap 설정 시 보이지 않는다면?"
date:   2019-09-23 23:16:00 +0530
---

ImageView에 분명히 Bitmap 이미지를 설정했을때 특정 기기에서만 표시되지 않는 현상이 발생되었다.
로그를 확인해보니 아래와 같은 로그가 발생되었다.

>W/OpenGLRenderer(4303): Bitmap too large to be uploaded into a texture (2592x1458, max=2048x2048)
 
#
원인은 Hardware acceleration 사용 시 ImageView에서는 GL_MAX_TEXTURE_SIZE 만큼 렌더링이 가능한데,
GL_MAX_TEXTURE_SIZE 값은 기기마다 다르기 때문에 특정 기기에서만 보이지 않는 현상이 발생하는 것이다.
이에 이미지 크기를 GL_MAX_TEXTURE_SIZE 이내로 줄여 설정하도록 수정하여 해결하였다.

#
기기의 GL_MAX_TEXTURE_SIZE 값은 아래의 코드로 확인이 가능하다.
~~~java
int[] maxTextureSize = new int[1];
gl.glGetIntegerv(GL10.GL_MAX_TEXTURE_SIZE, maxTextureSize, 0);
~~~
