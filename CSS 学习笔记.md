# CSS 学习笔记

## 一、CSS 引入方式

### 三种引入方式

1. **行内样式**：写在标签的 `style` 属性中（配合 JavaScript 使用）
   ```html
   <span style="color: red; font-size: 16px;">文本内容</span>
   ```

2. **内部样式**：写在 `<style>` 标签中（通常约定写在 `<head>` 标签中）
   ```html
   <head>
     <style>
       span { color: blue; }
     </style>
   </head>
   ```

3. **外部样式**：写在单独的 `.css` 文件中，通过 `<link>` 标签引入
   ```css
   /* style.css */
   span { color: green; }
   ```
   ```html
   <head>
     <link rel="stylesheet" href="./style.css">
   </head>
   ```

> [!important] 权重优先级
> 行内样式 > 内部样式 / 外部样式（后写的覆盖先写的）

> [!tip] 开发推荐
> - 实际项目统一使用**外部样式**
> - 行内样式仅 JS 动态修改时临时使用
> - 内部样式仅做简单 demo 测试

---

## 二、CSS 选择器

### 2.1 基本选择器

| 选择器 | 写法 | 示例 | 示例说明 |
|--------|------|------|----------|
| 元素选择器 | `元素名称 { ... }` | `h1 { ... }` | 选择所有 `<h1>` 标签 |
| 类选择器 | `.class值 { ... }` | `.cls { ... }` | 选择 class 为 cls 的标签 |
| ID 选择器 | `#id值 { ... }` | `#hid { ... }` | 选择 id 为 hid 的标签 |
| 通配符选择器 | `* { ... }` | `* { ... }` | 选择页面上**所有**元素 |

```css
/* 示例 */
p { color: gray; }        /* 元素选择器 */
.highlight { background: yellow; }  /* 类选择器 */
#title { font-size: 24px; }        /* ID 选择器 */
* { margin: 0; }          /* 通配符：清除所有元素的外边距 */
```

### 2.2 复合选择器

| 选择器 | 写法 | 示例 | 示例说明 |
|--------|------|------|----------|
| 分组选择器 | `选择器1, 选择器2 { ... }` | `h1, h2 { ... }` | 同时选择 h1 和 h2 |
| 后代选择器 | `父 子 { ... }` | `div p { ... }` | div 里**所有** p（包含孙子） |
| 子代选择器 | `父 > 子 { ... }` | `div > p { ... }` | div 的**直接**子元素 p |
| 相邻兄弟 | `元素 + 兄弟 { ... }` | `h1 + p { ... }` | 紧跟在 h1 后面的第一个 p |
| 属性选择器 | `[属性] { ... }` | `input[type="text"] { ... }` | type 为 text 的 input |

```css
/* 后代 vs 子代 */
div p { color: red; }     /* div 里面所有的 p 都变红 */
div > p { color: blue; }  /* 只有 div 直接儿子 p 变蓝，孙子不变 */
```

> [!note] 后代 vs 子代的区别
> 后代选择器选**所有层级**的后代，子代选择器只选**直接下一级**的子元素。就像"家族所有人"和"自己孩子"的区别。

### 2.3 伪类选择器

伪类用来描述元素的**特殊状态**，比如鼠标悬停、被点击等。

```css
/* 鼠标悬停变色 */
a:hover { color: red; }

/* 获得焦点时（输入框被点击时） */
input:focus { border-color: blue; }

/* 列表第一个和最后一个 */
li:first-child { font-weight: bold; }
li:last-child { color: gray; }

/* 第 n 个元素 */
li:nth-child(2) { background: #eee; }       /* 第2个 */
li:nth-child(odd) { background: #f5f5f5; }  /* 奇数行 */
li:nth-child(even) { background: #fff; }    /* 偶数行 */
```

### 2.4 伪元素选择器

伪元素用于给元素的**特定部分**添加样式，不需要额外 HTML。

