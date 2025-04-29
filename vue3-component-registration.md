# Vue3 컴포넌트 등록 방법

## 1. 전역 컴포넌트 등록

```javascript
// main.js
import { createApp } from 'vue'
import App from './App.vue'
import MyComponent from './components/MyComponent.vue'

const app = createApp(App)
app.component('my-component', MyComponent)
app.mount('#app')
```

## 2. 지역 컴포넌트 등록

### Options API 방식
```javascript
// MyParent.vue
import MyChild from './MyChild.vue'

export default {
  components: {
    'my-child': MyChild
  }
}
```

### Composition API 방식
```javascript
// MyParent.vue
import { defineComponent } from 'vue'
import MyChild from './MyChild.vue'

export default defineComponent({
  components: {
    'my-child': MyChild
  }
})
```

### `<script setup>` 방식
```javascript
// MyParent.vue
<script setup>
import MyChild from './MyChild.vue'
</script>

<template>
  <my-child />
</template>
```

## 3. 자동 컴포넌트 등록 (Vite)

```javascript
// vite.config.js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import Components from 'unplugin-vue-components/vite'

export default defineConfig({
  plugins: [
    vue(),
    Components({
      // 컴포넌트를 찾을 디렉토리
      dirs: ['src/components'],
      // 파일 확장자
      extensions: ['vue'],
      // 컴포넌트 이름 규칙
      dts: true
    })
  ]
})
```

## 4. 컴포넌트 이름 규칙

### 케밥 케이스 (권장)
```html
<my-component />
<my-child-component />
```

### 파스칼 케이스
```html
<MyComponent />
<MyChildComponent />
```

## 5. 비동기 컴포넌트 등록

### 기본적인 비동기 컴포넌트
```javascript
const AsyncComp = defineAsyncComponent(() =>
  import('./components/MyComponent.vue')
)
```

### 로딩/에러 상태 처리
```javascript
const AsyncComp = defineAsyncComponent({
  loader: () => import('./components/MyComponent.vue'),
  loadingComponent: LoadingComponent,
  errorComponent: ErrorComponent,
  delay: 200,
  timeout: 3000
})
```

## 주의사항

1. 전역 등록 vs 지역 등록
   - 전역 등록: 모든 컴포넌트에서 사용 가능
   - 지역 등록: 등록한 컴포넌트에서만 사용 가능
   - 가능하면 지역 등록 사용 권장 (번들 크기 최적화)

2. 컴포넌트 이름
   - HTML 요소와 충돌을 피하기 위해 케밥 케이스 사용 권장
   - 파스칼 케이스도 사용 가능하나 일관성 유지 필요

3. 비동기 컴포넌트
   - 큰 컴포넌트나 자주 사용하지 않는 컴포넌트에 적합
   - 초기 로딩 시간 단축에 도움
   - 로딩/에러 상태 처리 고려 필요

4. `<script setup>` 사용 시
   - import한 컴포넌트는 자동으로 등록됨
   - 별도의 components 옵션 설정 불필요 