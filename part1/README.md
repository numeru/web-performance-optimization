## 실행

```
$ npm install

$ npm run start

$ npm run server

$ npm run serve
```

---

## 최적화

- 로딩 성능 + 렌더링 성능

### Lighthouse

- 크롬에서 lighthouse를 통해 현재 웹사이트에 대한 객관적인 성능 지표를 얻는다.

### performance

- 페이지가 로드되면서 실행되는 작업들을 보여준다.
- network와 main 탭에서 과정 확인 가능.

### 과정

1. 이미지 가져오기

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

2. bottleneck

- js에서 필요이상으로 지속되는 부분을 더 적절한 방법으로 바꾸어준다.

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

3.