```css
/* 在元素内容前插入内容 */
p::before { content: "📌 "; }

/* 在元素内容后插入内容 */
p::after { content: " ✅"; }

/* 选中第一行文字 */
p::first-line { font-weight: bold; }

/* 选中文字（配合 ::selection） */
::selection { background: yellow; color: black; }
```

> [!tip] 伪类 vs 伪元素
> - 伪类用**一个冒号** `:hover`，描述元素的**状态**
> - 伪元素用**两个冒号** `::before`，描述元素的**部分**

---

## 三、CSS 权重（层叠与优先级）

当多条规则作用于同一个元素时，谁的"权重"大，谁就生效。

| 选择器类型 | 权重 | 示例 |
|-----------|------|------|
| `!important` | 最高（尽量别用） | `color: red !important;` |
| 行内样式 | 1000 | `style="color:red"` |
| ID 选择器 | 100 | `#title { }` |
| 类 / 伪类 / 属性选择器 | 10 | `.cls { }`、`:hover { }` |
| 元素 / 伪元素选择器 | 1 | `p { }`、`::before { }` |
| 通配符 `*` | 0 | `* { }` |

```css
/* 权重比较 */
p { color: red; }              /* 权重 1 */
.text { color: blue; }         /* 权重 10 → 蓝色胜出 */
#main { color: green; }        /* 权重 100 → 绿色胜出 */
```

> [!warning] 关于 !important
> `!important` 会覆盖一切，但会让代码难以维护。**面试常问，开发少用。**

---

## 四、CSS 颜色

CSS 支持多种颜色写法：

```css
p {
  color: red;                    /* 颜色关键字 */
  color: #ff6600;                /* 十六进制（最常用） */
  color: rgb(255, 102, 0);      /* RGB */
  color: rgba(255, 102, 0, 0.5); /* RGBA（带透明度） */
  color: hsl(25, 100%, 50%);    /* HSL（色相/饱和度/亮度） */
}
```

| 写法 | 说明 | 适用场景 |
|------|------|----------|
| 关键字 | `red`、`blue`、`transparent` | 快速测试 |
| 十六进制 | `#ff6600`，可简写为 `#f60` | **开发首选** |
| RGB | `rgb(255, 102, 0)` | 需要精确数值时 |
| RGBA | `rgba(255, 102, 0, 0.5)` | 需要半透明效果 |
| HSL | `hsl(25, 100%, 50%)` | 调整颜色方便 |

> [!tip] 小技巧
> 十六进制颜色简写：`#ff6600` 可以写成 `#f60`，每两位相同时可以省略一半。

---

## 五、字体与文本

### 5.1 字体属性

```css
p {
  font-size: 16px;                          /* 字体大小 */
  font-weight: bold;                        /* 字体粗细：bold / normal / 100~900 */
  font-style: italic;                       /* 字体风格：斜体 */
  font-family: "Microsoft YaHei", Arial;    /* 字体族：多个用逗号分隔，逐个尝试 */
  font: italic bold 16px "Microsoft YaHei"; /* 简写（顺序：style weight size family） */
}
```

### 5.2 文本属性

```css
p {
  color: #333;              /* 文字颜色 */
  text-align: center;       /* 文本水平对齐：left / center / right */
  text-decoration: none;    /* 文本装饰：none / underline / line-through（删除线） */
  text-indent: 2em;         /* 首行缩进（2em = 2个字的宽度） */
  text-transform: uppercase;/* 大小写转换：uppercase / lowercase / capitalize */
  letter-spacing: 2px;      /* 字间距 */
  line-height: 1.5;         /* 行高（1.5倍行距，最常用的行高写法） */
  word-spacing: 4px;        /* 词间距 */
}
```

> [!tip] 行高技巧
> 设置 `line-height` 等于元素的 `height`，可以实现**单行文字垂直居中**：
> ```css
> .btn { height: 40px; line-height: 40px; }
> ```

