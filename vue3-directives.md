# Vue3 디렉티브 요약

## 주요 디렉티브

| 디렉티브 | 설명 | 사용 예시 |
|----------|------|-----------|
| `v-bind` (`:`) | HTML 속성을 동적으로 바인딩 | `:src="imageUrl"` |
| `v-model` | 양방향 데이터 바인딩 | `v-model="name"` |
| `v-on` (`@`) | 이벤트 리스너 등록 | `@click="handleClick"` |
| `v-if` | 조건부 렌더링 | `v-if="isVisible"` |
| `v-show` | CSS display 속성 토글 | `v-show="isVisible"` |
| `v-for` | 리스트 렌더링 | `v-for="item in items"` |
| `v-text` | 텍스트 내용 업데이트 | `v-text="message"` |
| `v-html` | HTML 내용 삽입 | `v-html="htmlContent"` |
| `v-pre` | 컴파일 건너뛰기 | `v-pre` |
| `v-once` | 한 번만 렌더링 | `v-once` |
| `v-cloak` | 초기 렌더링 숨김 | `v-cloak` |

## 주요 디렉티브 상세 설명

### 1. 데이터 바인딩
- `v-bind`: 속성 바인딩
  ```html
  <img :src="imageUrl" />
  <div :class="{ active: isActive }">
  ```

- `v-model`: 양방향 데이터 바인딩
  ```html
  <input v-model="name" />
  <select v-model="selected">
  ```

### 2. 이벤트 처리
- `v-on`: 이벤트 리스너
  ```html
  <button @click="handleClick">
  <form @submit.prevent="onSubmit">
  ```

### 3. 조건부 렌더링
- `v-if`: 조건에 따른 요소 추가/제거
  ```html
  <div v-if="isVisible">보이는 내용</div>
  ```

- `v-show`: CSS display 속성 토글
  ```html
  <div v-show="isVisible">보이는 내용</div>
  ```

### 4. 리스트 렌더링
- `v-for`: 반복 렌더링
  ```html
  <li v-for="(item, index) in items" :key="item.id">
    {{ item.name }}
  </li>
  ```

## 디렉티브 수식어

### 1. 이벤트 수식어
- `.prevent`: `event.preventDefault()`
- `.stop`: `event.stopPropagation()`
- `.capture`: 캡처 단계에서 이벤트 처리
- `.self`: 이벤트가 요소 자체에서 발생했을 때만 처리
- `.once`: 한 번만 이벤트 처리
- `.passive`: 스크롤 성능 향상

### 2. v-model 수식어
- `.lazy`: change 이벤트 후 동기화
- `.number`: 입력값을 숫자로 변환
- `.trim`: 입력값의 앞뒤 공백 제거

## 주의사항

1. `v-if`와 `v-show`의 차이
   - `v-if`: 조건이 false일 때 DOM에서 완전히 제거
   - `v-show`: CSS display: none으로 숨김

2. `v-for` 사용 시
   - 항상 `:key` 바인딩 필요
   - 객체나 배열의 변경을 감지하기 위함

3. `v-html` 사용 시
   - XSS 공격 위험이 있으므로 신뢰할 수 있는 콘텐츠만 사용
   - 가능하면 `v-text` 사용 권장

4. `v-model` 사용 시
   - 컴포넌트에서 사용하려면 `modelValue` prop과 `update:modelValue` 이벤트 구현 필요 