# Vue 3 + Vue Router를 이용한 페이지 전환

## 🚦 1. Vue Router란?

- Vue 애플리케이션에서 SPA(Single Page Application) 방식으로 페이지를 전환할 수 있게 해주는 공식 라우팅 라이브러리입니다.
- URL에 따라 다른 컴포넌트를 렌더링할 수 있도록 합니다.

---

## 🔧 2. 설치 및 기본 설정

```bash
npm install vue-router
```

### 📄 router/index.js 생성

```js
// src/router/index.js
import { createRouter, createWebHistory } from 'vue-router';
import Home from '../views/Home.vue';
import About from '../views/About.vue';

const routes = [
  { path: '/', name: 'Home', component: Home },
  { path: '/about', name: 'About', component: About }
];

const router = createRouter({
  history: createWebHistory(),
  routes
});

export default router;
```

---

## 🧩 3. main.js에 라우터 등록

```js
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';

const app = createApp(App);
app.use(router);
app.mount('#app');
```

---

## 🧭 4. 라우터 링크 및 뷰

```html
<!-- App.vue -->
<template>
  <nav>
    <RouterLink to="/">홈</RouterLink>
    <RouterLink to="/about">소개</RouterLink>
  </nav>
  <RouterView />
</template>
```

- `<RouterLink>`: HTML `<a>` 태그를 대체하며, 페이지 새로고침 없이 전환됨.
- `<RouterView>`: 현재 경로에 해당하는 컴포넌트를 렌더링.

---

## 🗺️ 5. 동적 라우트

```js
{
  path: '/user/:id',
  name: 'User',
  component: () => import('../views/User.vue')
}
```

```html
<!-- User.vue -->
<script setup>
import { useRoute } from 'vue-router';
const route = useRoute();
const userId = route.params.id;
</script>

<template>
  <p>User ID: {{ userId }}</p>
</template>
```

---

## 🔁 6. 프로그래밍 방식의 라우팅

```js
import { useRouter } from 'vue-router';

const router = useRouter();
router.push('/about');
```

---

## ✅ 정리

- Vue Router는 SPA 애플리케이션에서 페이지 전환을 구현하는 핵심 도구입니다.
- 라우터 설정, 링크, 동적 라우트, 프로그래밍 방식 등 유연한 기능을 제공합니다.
- 다음 시간에는 Pinia를 이용한 상태 관리에 대해 배워보겠습니다.