### 5.3 文字溢出处理

```css
/* 单行文本溢出显示省略号 */
.single-line {
  white-space: nowrap;      /* 不换行 */
  overflow: hidden;         /* 超出隐藏 */
  text-overflow: ellipsis;  /* 显示省略号 */
}

/* 多行文本溢出显示省略号（Webkit 浏览器） */
.multi-line {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;    /* 最多显示3行 */
  overflow: hidden;
}
```

---

## 六、盒模型（Box Model）

> [!important] 核心概念
> CSS 中每个元素都可以看成一个**盒子**，由四层组成：`content` → `padding` → `border` → `margin`（从内到外）。

```
┌─────────────────────────── margin ──────────────────────────┐
│  ┌──────────────────────── border ───────────────────────┐  │
│  │  ┌───────────────────── padding ───────────────────┐  │  │
│  │  │  ┌────────────────── content ────────────────┐  │  │  │
│  │  │  │                                          │  │  │  │
│  │  │  │          实际显示的内容                    │  │  │  │
│  │  │  │                                          │  │  │  │
│  │  │  └──────────────────────────────────────────┘  │  │  │
│  │  └───────────────────────────────────────────────┘  │  │
│  └─────────────────────────────────────────────────────┘  │
└───────────────────────────────────────────────────────────┘
```

### 6.1 内边距 padding

```css
.box {
  padding: 20px;                 /* 四个方向都是 20px */
  padding: 10px 20px;            /* 上下 10px，左右 20px */
  padding: 10px 20px 30px 40px;  /* 上 右 下 左（顺时针） */
  padding-top: 10px;             /* 单独设置某一边 */
}
```

### 6.2 外边距 margin

```css
.box {
  margin: 20px;                  /* 四个方向都是 20px */
  margin: 0 auto;                /* 水平居中（固定写法） */
  margin-bottom: 20px;           /* 下外边距 */
}
```

> [!warning] margin 塌陷问题
> 垂直方向上，两个相邻元素的 margin 会**合并**，取较大的值而不是相加。
> ```css
> .box1 { margin-bottom: 30px; }
> .box2 { margin-top: 20px; }
> /* 最终间距是 30px，不是 50px */
> ```

### 6.3 边框 border

```css
.box {
  border: 1px solid #ccc;         /* 简写：宽度 样式 颜色 */
  border-style: solid;             /* 实线（dashed=虚线，dotted=点线） */
  border-radius: 10px;             /* 圆角 */
  border-radius: 50%;              /* 正圆（配合宽高相等） */
}
```

### 6.4 box-sizing

```css
/* 默认模式：width 只包含 content */
.box { box-sizing: content-box; width: 200px; padding: 20px; }
/* 实际总宽度 = 200 + 20*2 + border*2 = 240+ */

/* 推荐模式：width 包含 content + padding + border */
.box { box-sizing: border-box; width: 200px; padding: 20px; }
/* 实际总宽度 = 200（padding 已包含在内） */
```

> [!tip] 推荐设置
> 开发中通常全局设置 `border-box`，这样计算宽度更直观：
> ```css
> * { box-sizing: border-box; }
> ```

---

## 七、背景属性

```css
.box {
  background-color: #f5f5f5;                    /* 背景颜色 */
  background-image: url("bg.png");              /* 背景图片 */
  background-repeat: no-repeat;                 /* 是否平铺：repeat / no-repeat */
  background-size: cover;                       /* 尺寸：cover(铺满) / contain(完整显示) */
  background-position: center center;           /* 位置：水平 垂直 */
  background-attachment: fixed;                 /* 固定不动（视差滚动效果） */

  /* 简写（推荐） */
  background: #f5f5f5 url("bg.png") no-repeat center/cover;
}
```

| background-size 值 | 效果 |
|--------------------|------|
| `cover` | 图片铺满容器，可能裁切（常用作大图背景） |
| `contain` | 图片完整显示，可能留白 |

