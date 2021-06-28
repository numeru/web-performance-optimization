# Image

### 1. 종류

1. PNG

- 알파채널 이미지 변환 기법 사용
- 핵심 이미지 레이어를 제외한 배경 이미지 레이어를 제거
- 사이즈가 크다.
- 투명 기능이 필요하지 않을 경우 JPEG를 사용하는 것이 더 낫다.
- chunk 형태로 이미지 정보를 저장하지만 대부분 웹 렌더링에는 필요없다.

2. GIF

- 애니메이션 기능 제공
- 크기 때문에 복잡한 이미지보단 로고 등 단순한 형태에 사용.

3. JPEG

- 품질값을 정할 수 있다. (하지만 100으로 설정해도 약간의 손실은 발생한다.)
- 압축률이 높아 크기가 작다.
- 동일한 이미지를 여러번 편집해야 하는 경우, 편집할 때는 PNG를 사용하다 편집을 완료하면 JPEG로 저장하는 것을 권장.
- 이미지 정보 외에 많은 메타 데이터를 포함하지만 웹상에 표시할 때는 필요없다.

4. WebP

- 높은 압축률
- 무손실 압축, 애니메이션 기능, 투명 기능 지원
- 이미지 품질을 많이 낮추면 화질에 손실 발생
- JPEG와 같은 점진적 데이터 전송 기능이 없다.
- 따라서 일반적인 환경에서는 JPEG보다 낫지만 네트워크 품질이 낮은 경우를 고려한다면 아쉽다.

5. 무손실 이미지 vs 손실 이미지

- 압축시 원본 이미지의 정보 손실 허용 여부에 따라 나뉜다.
- 무손실 이미지: GIF, PNG
- 손실 이미지: JPEG ,WebP, JPEG XR, JPEG 2000

---

### 2. 압축

##### 1. 무손실 압축

- 이미지 유형마다 다르게 처리
- 스크립트롤 통해 압축을 자동화 할 수 있다.

1. GIF

- ImageMagicK: 비트맵 이미지 생성, 수정, 삭제 가능. 모든 형식의 이미지 변환 가능.
- Giflossy: GIF의 뮤손실, 손실 압축 지원. Animated GIF 생성 및 최적화 가능.
- Gifsicle: Animated GIF 생성, 최적화. WebP로 변환 가능.
- gif2webp converter: GIF를 WebP로 변경.

2. PNG

- Pngcrush: 최적화 도구. 실행 속도 대비 최적화 결과가 좋다. 스크립트를 사용하여 자동화하기 적합.
- Pngquant: 무손실, 손실 압축 지원.

3. JPEG

- MozJPEG: 압축 라이브러리. 1-100 품질 변환 가능. 무손실, 손실압축 지원. Progress JPEG로 변환 가능.
- libJpeg: jpegtran 모듈은 이미지 안 메타 정보를 삭제하거나 Progress JPEG로 전환하는 등 최적화 작업 수행.
- Guetzli: 최적화된 파일 생성. 손실 압축도 가능.

##### 2. 손실 압축

- 이미지 정보를 손실시켜 파일 크기를 줄인다.
- 80 ~ 85% 정도 품질의 이미지 손실 압축을 권장 (정확히는 DSSIM 참고).
- 디코딩 -> 알고리즘으로 화질 저하 -> 인코딩 (ImageMagick)

1. Akamai
2. Cloudinary
3. JPEGmini

---

### 3. 브라우저 특화 이미지로 변환

1. WebP

- libwebp
- ImageMagick

2. JPEG2000

- OpenJEPG
- Kakadu
- JPEG XR

---

### 4 .반응형 웹에서의 이미지

- 모바일 환경에서 필요하지 않은 리소스들을 과도하게 다운로드 받게 될 수 있다.

1. 내려받아 줄이기

- <img> 태그의 width와 height를 바꿔 화면에서 보이는 크기를 줄인다 해도 실제 이미지 자체가 작아지는 것이 아니기 때문에 성능이 저하된다.

2. 내려받아 숨기기

- 데스크톱 환경에서 보이는 것을 모바일 환경에서 display: none을 통해 숨겨도 리소스는 여전히 받아오므로 성능이 저하된다.

3. 화면 바깥 부분

