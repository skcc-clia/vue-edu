# Vue3 템플릿의 JavaScript 표현식

## 1. 기본 표현식

```html
<template>
  <!-- 변수 사용 -->
  <p>{{ message }}</p>
  
  <!-- 연산자 사용 -->
  <p>{{ number + 1 }}</p>
  <p>{{ ok ? 'YES' : 'NO' }}</p>
  
  <!-- 메서드 호출 -->
  <p>{{ message.toUpperCase() }}</p>
  
  <!-- 삼항 연산자 -->
  <p>{{ isActive ? '활성' : '비활성' }}</p>
</template>
```

## 2. 복합 표현식

```html
<template>
  <!-- 여러 연산자 사용 -->
  <p>{{ (a + b) * c }}</p>
  
  <!-- 조건부 연산 -->
  <p>{{ isActive && isVisible ? '보임' : '숨김' }}</p>
  
  <!-- 배열 메서드 -->
  <p>{{ items.filter(item => item.active).length }}</p>
</template>
```

## 3. 제한사항

### 사용할 수 없는 표현식
```html
<!-- 선언문은 사용 불가 -->
{{ var a = 1 }}

<!-- 조건문은 사용 불가 -->
{{ if (ok) { return message } }}

<!-- 반복문은 사용 불가 -->
{{ for (item of items) { return item } }}
```

### 대안
```html
<!-- v-if 디렉티브 사용 -->
<div v-if="ok">{{ message }}</div>

<!-- v-for 디렉티브 사용 -->
<div v-for="item in items" :key="item.id">
  {{ item.name }}
</div>
```

## 4. 계산된 속성 활용

```html
<template>
  <!-- 복잡한 로직은 계산된 속성으로 이동 -->
  <p>{{ fullName }}</p>
  <p>{{ reversedMessage }}</p>
</template>

<script setup>
import { ref, computed } from 'vue'

const firstName = ref('John')
const lastName = ref('Doe')
const message = ref('Hello Vue!')

// 계산된 속성 사용
const fullName = computed(() => {
  return firstName.value + ' ' + lastName.value
})

const reversedMessage = computed(() => {
  return message.value.split('').reverse().join('')
})
</script>
```

## 5. 메서드 활용

```html
<template>
  <!-- 메서드 호출 -->
  <p>{{ formatDate(date) }}</p>
  <p>{{ calculateTotal(items) }}</p>
</template>

<script setup>
import { ref } from 'vue'

const date = ref(new Date())
const items = ref([
  { price: 100 },
  { price: 200 },
  { price: 300 }
])

// 메서드 정의
const formatDate = (date) => {
  return date.toLocaleDateString()
}

const calculateTotal = (items) => {
  return items.reduce((sum, item) => sum + item.price, 0)
}
</script>
```

## 6. 필터 대신 메서드나 계산된 속성 사용

```html
<template>
  <!-- 필터 대신 메서드 사용 -->
  <p>{{ capitalize(message) }}</p>
  
  <!-- 필터 대신 계산된 속성 사용 -->
  <p>{{ capitalizedMessage }}</p>
</template>

<script setup>
import { ref, computed } from 'vue'

const message = ref('hello vue')

// 메서드로 구현
const capitalize = (text) => {
  return text.charAt(0).toUpperCase() + text.slice(1)
}

// 계산된 속성으로 구현
const capitalizedMessage = computed(() => {
  return message.value.charAt(0).toUpperCase() + message.value.slice(1)
})
</script>
```

## 주의사항

1. **단일 표현식만 사용 가능**
   - 템플릿 내에서는 하나의 표현식만 사용할 수 있습니다.
   - 여러 줄의 로직이 필요한 경우 메서드나 계산된 속성을 사용하세요.

2. **성능 고려**
   - 복잡한 표현식은 계산된 속성으로 이동하는 것이 좋습니다.
   - 계산된 속성은 의존성이 변경될 때만 재계산됩니다.

3. **가독성**
   - 템플릿 내의 표현식이 복잡해지면 가독성이 떨어집니다.
   - 복잡한 로직은 스크립트 부분으로 이동하는 것이 좋습니다.

4. **디버깅**
   - 템플릿 내의 표현식에서 오류가 발생하면 디버깅이 어려울 수 있습니다.
   - 복잡한 로직은 메서드나 계산된 속성으로 분리하여 디버깅을 용이하게 하세요. 