---

## 八、display 显示模式

HTML 元素分为三种显示模式：

| 模式 | 特点 | 常见元素 |
|------|------|----------|
| `block`（块级） | 独占一行，可设置宽高 | `div`、`p`、`h1~h6`、`ul`、`li` |
| `inline`（行内） | 不换行，**不能**设置宽高 | `span`、`a`、`em`、`strong` |
| `inline-block`（行内块） | 不换行，**可以**设置宽高 | `img`、`input`、`button` |

```css
/* 转换显示模式 */
span { display: block; }          /* 行内 → 块级 */
div { display: inline; }          /* 块级 → 行内 */
div { display: inline-block; }    /* 块级 → 行内块 */
div { display: none; }            /* 隐藏元素（不占空间） */
```

> [!note] display: none vs visibility: hidden
> - `display: none` — 完全消失，不占位置
> - `visibility: hidden` — 看不见，但**还占着位置**

---

## 九、浮动 float

浮动让元素脱离文档流，向左或向右排列。

```css
.left { float: left; }    /* 向左浮动 */
.right { float: right; }  /* 向右浮动 */
```

### 9.1 清除浮动

浮动会导致父元素高度塌陷，需要清除：

```css
/* 方法1：父元素添加 overflow */
.parent { overflow: hidden; }

/* 方法2：伪元素清除（推荐） */
.parent::after {
  content: "";
  display: block;
  clear: both;
}

/* 方法3：给新元素添加 clear */
.clear { clear: both; }
```

> [!tip] 现代开发建议
> 浮动主要用于**文字环绕图片**。布局推荐使用 **Flexbox** 或 **Grid**，更简单可控。

---

## 十、定位 position

| 定位值 | 特点 | 是否脱离文档流 |
|--------|------|---------------|
| `static` | 默认，没有定位 | 否 |
| `relative` | 相对**自身原来位置**偏移 | 否（原位置仍占着） |
| `absolute` | 相对**最近的定位祖先**定位 | 是 |
| `fixed` | 相对**浏览器窗口**定位 | 是 |
| `sticky` | 正常流，滚动到阈值后粘住 | 否 |

```css
/* relative：相对自己原来的位置偏移 */
.box { position: relative; top: 10px; left: 20px; }

/* absolute：找最近的定位祖先来定位 */
.parent { position: relative; }   /* 父元素设为参照物 */
.child { position: absolute; top: 0; right: 0; }

/* fixed：固定在屏幕某位置（常见：返回顶部按钮） */
.back-top { position: fixed; bottom: 50px; right: 50px; }

/* sticky：滚动到某位置后粘住（常见：粘性导航栏） */
.nav { position: sticky; top: 0; }
```

> [!tip] 水平垂直居中（万能方案）
> ```css
> .parent { position: relative; }
> .child {
>   position: absolute;
>   top: 50%;
>   left: 50%;
>   transform: translate(-50%, -50%);
> }
> ```

---

## 十一、Flex 弹性布局

> [!important] 最常用的布局方案
> Flex 布局是目前开发中**使用最多**的布局方式，用来解决元素的排列和对齐问题。

### 11.1 开启 Flex

```css
/* 在父元素上设置 display: flex */
.container {
  display: flex;
  /* 父元素变成"弹性容器"，子元素变成"弹性项目" */
}
```

### 11.2 容器属性（设置在父元素上）

```css
.container {
  display: flex;

  /* 主轴方向（子元素排列方向） */
  flex-direction: row;            /* 水平从左到右（默认） */
  /* flex-direction: column;     /* 垂直从上到下 */ */

  /* 主轴对齐方式 */
  justify-content: center;        /* 居中 */
  /* flex-start / flex-end / space-between / space-around / space-evenly */

  /* 交叉轴对齐方式（单行） */
  align-items: center;            /* 居中 */
  /* flex-start / flex-end / stretch / baseline */

  /* 是否换行 */
  flex-wrap: wrap;                /* 换行 */
  /* nowrap（默认，不换行） */

  /* 多行对齐 */
  align-content: center;
}
```

