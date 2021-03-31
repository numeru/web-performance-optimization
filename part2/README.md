## 실행

```
1. npm install

2. npm run start

3. npm run server
```

---

## 최적화

### 1. animation

- 브라우저 렌더링 과정: DOM + CSSOM -> Render Tree -> Layout -> Paint -> Composite
- 쟁크 현상: 브라우저에 과부하가 걸려 애니메이션이 버벅이는 현상
- reflow나 repaint보다 GPU의 도움을 받을 수 있는 transform을 사용하기.

```
// 이전
width: ${({width}) => width}%;

// 이후
transform: scaleX(${({ width }) => width / 100});
```

### 2. Component Lazy Load

- 한 컴포넌트 안에서 당장 필요하지 않은 요소는 Code Splitting을 통해 늦게 load한다.
- 파일이 불러와지는동안 요소가 제대로 보이지 않는 단점이 있다.

```js
const LazyImageModal = lazy(() => import("./components/ImageModal"));

<Suspense fallback={null}>
  {showModal ? (
    <LazyImageModal
      closeModal={() => {
        setShowModal(false);
      }}
    />
  ) : null}
</Suspense>;
```

### 3. Component Preloading

- 사용되기 전에 미리 코드를 불러온다.
- 여기서는 마우스를 버튼 위에 올렸을 때, 모든 컴포넌트의 로드가 완료된 후 두가지로 나눈다.

```js
const handleMouseEnter = () => import("./components/ImageModal");

<ButtonModal onMouseEnter={handleMouseEnter}>
```

```js
useEffect(() => import("./components/ImageModal"), []);
```

### 4. Image Preloading

- 이미지를 미리 가져온다.

```js
useEffect(() => {
  const img = new Image();
  img.src =
    "https://stillmed.olympic.org/media/Photos/2016/08/20/part-1/20-08-2016-Football-Men-01.jpg?interpolation=lanczos-none&resize=*:800";
}, []);
```
