---
title: Flex布局
date: 2017-06-05 17:07:50
tags: css3
---

## Flex布局是什么？

Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。

任何一个容器都可以指定为 Flex 布局。

<!--more-->

### 定义容器的display属性

```css
.box{
  	display: -webkit-flex;
  	dispaly: flex;
}
/*行内样式*/
.box{
  	display: -webkit-inline-flex;
  	display: inline-flex;
}
```

### 容器样式

**主轴方向（flex-direction）**

| 属性值            | 属性的含义   |
| -------------- | ------- |
| row            | 左到右（默认） |
| row-reverse    | 右到左     |
| column         | 上到下     |
| column-reverse | 下到上     |

**换行（flex-wrap）**

| 属性值          | 属性的含义     |
| ------------ | --------- |
| nowrap       | 不换行（默认）   |
| wrap         | 换行        |
| wrap-reverse | 换行并第一行在下方 |

**主轴方向和换行简写**

```css
flew-flow:<flex-direction>||<flex-wrap>
```

**主轴对齐方式（justify-content）**

| 属性值           | 属性的含义   |
| ------------- | ------- |
| flex-start    | 左对齐（默认） |
| flex-end      | 右对齐     |
| center        | 居中对齐    |
| space-between | 两端对齐    |
| space-around  | 平均分布    |

**交叉轴对齐方式（align-items）**

| 属性值        | 属性的含义                            |
| ---------- | -------------------------------- |
| flex-start | 顶部对齐                             |
| flex-end   | 底部对齐                             |
| center     | 居中对齐                             |
| baseline   | 文本基线对齐                           |
| stretch    | 如果项目未设置高度或设为auto，将占满整个容器的高度。（默认） |

### 子元素属性

**排序（order：<number>）**：排序，数值越小，越排前，默认为0

**放大（flex-grow: <number>）**：放大：默认0（即如果有剩余空间也不放大，值为1则放大，2是1的双倍大小，以此类推）

**缩小（flex-shrink:<number>）**：缩小：如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。负值对该属性无效。

**固定大小（flex-basis:<length> | auto）**:固定大小：默认为0，可以设置px值，也可以设置百分比大小

**flex:none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]**：flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。

**单独对齐方式（align-self）**



| 属性值        | 含义      |
| ---------- | ------- |
| auto       | 自动（默认）  |
| flex-start | 顶部对齐    |
| flex-end   | 底部对齐    |
| center     | 居中对齐    |
| baseline   | 文本基线对齐  |
| stretch    | 上下对齐并铺满 |