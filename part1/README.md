## 실행

```
$ npm install

$ npm run start

$ npm run server

// build
$ npm run serve
```

---

## 도구

### Lighthouse

- 크롬의 lighthouse를 통해 현재 웹사이트에 대한 객관적인 성능 지표를 얻는다.

### Performance

- 페이지가 로드되면서 실행되는 작업들을 보여준다.
- network와 main 탭에서 과정 확인 가능.

---

## 최적화

- 로딩 성능 + 렌더링 성능

### 과정

#### 1. image

- 필요한 크기의 두배로 가져오는 것이 좋다. (120 x 120 -> 240 x 240)
- 보통 사용자에게 전해주기 전에 원하는 크기로 가공하기 위해 CDN을 사용한다.

```js
function getParametersForUnsplash({ width, height, quality, format }) {
  return `?w=${width}&h=${height}&q=${quality}&fm=${format}&fit=crop`;
}

<img
  src={
    props.image +
    getParametersForUnsplash({
      width: 240,
      height: 240,
      quality: 80,
      format: "jpg",
    })
  }
  alt="thumbnail"
/>;
```

#### 2. bottleneck

1. js에서 필요이상으로 지속되는 부분을 더 적절한 방법으로 바꾸어준다.

- removeSpecialCharacter 메서드에 의해 Article이 매우 오래 실행되고 있다.

```js
function removeSpecialCharacter(str) {
  const removeCharacters = [
    "#",
    "_",
    "*",
    "~",
    "&",
    ";",
    "!",
    "[",
    "]",
    "`",
    ">",
    "\n",
    "=",
    "-",
  ];
  let _str = str;
  let i = 0,
    j = 0;

  for (i = 0; i < removeCharacters.length; i++) {
    j = 0;
    while (j < _str.length) {
      if (_str[j] === removeCharacters[i]) {
        _str = _str.substring(0, j).concat(_str.substring(j + 1));
        continue;
      }
      j++;
    }
  }

  return _str;
}
```

- 알고리즘을 바꾸거나 검사되는 양을 줄여 최적화한다.

```js
function removeSpecialCharacter(str) {
  let _str = str.substring(0, 300);
  _str = _str.replace(/[\#\_\*\~\&\;\!\[\]\`\>\n\=\-]/g, "");
  return _str;
}
```

#### 3. bundle

1. Bundle Analyzer

```js
// 설치
npm install --save-dev cra-bundle-analyzer

// 실행
npx cra-bundle-analyzer
```

- react-syntax-highlighter의 refractor가 chunk.js의 큰 부분을 차지하고 있다.
- 당장 필요한 모듈만 다운로드 받을 수 있도록 나눈다.

2. Code Splitting[(관련 docs)](https://ko.reactjs.org/docs/code-splitting.html)

- bundle에는 여러 페이지와 모듈들이 들어 있는데, 이를 일정 단위로 분할한다.
- 불필요한 코드 또는 중복되는 코드가 없이 적절한 사이즈의 코드가 적절한 타이밍에 로드될 수 있도록 한다.

```js
const ListPage = lazy(() => import("./pages/ListPage/index"));
const ViewPage = lazy(() => import("./pages/ViewPage/index"));

function App() {
  return (
    <div className="App">
      <Suspense fallback={<div>로딩중...</div>}>
        <Switch>
          <Route path="/" component={ListPage} exact />
          <Route path="/view/:id" component={ViewPage} exact />
        </Switch>
      </Suspense>
    </div>
  );
}
```

#### 4. Text Compression

- 서버에서 보내는 resource를 압축한다. (gzip)

```js
node ./node_modules/serve/bin/serve.js -s build
```

- 압축을 푸는데도 시간이 소요되므로, 파일의 크기가 2KB이상일 때 압축을 해주는 것이 좋다.
