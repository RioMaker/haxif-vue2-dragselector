<template>
  <div
    class="selector-div"
    :style="`--width:${/^\d+$/.test(width)? width + 'px' : width}`"
    @mouseleave="itemMouseleave()"
    tabindex="0"
    @keyup.ctrl.90.exact="historyBack"
    @keyup.ctrl.shift.90.exact="historyRedo"
  >
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
        {{ item.text ? item.text : `${i + 1}` }}
      </div>
    </div>
    <div class="cancel" @mouseup="itemMouseleave()">
      <span class="cancel-info">取消本次操作</span>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    data: {
      type: Array,
      default: () => {
        return [
          {
            text: "测试数据1",
            ch: false,
          },
          {
            text: "测试数据2",
            ch: false,
          },
          {
            text: "测试数据3",
            ch: false,
          },
          {
            text: "测试数据4",
            ch: false,
          },
          {
            text: "测试数据5",
            ch: false,
          },
          {
            text: "测试数据6",
            ch: false,
          },
          {
            text: "测试数据7",
            ch: false,
          },
          {
            text: "测试数据8",
            ch: false,
          },
          {
            text: "测试数据9",
            ch: false,
          },
          {
            text: "测试数据10",
            ch: false,
          },
          {
            text: "测试数据11",
            ch: false,
          },
          {
            text: "测试数据12",
            ch: false,
          },
          {
            text: "测试数据13",
            ch: false,
          },
          {
            text: "测试数据14",
            ch: false,
          },
          {
            text: "测试数据15",
            ch: false,
          },
        ];
      },
    },
    width: {
      type: String,
      default: () => {
        return "500";
      },
    },
  },
  mounted() {
    for (let i = 0; i < this.data.length; i++) {
      this.data[i].ch = false;
    }
    this.select_list = this.data;
    this.select_cache = JSON.parse(JSON.stringify(this.select_list));
  },
  data() {
    return {
      select_is: "", //点击项
      select_start: "", //移动项
      select_start_cache_r: "", //临界暂存
      select_start_cache_l: "", //临界暂存
      select_cache: [], //显示|预览
      del: false, //删除预览效果
      select_list: [], //真实数据
      history: [], //历史操作
      history_indexs: 0, //历史索引
    };
  },
  methods: {
    itemMousedown(i) {
      // e.preventDefault();
      this.select_cache = JSON.parse(JSON.stringify(this.select_list));
      this.select_list[i]["ch"] = this.select_cache[i]["ch"] = !this
        .select_cache[i]["ch"];
      this.select_is = i;
      this.select_start_cache_r = this.select_start_cache_l = i;
      this.del = !this.select_cache[i]["ch"];
    },
    itemMousemove(i) {
      // e.preventDefault();
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
      if (typeof this.select_is == "number") {
        this.historySave();
      }
      this.select_list = JSON.parse(JSON.stringify(this.select_cache));
      this.select_is = "";
      this.select_start_cache_r = "";
      this.select_start_cache_l = "";
      this.select_start = "";
      this.del = false;
    },
    itemMouseleave() {
      if (typeof this.select_is == "number") {
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
    //克隆体自检测
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
    //历史保存函数
    historySave() {
      // console.log(this.history_indexs);
      let list = JSON.parse(JSON.stringify(this.select_list));
      let cache = JSON.parse(JSON.stringify(this.select_cache));
      let L = this.select_start_cache_l;
      let R = this.select_start_cache_r;
      let his_bit = [
        {
          index: this.select_is,
          ch: cache[this.select_is]["ch"],
        },
      ];
      for (let i = L; i <= R; i++) {
        if (list[i]["ch"] != cache[i]["ch"]) {
          his_bit.push({
            index: i,
            ch: cache[i]["ch"],
          });
        }
        // console.log(i,list[i]["ch"] != cache[i]["ch"],list[i]["ch"],cache[i]["ch"]);
      }
      this.history[this.history_indexs] = {
        index: this.history_indexs,
        operating: his_bit,
      };
      this.history_indexs++;
    },
    historyBack() {
      // console.log("c z");
      if (this.history_indexs - 1 < 0) {
        alert("历史记录已清空");
        return;
      }
      let his_data = this.history[--this.history_indexs];
      // console.log(his_data, this.history_indexs);
      if (his_data) {
        let back_data = his_data["operating"];
        for (let i = 0; i < back_data.length; i++) {
          const element = back_data[i];
          if (typeof element["index"] == "number") {
            // console.log(element["index"], element["ch"]);
            this.select_list[element["index"]]["ch"] = !element["ch"];
          }
        }
        this.select_cache = JSON.parse(JSON.stringify(this.select_list));
      } else {
        // console.log("历史记录已清空");
        alert("历史记录已清空");
      }
    },
    historyRedo() {
      // console.log("c sh z");
      let his_data = this.history[this.history_indexs];
      // console.log(his_data, this.history_indexs);
      if (his_data) {
        let back_data = his_data["operating"];
        for (let i = 0; i < back_data.length; i++) {
          const element = back_data[i];
          if (typeof element["index"] == "number") {
            // console.log(element["index"], element["ch"]);
            this.select_list[element["index"]]["ch"] = element["ch"];
          }
        }
        this.select_cache = JSON.parse(JSON.stringify(this.select_list));
        this.history_indexs++;
      } else {
        // console.log("历史记录已是最新");
        alert("历史记录已是最新");
      }
    },
  },
};
</script>

<style scoped>
.selector-div {
  width: var(--width);
  height: fit-content;
  display: flex;
  flex-direction: row;
  /* border: 1px solid #409ace; */
  /* text-align: center; */
  justify-content: center;
  border: none;
  outline: none;
  background: rgba(97, 97, 97, 0.157);
  transition: all 0.3s;
}
.selector-div:focus {
  background: rgb(255, 255, 255);
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
.cancel:hover .cancel-info {
  opacity: 1;
}
.selector {
  max-width: 100%;
  /* height: fit-content; */
  display: flex;
  flex-direction: row;
  flex-flow: wrap;
  /* overflow: auto; */
  /* background: rgb(255, 255, 255); */
}
.ch_item {
  min-width: 50px;
  line-height: 50px;
  padding: 1px 2px;
  text-align: center;
  border: 1px solid rgb(255, 255, 255);
  background: #d2eeff88;
  color: #030a428a;
  font-weight: 600;
  white-space: nowrap;
  transition: all 0.3s;
  user-select: none;
}
.ch_item:hover {
  background: #369bff9a;
  /* box-shadow: 0 0 100px #52ade9b7 inset; */
  color: rgb(255, 255, 255);
}
.ch_item:active {
  background: #369bffb7;
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