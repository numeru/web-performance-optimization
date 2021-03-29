## 실행

```
1. npm install

2. npm start

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
