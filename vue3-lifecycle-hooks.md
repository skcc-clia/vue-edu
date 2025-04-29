# Vue3 생명주기 훅 비교표

## 생명주기 훅 개요

| 생명주기 훅 | 실행 시점 | 주요 사용 사례 |
|------------|-----------|---------------|
| `onMounted` | 컴포넌트가 DOM에 마운트된 직후 | - API 호출을 통한 초기 데이터 로딩<br>- DOM 요소 접근 작업<br>- 외부 라이브러리 초기화<br>- 이벤트 리스너 등록 |
| `onUpdated` | 반응형 데이터 변경으로 DOM 업데이트 후 | - DOM 업데이트 후 추가 작업<br>- props 변경 후 처리<br>- 외부 라이브러리 업데이트 |
| `onUnmounted` | 컴포넌트가 DOM에서 제거되기 직전 | - 이벤트 리스너 제거<br>- 타이머/인터벌 정리<br>- 외부 라이브러리 인스턴스 정리<br>- 메모리 누수 방지 |
| `onBeforeMount` | 컴포넌트가 DOM에 마운트되기 직전 | - 마운트 전 데이터 초기화<br>- 마운트 전 상태 설정 |
| `onBeforeUpdate` | DOM 업데이트 직전 | - DOM 업데이트 전 상태 저장<br>- 업데이트 전 데이터 백업 |
| `onBeforeUnmount` | 컴포넌트 제거 직전 | - 컴포넌트 제거 전 정리 작업<br>- 부모 컴포넌트에 이벤트 발생 |
| `onErrorCaptured` | 자식 컴포넌트 에러 발생 시 | - 에러 로깅<br>- 대체 UI 표시<br>- 에러 복구 로직 |
| `onActivated` | `<keep-alive>` 컴포넌트 재활성화 시 | - 캐시된 컴포넌트 재활성화 작업 |
| `onDeactivated` | `<keep-alive>` 컴포넌트 비활성화 시 | - 컴포넌트 비활성화 시 정리 작업 |

## 사용 예시

```javascript
import { 
  onMounted, 
  onUpdated, 
  onUnmounted,
  onBeforeMount,
  onBeforeUpdate,
  onBeforeUnmount,
  onErrorCaptured,
  onActivated,
  onDeactivated
} from 'vue'

export default {
  setup() {
    // 컴포넌트 마운트 후
    onMounted(() => {
      console.log('컴포넌트가 마운트되었습니다')
    })

    // DOM 업데이트 후
    onUpdated(() => {
      console.log('컴포넌트가 업데이트되었습니다')
    })

    // 컴포넌트 제거 전
    onUnmounted(() => {
      console.log('컴포넌트가 제거됩니다')
    })

    // 에러 처리
    onErrorCaptured((err, instance, info) => {
      console.error('에러가 발생했습니다:', err)
      return false // 에러 전파 중단
    })
  }
}
```

## 주의사항

1. 생명주기 훅은 반드시 `setup()` 함수 내에서 호출되어야 합니다.
2. 비동기 작업은 `onMounted`에서 처리하는 것이 좋습니다.
3. 메모리 누수를 방지하기 위해 `onUnmounted`에서 모든 정리 작업을 수행해야 합니다.
4. `onErrorCaptured`는 에러를 처리한 후 `false`를 반환하면 에러 전파를 중단할 수 있습니다.
5. `<keep-alive>`와 함께 사용할 때는 `onActivated`와 `onDeactivated`를 활용하여 컴포넌트 상태를 관리해야 합니다. 