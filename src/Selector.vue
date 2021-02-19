<template>
  <div class="main" @mouseleave="itemMouseleave()">
    <div class="selector" @mouseup="itemMouseup()">
      <div
        v-for="(item, i) in select_cache"
        :class="`ch_item ${del ? 'ch_item_del' : ''} ${
          (item.ch = nbility(i))
            ? 'choose'
            : del && i <= select_start_cache_r && i >= select_start_cache_l
            ? 'del'
            : ''
        }`"
        :key="i"
        @mousedown="itemMousedown(i, $event)"
        @mouseover="itemMousemove(i, $event)"
      >
        {{ i }}
      </div>
    </div>
    <div class="cancel" @mouseup="itemMouseleave()">
      <span class="cancel-info">取消本次操作</span>
    </div>
  </div>
</template>
<script>
export default {
  methods: {
    itemMousedown(i, e) {
      e.preventDefault();
      this.select_cache = JSON.parse(JSON.stringify(this.select_list));
      this.select_list[i]["ch"] = this.select_cache[i]["ch"] = !this
        .select_cache[i]["ch"];
      this.select_is = i;
      this.select_start_cache_r = this.select_start_cache_l = i;
      this.del = !this.select_cache[i]["ch"];
    },
    itemMousemove(i, e) {
      e.preventDefault();
      this.select_cache = JSON.parse(JSON.stringify(this.select_list));
      // if (
      //   (i > this.select_is &&
      //     i - this.select_is > this.select_start - this.select_is) ||
      //   (i < this.select_is &&
      //     i - this.select_is < this.select_start - this.select_is)
      // ) {
      //   this.select_start_cache_r =
      //     i > this.select_is ? i : this.select_start_cache_r;
      //   this.select_start_cache_l =
      //     i < this.select_is ? i : this.select_start_cache_l;
      // } else {
      //   this.select_start_cache_l =
      //     i <= this.select_is ? this.select_start_cache_l : this.select_is;
      //   this.select_start_cache_r =
      //     i >= this.select_is ? this.select_start_cache_r : this.select_is;
      // }
      if (i > this.select_is) {
        this.select_start_cache_r = i;
        this.select_start_cache_l = this.select_is;
      } else {
        this.select_start_cache_r = this.select_is;
        this.select_start_cache_l = i;
      }
      this.select_start = i;
    },
    itemMouseup() {
      this.select_list = JSON.parse(JSON.stringify(this.select_cache));
      this.select_is = "";
      this.select_start_cache_r = "";
      this.select_start_cache_l = "";
      this.select_start = "";
      this.del = false;
    },
    itemMouseleave() {
      if(typeof this.select_is=='number'){
        this.select_list[this.select_is]["ch"] = !this.select_list[
          this.select_is
        ]["ch"];
      }
      this.select_cache = JSON.parse(JSON.stringify(this.select_list));
      this.select_is = "";
      this.select_start_cache_r = "";
      this.select_start_cache_l = "";
      this.select_start = "";
      this.del = false;
    },
    nbility(i) {
      if (
        typeof this.select_is == "number" &&
        typeof this.select_start == "number" &&
        typeof this.select_start_cache_r == "number" &&
        typeof this.select_start_cache_l == "number"
      ) {
        if (
          (i > this.select_is && i <= this.select_start) ||
          (i < this.select_is && i >= this.select_start)
        ) {
          this.select_cache[i]["ch"] = this.select_cache[this.select_is]["ch"];
          // this.del = false;
        } else if (
          (this.select_start > this.select_is &&
            i > this.select_start &&
            i <= this.select_start_cache_r) ||
          (this.select_start < this.select_is &&
            i < this.select_start &&
            i >= this.select_start_cache_l) ||
          (this.select_start <= this.select_is &&
            i > this.select_is &&
            i <= this.select_start_cache_r) ||
          (this.select_start >= this.select_is &&
            i < this.select_is &&
            i >= this.select_start_cache_l)
        ) {
          this.select_cache[i]["ch"] = false; //!this.select_cache[this.select_is]["ch"]
        }
      }
      return this.select_cache[i]["ch"] == true;
    },
  },
  mounted() {
    for (let i = 0; i < 500; i++) {
      this.select_list.push({ ch: false });
    }

    this.select_cache = JSON.parse(JSON.stringify(this.select_list));
  },
  data() {
    return {
      select_is: "",
      select_start: "",
      select_start_cache_r: "",
      select_start_cache_l: "",
      select_cache: [],
      del: false,
      select_list: [
        { ch: false },
        { ch: false },
        { ch: false },
        { ch: false },
        { ch: false },
        { ch: false },
        { ch: false },
        { ch: false },
        { ch: false },
        { ch: false },
        { ch: false },
        { ch: false },
        { ch: false },
        { ch: false },
        { ch: false },
      ],
    };
  },
};
</script>

<style scoped>
.main {
  width: 100%;
  display: flex;
  flex-direction: row;
  /* border: 1px solid #409ace; */
  /* text-align: center; */
  justify-content: center;
}
.cancel {
  min-width: 40px;
  background: #ff3636e1;
  transition: all 0.3s;
  position: relative;
}
.cancel:hover {
  background: #ff3636be;
}
.cancel-info {
  position: fixed;
  top: 20px;
  right: 0;
  /* transform: translateY(-50%); */
  transition: all 0.3s;
  opacity: 0;
  width: fit-content;
  padding: 5px 7px;
  white-space: nowrap;
  background: rgb(192, 100, 100);
  text-align: center;
  color: white;
  line-height: 30px;
  font-size: large;
  border-radius: 7px;
}
.cancel:hover .cancel-info{
  opacity: 1;
}
.selector {
  max-width: 100%;
  height: fit-content;
  display: flex;
  flex-direction: row;
  flex-flow: wrap;
  background: rgb(255, 255, 255);
}
.ch_item {
  width: 50px;
  line-height: 50px;
  text-align: center;
  border: 1px solid rgb(255, 255, 255);
  background: #d2eeff88;
  color: #030a428a;
  font-weight: 600;
  transition: all 0.3s;
  user-select: none;
}
.ch_item:hover {
  background: #369bffc0;
  /* box-shadow: 0 0 100px #52ade9b7 inset; */
  color: rgb(255, 255, 255);
}
.ch_item_del:hover {
  background: #ff363673;
  /* box-shadow: 0 0 100px #52ade9b7 inset; */
  color: rgb(255, 255, 255);
}
.del {
  background: #ff3636c0;
  /* box-shadow: 0 0 100px #52ade9b7 inset; */
  color: rgb(255, 255, 255);
}
.choose {
  background: rgb(82, 173, 233);
}
</style>