- 화면에 보이지 않는 부분 (스크롤로 내려야 보이는 부분)도 로드될 때 한번에 다운로드 되기 때문에 성능에 영향을 준다.

##### 사용자의 환경에 따라 그에 맞는 적합한 이미지를 전송해야한다 = 반응형 이미지

---

### 5. 반응형 이미지

1. <img> 태그의 srcset, size

- 비율, 내용 등이 동일한 이미지를 크기만 다르게 사용할 것을 권장.
- srcset: 사용자의 다양한 환경에 따라 다른 이미지 URL을 지정할 수 있다.
- size: 브레이크 포인트에 따른 이미지 크기를 지정할 수 있다.

```html
<!-- 1x, 2x는 이미지 화소 밀도 -->
<img src="small.jpg" alt="rwd" srcset="pic-200.jpg 1x, pic-400.jpg 2x" />

<!-- 
  size 속성을 사용할 때는 width 정보를 나타낸다.
  200w, 400w: 이미지의 width
  100vw, 30vw, 300px: 화면에 표현될 크기 
-->
<img
  src="small.jpg"
  alt="rwd"
  srcset="pic-200.jpg 200w, pic-400.jpg 400w"
  size="(max-width: 400px) 100vw, (max-width: 800px) 30vw, 300px"
/>
```

2. <picture> 태그

- 위 방법의 단점을 모두 보완
- 조건에 맞는 이미지만 다운로드하여 사용할 수 있다.

```html
<picture>
  <source media="(min-width: 45em)" srcset="large.jpg, large-hd.jpg 2x" />
  <source media="(min-width: 18em)" srcset="med.jpg, med-hd.jpg 2x" />
  <source srcset="small.jpg, small-hd.jpg 2x" />
  <img src="small-1.jpg" alt="rwd" />
</picture>
```

3. Client Hints

- 서버를 이용한 이미지 처리

---

### 6. 이미지 지연 로딩

1. js

```html
<script>
    function loadReal(img) {
      if (img.display != "none") {
        img.onload = null;
        img.src = img.getAttribute("data-src");
      }
    }
<script>

// src: 1x1의 작고 투명한 파일
// data-src: 실제 이미지
<img src="1px.gif" data-src="book.jpg" alt="A Book" onload="loadReal(this)">
```

2. 라이브러리 (LazyLoad)

```html
<script src="lazyload.min.js" />
<img
  data-src="real/image/src.jpg"
  src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw=="
  onload="lzld(this)"
/>
```

---

### 7. 적응형 이미지

- 반응형 이미지는 코드가 늘어나고 이미지가 많아질수록 웹 페이지의 크기가 커진다.
- 요청 클라이언트의 정보를 감지하여 서버에서 맞춤형 데이터를 로딩한다.

##### 1. 클라이언트 정보 감지

- 앞으로 HTTP 요청에 클라이언트 정보가 추가되어 Client Hints에 담길 예정이지만 아직 지원하는 브라우저가 적다.
- HTTP 요청의 User-Agent 헤더 이용.

```
// 일반적인 형식
User-Agent: <product> / <product-version> <comment>

// 브라우저 형식
User-Agent: Mozilla/<version> (<system-information>) <platform> (platform-details) <extensions>
```

- 하지만 뷰포트, 스크린 크기, 화소 밀도 등은 솔루션을 사용해야 한다.
- DeviceAtlas, ScientiaMobile/Wurf, 51degree 등

- CDN 서비스의 부가 기능으로도 가능하다.

##### 2. 기기 정보에 따라 서버 로직 수행

##### 3. 브라우저별 이미지 전달

- HTTP 요청의 Accept 에서 브라우저 전용 이미지 형식을 확인하여 결정 가능.

##### 4. 캐시 고려 사항

- 이미지를 캐시할 경우 Vary 헤더를 활용하여 특정 헤더에 따라 콘텐츠가 달라질 것임을 알려야 한다.

```
// 요청 헤더
GET /mytest.jpg
...
Width: 320
Viewport-Width: 320

// 응답 헤더
HTTP/1.1 200 OK
Content-Type: image/jpeg
Vary: Width
```

- CDN 서비스의 캐시 키 정책을 사용해도 가능하다.
