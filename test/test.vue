<template>
  <div class="main">
    <div class="test-selector">
      <selector width="500" :data="list"></selector>
    </div>
  </div>
</template>

<script>
import Selector from "../src/Selector";
export default {
  components: { Selector },
  mounted() {
    for (let i = 0; i < 500; i++) {
      this.list[i] = {};
    }
  },
  data() {
    return {
      list: [{}],
    };
  },
};
</script>

<style scoped>
.main {
  width: 100%;
  height: 100vh;
  display:flex;
  justify-content: center;
}
</style>