# Vue 3 + Pinia를 이용한 상태 관리

## 🧠 1. Pinia란?

- Vue 공식 상태 관리 라이브러리입니다.
- Vuex의 후속 라이브러리로, 더 가볍고 사용하기 쉽습니다.
- `Composition API` 스타일과 훌륭하게 호환됩니다.

---

## 🔧 2. 설치 및 설정

```bash
npm install pinia
```

### 📦 main.js 등록

```js
import { createApp } from 'vue';
import { createPinia } from 'pinia';
import App from './App.vue';

const app = createApp(App);
app.use(createPinia());
app.mount('#app');
```

---

## 🏪 3. 스토어 생성

```js
// stores/counter.js
import { defineStore } from 'pinia';

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),
  actions: {
    increment() {
      this.count++;
    }
  }
});
```

> `defineStore`를 사용하여 state, actions, getters를 설정합니다.

---

## 🧩 4. 스토어 사용

```vue
<script setup>
import { useCounterStore } from '@/stores/counter';

const counter = useCounterStore();
</script>

<template>
  <p>Count: {{ counter.count }}</p>
  <button @click="counter.increment">+</button>
</template>
```

---

## 🧠 5. Getter 사용

```js
// stores/counter.js
getters: {
  doubleCount: (state) => state.count * 2
}
```

```vue
<p>두 배 수치: {{ counter.doubleCount }}</p>
```

---

## 📦 6. Setup 방식 스토어

```js
// stores/user.js
import { defineStore } from 'pinia';
import { ref } from 'vue';

export const useUserStore = defineStore('user', () => {
  const name = ref('홍길동');
  const updateName = (newName) => name.value = newName;

  return { name, updateName };
});
```

---

## 🔁 7. 상태 영속화 (Persisted State)

- 플러그인을 이용해 상태를 localStorage에 저장할 수 있습니다.

```bash
npm install pinia-plugin-persistedstate
```

```js
// main.js
import piniaPluginPersistedstate from 'pinia-plugin-persistedstate';
pinia.use(piniaPluginPersistedstate);
```

```js
export const useCounterStore = defineStore('counter', {
  state: () => ({ count: 0 }),
  persist: true
});
```

---

## ✅ 정리

- Pinia는 Vue 3 공식 상태 관리 솔루션입니다.
- `state`, `actions`, `getters`를 통해 상태를 쉽고 명확하게 관리할 수 있습니다.
- `setup` 스타일과 영속 저장 기능도 지원합니다.
- 다음 시간에는 **Vue 앱을 테스트하고 배포**하는 방법을 다룹니다.
