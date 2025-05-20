# TailwindCSS基本使用


# Taiwindcss

- 用于 react 的 application ui 组件库

### Installation

#### 不使用 React

创建 tailwind.config.js file 和 download

```bash
npm install -D tailwindcss
npx tailwindcss init
```

在配置文件中添加模板文件路径

```bash
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{html,js}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

在主 css 文件添加@taiwind 指令

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

完成，可以在 html 中使用 taiwind 添加 class 类名了

#### 如果使用 Vite 驱动框架：

```bash
npm install tailwindcss@next @tailwindcss/vite@next
```

在 vite.config.ts：

```ts
import { defineConfig } from "vite";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  plugins: [tailwindcss()],
});
```

导入主 css 文件：

```css
@import "tailwindcss";
```

#### 使用 React

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

在 css 中：

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### bg color

不同灰度级的背景色调色盘

**css 类**:bg-black/white/slate(偏蓝灰)-(100-950),gray, zinc(纯灰),neutral, stone,red,orange,amber,lime....

设置**不透明度**：e.g. bg-sky-500**/100,75,50**

其他状态：hover:b-cyan-600

### w

w-4 代表**1rem**

w-1 0.25rem

w-px 使用像素

w-1/2 分数设置百分比

w-full 全宽

w-screen 视口宽度

min-w- 最小宽度

**媒体查询**md:min-w-0 在中等屏幕尺寸下应用

### h

与 w 同理

h-dvh 动态视口高度

h-lvh 最大视口高度

h-svh 最小

### size

与 w，h 同理，同时设置 wh

### text

xs，sm，base，lg，xl，2xl，4xl

**font-bold**

### rounded

=border 同 text

#### flex

flex 设置为弹性布局

flex-row flex-col 设置主轴

flex-wrap 换行

justify-center 沿主轴居中

justify-between/around/evenly

items-center...

gap 设置子元素艰巨

### m

设置 margin

mx-4 my-4 水平和垂直方向

mt mr mb ml 上下左右方向

-m-4 负值

### p

设置 padding

同 m

### transform

Transform 设置变换效果

**translate 平移** -x-4 -y-2

rotate-45 顺时针

scale-110 缩放 1.1 倍

skew-x-12 倾斜度数

### transition

transition 过渡

transition-colors 渐变颜色

**transition-transform 变换相关属性进行过渡**

duration-300 持续时间 300ms

ease-in/out/in-out 缓动函数

delay-200 延迟 ms

### animation

animation-ping 逐渐淡出

-bounce 弹跳动画

-spin 旋转动画

-pulse 呼吸动画

### 响应式

sm: md: lg:

### 状态类

hover: active: focus:

