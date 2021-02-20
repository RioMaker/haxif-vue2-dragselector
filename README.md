# 拖动选择器组件
### 安装
```bash
npm install --save haxif-vue2-dragselector
```

### 引用

>width:数字自动转义px基准
>data:传入参数，要求为对象数组（数组关键词 "ch" ，请不要使用ch作为变量存储数据否则会导致数据丢失 ）

#### 参考引用
```html
<template>
  <div class="main">
    <xDragSelector width="500" :data="list"></xDragSelector>
  </div>
</template>

<script>
import xDragSelector from "haxif-vue2-dragselector";
export default {
  name: "DragSelector",
  components: { xDragSelector },
  data() {
    return {
      list: [
        {
          text:"倘若不输入则默认显示序号",
          ch:"占用，请勿使用改变量"
        },
        {}
      ],
    };
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

>2021.02.20 - 新增历史记录控制（ctrl + z撤销，ctrl+shift+z 重做）
>注意：在历史节点上进行操作后会覆盖原有数据请注意