### 11.3 项目属性（设置在子元素上）

```css
.item {
  flex: 1;           /* 占据剩余空间的份数（最常用） */
  flex-grow: 1;      /* 放大比例 */
  flex-shrink: 0;    /* 缩小比例（0=不缩小） */
  flex-basis: 200px; /* 基础大小 */
  order: -1;         /* 排列顺序（越小越靠前） */
  align-self: center; /* 单独设置自己的交叉轴对齐 */
}
```

### 11.4 常见布局示例

```css
/* 水平 + 垂直居中 */
.center-box {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

/* 两端对齐 */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* 三栏等分 */
.row {
  display: flex;
}
.col { flex: 1; }

/* 底部固定 */
.page {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
.main { flex: 1; }   /* 主内容区自动撑满 */
.footer { }           /* 底部自然在最下方 */
```

> [!tip] 记忆口诀
> **主轴用 `justify`，交叉轴用 `align`**。容器上用 `align-items`，单项覆盖用 `align-self`。

---

## 十二、Grid 网格布局

Grid 是二维布局，可以同时控制行和列，适合复杂的页面整体布局。

### 12.1 开启 Grid

```css
.container {
  display: grid;

  /* 定义列（3列等宽） */
  grid-template-columns: 1fr 1fr 1fr;
  /* 简写：grid-template-columns: repeat(3, 1fr); */

  /* 定义行 */
  grid-template-rows: 100px auto 100px;

  /* 间距 */
  gap: 20px;            /* 行间距和列间距都是 20px */
  /* row-gap: 20px;     /* 行间距 */ */
  /* column-gap: 20px;  /* 列间距 */ */
}
```

### 12.2 常用单位

| 单位 | 说明 | 示例 |
|------|------|------|
| `px` | 固定像素 | `200px` |
| `%` | 百分比 | `50%` |
| `fr` | 占剩余空间的份数（Grid 专属） | `1fr 2fr`（1:2 分配） |
| `auto` | 自动适应 | `auto` |
| `repeat()` | 重复 | `repeat(3, 1fr)` |

### 12.3 项目定位

```css
.item {
  /* 指定放在哪（从第1条线开始） */
  grid-column: 1 / 3;    /* 从第1列到第3列（跨2列） */
  grid-row: 1 / 2;       /* 从第1行到第2行 */
}

/* 简写：span 关键字 */
.item {
  grid-column: span 2;   /* 占2列 */
  grid-row: span 3;      /* 占3行 */
}
```

### 12.4 经典布局示例

```css
/* 圣杯布局（头+侧栏+主内容+底部） */
.layout {
  display: grid;
  grid-template-areas:
    "header header header"
    "sidebar main main"
    "footer footer footer";
  grid-template-columns: 200px 1fr 1fr;
  grid-template-rows: 60px 1fr 60px;
  min-height: 100vh;
}
.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main    { grid-area: main; }
.footer  { grid-area: footer; }
```

```css
/* 响应式网格卡片（自动填充） */
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
}
```

> [!tip] Flex vs Grid 怎么选？
> - **一维布局**（一行或一列）→ 用 **Flex**
> - **二维布局**（行+列同时控制）→ 用 **Grid**
> - 实际开发中两者经常**配合使用**

---

## 十三、CSS 过渡 transition

让属性变化有一个**平滑的过渡效果**，而不是瞬间切换。

```css
.btn {
  background-color: blue;
  color: white;
  /* transition: 要过渡的属性 时长 速度曲线 延迟 */
  transition: all 0.3s ease;
  /* all = 所有属性都过渡 */
}

.btn:hover {
  background-color: red;
  transform: scale(1.1);  /* 鼠标悬停时放大 + 变色，有过渡动画 */
}
```

