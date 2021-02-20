# 拖动选择器组件
### 安装
```bash
npm install --save haxif-vue2-dragselector
```

### 引用

>本版本为测试版本暂不支持添加数据请等待后续版本
>注意：引用本组件需要设定最大宽度和高度方式，如需滚动调请设置scroll-y:auto属性

#### 参考引用
```html
<template>
  <div class="main">
    <xDragSelector></xDragSelector>
  </div>
</template>

<script>
import xDragSelector from "haxif-vue2-dragselector";
export default {
  name: "DragSelector",
  components: { xDragSelector },
  data() {
    return {};
  },
};
</script>

<style scoped>
.main {
  max-width: 100%;
  height: fit-content;
}
</style>
```