# JavaScript 核心概念

## 变量和作用域

### var、let、const 区别
```javascript
// var 存在变量提升
console.log(a); // undefined
var a = 1;

// let 块级作用域，不存在提升
// console.log(b); // ReferenceError
let b = 2;

// const 常量，必须初始化
const c = 3;
// c = 4; // TypeError
```

### 闭包详解
> **💡 概念**：函数能够记住并访问其词法作用域，即使函数在其作用域外执行。

```javascript
function createCounter() {
  let count = 0;
  return function() {
    count++;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

**应用场景：**
- 数据私有化
- 函数工厂
- 模块模式

## 异步编程

### Promise 链式调用
```javascript
function fetchData(url) {
  return fetch(url)
    .then(response => {
      if (!response.ok) throw new Error('Network error');
      return response.json();
    })
    .then(data => processData(data))
    .catch(error => {
      console.error('Fetch failed:', error);
      throw error;
    });
}
```

### async/await 最佳实践
```javascript
async function getUserData(userId) {
  try {
    const user = await fetchUser(userId);
    const posts = await fetchUserPosts(userId);
    return { user, posts };
  } catch (error) {
    // 统一错误处理
    Sentry.captureException(error);
    throw new Error('Failed to load user data');
  }
}
```

## 实用代码片段

### 防抖函数
```javascript
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

// 使用示例
const searchInput = document.getElementById('search');
searchInput.addEventListener('input', debounce(handleSearch, 300));
```

| 方法 | 适用场景 | 注意事项 |
|------|----------|----------|
| 防抖 | 搜索框输入 | 连续触发只执行最后一次 |
| 节流 | 滚动事件 | 固定时间间隔执行 |