| 速度曲线值 | 效果 |
|-----------|------|
| `ease` | 默认，先慢后快再慢 |
| `linear` | 匀速 |
| `ease-in` | 慢开始 |
| `ease-out` | 慢结束 |
| `ease-in-out` | 慢开始慢结束 |

```css
/* 分别设置不同属性的过渡 */
.card {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 20px rgba(0,0,0,0.15);
}
```

---

## 十四、CSS 动画 animation

比过渡更强大，可以定义**多个关键帧**，实现复杂动画。

### 14.1 定义动画

```css
@keyframes slideIn {
  from {
    transform: translateX(-100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

/* 或者用百分比定义多个阶段 */
@keyframes bounce {
  0%   { transform: translateY(0); }
  50%  { transform: translateY(-20px); }
  100% { transform: translateY(0); }
}
```

### 14.2 使用动画

```css
.box {
  /* animation: 动画名 时长 速度曲线 延迟 次数 方向 填充模式 */
  animation: slideIn 1s ease forwards;
  /* infinite = 无限循环 */
  /* alternate = 来回播放 */
  /* forwards = 动画结束后保持最终状态 */
}

/* 简写 */
.box {
  animation: bounce 0.6s ease-in-out infinite;
}
```

> [!note] transition vs animation
> - **transition**：需要触发（如 hover），只有两个状态（开始→结束）
> - **animation**：可以自动播放，支持多个关键帧，更灵活

---

## 十五、CSS 变量（自定义属性）

CSS 变量可以定义可复用的值，方便统一管理和主题切换。

```css
/* 在 :root 中定义全局变量 */
:root {
  --primary-color: #1890ff;
  --font-size-base: 16px;
  --spacing: 16px;
  --border-radius: 8px;
}

/* 使用变量 */
.btn {
  background-color: var(--primary-color);
  font-size: var(--font-size-base);
  padding: var(--spacing);
  border-radius: var(--border-radius);
}

/* 覆盖变量（局部） */
.card {
  --primary-color: #52c41a;  /* 这个卡片里用绿色 */
  border: 1px solid var(--primary-color);
}
```

> [!tip] 实际应用场景
> CSS 变量非常适合做**主题切换**（如暗黑模式）：
> ```css
> :root { --bg-color: #fff; --text-color: #333; }
> [data-theme="dark"] { --bg-color: #1a1a1a; --text-color: #e0e0e0; }
> body { background: var(--bg-color); color: var(--text-color); }
> ```

---

## 十六、响应式设计

让网页在不同屏幕尺寸下都能良好显示。

### 16.1 媒体查询

```css
/* 手机（小于768px） */
@media (max-width: 768px) {
  .sidebar { display: none; }
  .main { width: 100%; }
}

/* 平板（768px ~ 1024px） */
@media (min-width: 768px) and (max-width: 1024px) {
  .sidebar { width: 200px; }
}

/* 桌面（大于1024px） */
@media (min-width: 1024px) {
  .container { max-width: 1200px; margin: 0 auto; }
}
```

### 16.2 响应式单位

```css
html { font-size: 16px; }

.responsive {
  font-size: 1.2rem;     /* 相对于根元素字体大小（推荐） */
  width: 80vw;            /* 视口宽度的 80% */
  height: 50vh;           /* 视口高度的 50% */
  max-width: 1200px;      /* 最大宽度限制 */
}
```

| 单位 | 说明 |
|------|------|
| `rem` | 相对于根元素（`<html>`）字体大小 |
| `em` | 相对于父元素字体大小 |
| `vw` | 视口宽度的 1% |
| `vh` | 视口高度的 1% |
| `vmin` | vw 和 vh 中较小的那个 |
| `vmax` | vw 和 vh 中较大的那个 |

### 16.3 响应式图片

```css
img {
  max-width: 100%;   /* 图片不会超过父容器宽度 */
  height: auto;      /* 高度自动等比缩放 */
}
```

