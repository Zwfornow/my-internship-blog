# Vue 3 实战指南

## Composition API 核心

### setup() 函数
```vue
<script setup>
import { ref, reactive, computed, onMounted } from 'vue'

// 响应式数据
const count = ref(0)
const user = reactive({
  name: '张三',
  age: 25
})

// 计算属性
const doubleCount = computed(() => count.value * 2)

// 方法
const increment = () => {
  count.value++
}

// 生命周期
onMounted(() => {
  console.log('组件已挂载')
})
</script>

<template>
  <div>
    <p>{{ count }} * 2 = {{ doubleCount }}</p>
    <button @click="increment">增加</button>
  </div>
</template>
```

### 组合式函数
```javascript
// composables/useCounter.js
import { ref, computed } from 'vue'

export function useCounter(initialValue = 0) {
  const count = ref(initialValue)
  
  const double = computed(() => count.value * 2)
  const increment = () => count.value++
  const decrement = () => count.value--
  const reset = () => count.value = initialValue
  
  return {
    count,
    double,
    increment,
    decrement,
    reset
  }
}

// 在组件中使用
<script setup>
import { useCounter } from '@/composables/useCounter'

const { count, increment, double } = useCounter(10)
</script>
```

## 高级特性

### 自定义指令
```javascript
// 自定义 v-focus 指令
const vFocus = {
  mounted: (el) => {
    el.focus()
  }
}

// 自定义 v-longpress 长按指令
const vLongpress = {
  mounted(el, binding) {
    let timer = null
    const handler = () => binding.value()
    
    const start = (e) => {
      if (e.button !== 0) return
      timer = setTimeout(handler, 1000)
    }
    
    const cancel = () => {
      if (timer) {
        clearTimeout(timer)
        timer = null
      }
    }
    
    el.addEventListener('mousedown', start)
    el.addEventListener('touchstart', start)
    el.addEventListener('mouseup', cancel)
    el.addEventListener('touchend', cancel)
  }
}
```

### 状态管理 (Pinia)
```javascript
// stores/counter.js
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0,
    history: []
  }),
  
  getters: {
    doubleCount: (state) => state.count * 2,
    recentHistory: (state) => state.history.slice(-5)
  },
  
  actions: {
    increment() {
      this.count++
      this.history.push(`incremented to ${this.count}`)
    },
    
    decrement() {
      this.count--
      this.history.push(`decremented to ${this.count}`)
    }
  }
})

// 在组件中使用
<script setup>
import { useCounterStore } from '@/stores/counter'

const counter = useCounterStore()
</script>

<template>
  <div>
    <p>{{ counter.count }}</p>
    <p>历史: {{ counter.recentHistory }}</p>
    <button @click="counter.increment()">+</button>
  </div>
</template>
```

## 性能优化

### 组件懒加载
```vue
<script setup>
import { defineAsyncComponent } from 'vue'

// 异步组件
const AsyncComponent = defineAsyncComponent(() =>
  import('./AsyncComponent.vue')
)

// 带加载状态的异步组件
const AsyncWithLoading = defineAsyncComponent({
  loader: () => import('./HeavyComponent.vue'),
  loadingComponent: LoadingSpinner,
  delay: 200,
  timeout: 3000
})
</script>
```

### 虚拟滚动
```vue
<template>
  <RecycleScroller
    :items="largeList"
    :item-size="50"
    key-field="id"
    v-slot="{ item }"
  >
    <div class="item">
      {{ item.name }}
    </div>
  </RecycleScroller>
</template>
```

## 实战技巧

### 表单验证
```vue
<script setup>
import { reactive, computed } from 'vue'

const form = reactive({
  email: '',
  password: '',
  confirmPassword: ''
})

const errors = computed(() => {
  const err = {}
  
  if (!form.email) {
    err.email = '邮箱不能为空'
  } else if (!/^\S+@\S+\.\S+$/.test(form.email)) {
    err.email = '邮箱格式不正确'
  }
  
  if (form.password.length < 6) {
    err.password = '密码至少6位'
  }
  
  if (form.password !== form.confirmPassword) {
    err.confirmPassword = '两次密码不一致'
  }
  
  return err
})

const isValid = computed(() => Object.keys(errors.value).length === 0)
</script>
```

| 特性 | Vue 2 | Vue 3 | 优势 |
|------|-------|-------|------|
| 响应式系统 | Object.defineProperty | Proxy | 更好的性能，支持数组索引 |
| 组合API | Options API | Composition API | 更好的逻辑复用 |
| 类型支持 | 一般 | 优秀 | 完整的 TypeScript 支持 |