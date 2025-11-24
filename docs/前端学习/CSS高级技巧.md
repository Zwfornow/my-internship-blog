# CSS 高级技巧与实战

## 现代布局技术

### Grid 网格布局
```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  grid-gap: 20px;
  grid-template-areas: 
    "header header"
    "sidebar content"
    "footer footer";
}

.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.content { grid-area: content; }
.footer { grid-area: footer; }

/* 响应式网格 */
@media (max-width: 768px) {
  .container {
    grid-template-areas: 
      "header"
      "content"
      "sidebar"
      "footer";
    grid-template-columns: 1fr;
  }
}
```

### Flexbox 高级技巧
```css
/* 完美居中 */
.center {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

/* 等分布局 */
.equal-columns {
  display: flex;
}

.equal-columns > * {
  flex: 1;
  min-width: 0; /* 防止内容溢出 */
}

/* 圣杯布局 */
.holy-grail {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.holy-grail main {
  flex: 1;
  display: flex;
}

.holy-grail .content {
  flex: 1;
}
```

## 动画与交互

### 关键帧动画
```css
@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateX(-100%);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes bounce {
  0%, 20%, 53%, 80%, 100% {
    transform: translate3d(0, 0, 0);
  }
  40%, 43% {
    transform: translate3d(0, -30px, 0);
  }
  70% {
    transform: translate3d(0, -15px, 0);
  }
  90% {
    transform: translate3d(0, -4px, 0);
  }
}

.animated {
  animation-duration: 1s;
  animation-fill-mode: both;
}

.slide-in {
  animation-name: slideIn;
}

.bounce {
  animation-name: bounce;
  transform-origin: center bottom;
}
```

### 悬停效果
```css
/* 3D 卡片效果 */
.card {
  perspective: 1000px;
}

.card-inner {
  transition: transform 0.6s;
  transform-style: preserve-3d;
}

.card:hover .card-inner {
  transform: rotateY(180deg);
}

.card-front, .card-back {
  backface-visibility: hidden;
}

.card-back {
  transform: rotateY(180deg);
}

/* 渐变边框 */
.gradient-border {
  position: relative;
  background: white;
  padding: 20px;
  border-radius: 10px;
}

.gradient-border::before {
  content: '';
  position: absolute;
  top: -2px;
  left: -2px;
  right: -2px;
  bottom: -2px;
  background: linear-gradient(45deg, #ff6b6b, #4ecdc4, #45b7d1);
  border-radius: 12px;
  z-index: -1;
}
```

## 响应式设计

### CSS 自定义属性
```css
:root {
  --primary-color: #007bff;
  --secondary-color: #6c757d;
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.25rem;
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --border-radius: 0.375rem;
}

.btn {
  padding: var(--spacing-sm) var(--spacing-md);
  font-size: var(--font-size-base);
  border-radius: var(--border-radius);
  background-color: var(--primary-color);
}

/* 暗色主题 */
[data-theme="dark"] {
  --primary-color: #0d6efd;
  --bg-color: #1a1a1a;
  --text-color: #ffffff;
}
```

### 容器查询
```css
.component {
  container-type: inline-size;
}

@container (min-width: 400px) {
  .card {
    display: flex;
    gap: 1rem;
  }
  
  .card img {
    width: 150px;
    height: 150px;
    object-fit: cover;
  }
}

@container (min-width: 600px) {
  .card {
    padding: 2rem;
  }
}
```

## 性能优化

### 内容可见性
```css
.long-list {
  content-visibility: auto;
  contain-intrinsic-size: 0 500px; /* 预估高度 */
}

/* 减少重绘 */
.stable-element {
  transform: translateZ(0);
  will-change: transform;
}

/* 图片优化 */
.optimized-image {
  image-rendering: -webkit-optimize-contrast;
  image-rendering: crisp-edges;
}
```

### 实用工具类
```css
/* 文本截断 */
.truncate {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* 滚动条样式 */
.custom-scrollbar {
  scrollbar-width: thin;
  scrollbar-color: #cbd5e0 #f7fafc;
}

.custom-scrollbar::-webkit-scrollbar {
  width: 8px;
}

.custom-scrollbar::-webkit-scrollbar-track {
  background: #f7fafc;
}

.custom-scrollbar::-webkit-scrollbar-thumb {
  background: #cbd5e0;
  border-radius: 4px;
}
```

## 现代特性

### 子网格
```css
.grid-container {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
  gap: 20px;
}

.grid-item {
  display: grid;
  grid-template-columns: subgrid;
  grid-column: 1 / 4;
}
```

### 层叠层
```css
@layer base, components, utilities;

@layer base {
  h1 { font-size: 2rem; }
  p { margin-bottom: 1rem; }
}

@layer components {
  .btn { padding: 0.5rem 1rem; }
}

@layer utilities {
  .text-center { text-align: center; }
}
```