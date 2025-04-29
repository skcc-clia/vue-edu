# Vue3 비동기 컴포넌트 등록 가이드

## 1. 컴포넌트 파일에서 직접 등록

```javascript
// MyParent.vue
<script setup>
import { defineAsyncComponent } from 'vue'

// 기본적인 비동기 컴포넌트 등록
const AsyncChild = defineAsyncComponent(() =>
  import('./AsyncChild.vue')
)

// 로딩/에러 상태 처리가 포함된 비동기 컴포넌트
const AsyncChildWithStates = defineAsyncComponent({
  loader: () => import('./AsyncChild.vue'),
  loadingComponent: LoadingSpinner,
  errorComponent: ErrorDisplay,
  delay: 200,
  timeout: 3000
})
</script>

<template>
  <AsyncChild />
  <AsyncChildWithStates />
</template>
```

## 2. 별도의 컴포넌트 파일에서 등록

```javascript
// components/AsyncComponents.js
import { defineAsyncComponent } from 'vue'

export const AsyncChild = defineAsyncComponent(() =>
  import('./AsyncChild.vue')
)

export const AsyncForm = defineAsyncComponent({
  loader: () => import('./AsyncForm.vue'),
  loadingComponent: LoadingSpinner,
  errorComponent: ErrorDisplay
})
```

```javascript
// MyParent.vue
<script setup>
import { AsyncChild, AsyncForm } from './components/AsyncComponents'
</script>

<template>
  <AsyncChild />
  <AsyncForm />
</template>
```

## 3. 라우터에서 비동기 컴포넌트 등록

```javascript
// router/index.js
import { createRouter, createWebHistory } from 'vue-router'

const routes = [
  {
    path: '/dashboard',
    component: () => import('../views/Dashboard.vue')
  },
  {
    path: '/profile',
    component: () => import('../views/Profile.vue')
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

## 4. 전역 비동기 컴포넌트 등록

```javascript
// main.js
import { createApp, defineAsyncComponent } from 'vue'
import App from './App.vue'

const app = createApp(App)

// 전역 비동기 컴포넌트 등록
app.component('async-modal', defineAsyncComponent(() =>
  import('./components/Modal.vue')
))

app.mount('#app')
```

## 비동기 컴포넌트 등록 시 고려사항

1. **성능 최적화**
   - 큰 컴포넌트나 자주 사용하지 않는 컴포넌트에 적합
   - 초기 번들 크기를 줄여 첫 로딩 시간 단축
   - 필요한 시점에만 컴포넌트 로드

2. **로딩 상태 처리**
   ```javascript
   const AsyncComponent = defineAsyncComponent({
     loader: () => import('./HeavyComponent.vue'),
     loadingComponent: LoadingSpinner,
     delay: 200,  // 로딩 컴포넌트 표시 전 대기 시간
     timeout: 3000  // 타임아웃 시간
   })
   ```

3. **에러 처리**
   ```javascript
   const AsyncComponent = defineAsyncComponent({
     loader: () => import('./HeavyComponent.vue'),
     errorComponent: ErrorDisplay,
     onError(error, retry, fail, attempts) {
       if (attempts <= 3) {
         retry()
       } else {
         fail()
       }
     }
   })
   ```

4. **Suspense 컴포넌트와 함께 사용**
   ```html
   <template>
     <Suspense>
       <template #default>
         <AsyncComponent />
       </template>
       <template #fallback>
         <LoadingSpinner />
       </template>
     </Suspense>
   </template>
   ```

## 주의사항

1. 비동기 컴포넌트는 `setup()` 함수 내에서 정의해야 합니다.
2. `<script setup>`을 사용할 때는 컴포넌트를 import한 후 바로 사용할 수 있습니다.
3. 라우터에서 사용할 때는 별도의 `defineAsyncComponent` 호출 없이 직접 import 함수를 사용할 수 있습니다.
4. 전역 등록은 가능하지만, 가능하면 지역 등록을 사용하는 것이 번들 크기 최적화에 도움이 됩니다. 