# Vue에서 index.html과 App.vue의 구성 차이

## 코드
```html
<!doctype html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />

    <link rel="icon" href="/favicon.ico" />

    <meta
      name="viewport"
      content="width=device-width, initial-scale=1.0"
    />

    <title>Seoulog</title>
  </head>

  <body>
    <div id="app"></div>

    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

```vue
<!-- App.vue -->
<template>
  <div>
    <h1>Vue 앱 시작</h1>
  </div>
</template>
```

## 오류 코드
```text
Vue에서 index.html 구조가 일반 HTML과 다르게 느껴짐
App.vue로 진행하는 방식이 헷갈림
```

## 해결 방법
1. `index.html`은 Vue 앱의 진입점 템플릿이며, `#app` 영역에 앱이 마운트된다는 점을 이해합니다.
2. `App.vue`는 실제 화면을 구성하는 단일 컴포넌트이고, Vue가 이를 렌더링한다는 점을 이해합니다.
3. `main.js`가 `createApp(App)`로 `App.vue`를 연결해주기 때문에, HTML은 기본 골격이고 화면 내용은 Vue 컴포넌트에서 관리된다는 흐름을 익힙니다.

## 해결 완료
Vue 프로젝트에서는 `index.html`이 HTML 기본 구조를 담당하고, 실제 UI는 `App.vue`와 같은 컴포넌트에서 구성됩니다.

## 정리
- `index.html`은 브라우저가 읽는 진입 HTML 파일입니다.
- `App.vue`는 Vue 컴포넌트로서 화면 내용을 담습니다.
- `main.js`가 두 개를 연결해 Vue 앱이 실행됩니다.