---

## 十七、CSS 2D / 3D 变形 transform

### 17.1 2D 变形

```css
.box {
  transform: translate(50px, 100px);  /* 平移 */
  transform: rotate(45deg);           /* 旋转 */
  transform: scale(1.5);              /* 放大1.5倍 */
  transform: scale(0.8, 1.2);         /* 水平缩小，垂直放大 */
  transform: skew(10deg);             /* 倾斜 */

  /* 组合使用（顺序有影响） */
  transform: translate(50px, 0) rotate(45deg) scale(1.2);
}

/* transform-origin：改变变形原点（默认中心） */
.box {
  transform-origin: left top;   /* 左上角为原点 */
  transform: rotate(45deg);
}
```

### 17.2 3D 变形

```css
.box {
  transform: perspective(500px) rotateY(30deg);  /* 透视 + Y轴旋转 */
  transform: rotateX(45deg);                      /* X轴旋转（翻转） */
}
```

---

## 十八、常用 CSS 技巧

### 18.1 清除默认样式

```css
/* 简单重置 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

a { text-decoration: none; color: inherit; }
ul, ol { list-style: none; }
```

### 18.2 三角形（纯 CSS）

```css
.triangle {
  width: 0;
  height: 0;
  border: 10px solid transparent;
  border-top-color: red;    /* 向下的红色三角形 */
}
```

### 18.3 隐藏元素

| 方式 | 是否占位 | 是否可访问 |
|------|---------|-----------|
| `display: none` | 否 | 否 |
| `visibility: hidden` | 是 | 否 |
| `opacity: 0` | 是 | 是（屏幕阅读器可读） |
| `position: absolute; left: -9999px;` | 否 | 是 |
| `clip-path: inset(50%)` | 是 | 是 |

### 18.4 滚动条美化

```css
/* 自定义滚动条（Webkit） */
::-webkit-scrollbar { width: 8px; }
::-webkit-scrollbar-track { background: #f1f1f1; }
::-webkit-scrollbar-thumb { background: #888; border-radius: 4px; }
::-webkit-scrollbar-thumb:hover { background: #555; }
```

### 18.5 毛玻璃效果

```css
.glass {
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.3);
}
```

---

## 十九、BEM 命名规范

BEM 是一种 CSS 类名的命名方法，让代码更清晰可维护。

```
Block（块）     →  .card
Element（元素） →  .card__title
Modifier（修饰）→  .card--highlight
```

```html
<div class="card card--highlight">
  <h2 class="card__title">标题</h2>
  <p class="card__content">内容</p>
  <button class="card__btn card__btn--primary">按钮</button>
</div>
```

> [!tip] 命名规则总结
> - 块和元素之间用 **双下划线** `__`
> - 块/元素和修饰之间用 **双横线** `--`
> - 单词之间用 **单横线** `-`

---

## 二十、总结速查表

| 知识点 | 关键属性 | 核心要点 |
|--------|---------|---------|
| 选择器 | `#id`、`.class`、`元素` | 权重：ID > 类 > 元素 |
| 盒模型 | `padding`、`margin`、`border` | `box-sizing: border-box` |
| 浮动 | `float: left/right` | 用于文字环绕，布局用 Flex |
| 定位 | `position` | `relative`+`absolute` 最常用 |
| Flex | `display: flex` | 一维布局首选，主轴 `justify`，交叉轴 `align` |
| Grid | `display: grid` | 二维布局，`fr` 单位，`grid-area` |
| 过渡 | `transition` | 需触发，两状态过渡 |
| 动画 | `@keyframes` + `animation` | 自动播放，多关键帧 |
| 变量 | `--变量名` + `var()` | 主题切换神器 |
| 响应式 | `@media` | 移动端优先 or 桌面端优先 |
| 变形 | `transform` | 平移/旋转/缩放/倾斜 |
