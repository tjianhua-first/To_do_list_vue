<template>
  <div>
    <div class="box">
      <div>
        <input type="text" v-model="inVal" />
        <button @click="addVal">添加</button>
      </div>
      <div>
        <input type="text" v-model="seVal" />
        <button @click="sear">搜索</button>
      </div>
      <ul class="bbd">
        <li index="0" class="bgc" v-show="searh">
          查询未完成
          <i
            class="iconfont icon-ashbin"
            @click="guang"
            style="float: right"
          ></i>
        </li>
        <li v-for="(item, i) in seaaa" :key="i" v-show="searh">
          {{ item.val }}
        </li>
        <li index="0" class="bgc">未任务</li>
        <li v-for="(item, i) in arr" :key="i + '2'">
          <span v-if="item.task == false">
            {{ item.val }}
            <i class="iconfont icon-more" @click="puts(item)"></i>
            <i class="iconfont icon-ashbin" @click="del(item)"></i>
            <i
              v-if="item.task === false"
              :class="item.task ? '' : 'iconfont icon-stop'"
              @click="sta(item)"
            ></i>
            <input
              type="text"
              class="put"
              v-focus="focusState"
              v-show="item.show"
              v-model="putVal"
              @blur="putd(item)"
              @keyup.enter="putd(item)"
            />
          </span>
        </li>
        <li index="0" class="bgc">已完成</li>
        <li v-for="(item, i) in arr" :key="i + '1'">
          <span v-if="item.task == true">
            {{ item.val }}
            <i class="iconfont icon-more" @click="puts(item)"></i>
            <i class="iconfont icon-ashbin" @click="del(item)"></i>
            <i
              v-if="item.task === false"
              :class="item.task ? '' : 'iconfont icon-stop'"
              @click="sta(item)"
            ></i>
            <input
              type="text"
              class="put"
              v-focus="focusState"
              v-show="item.show"
              v-model="putVal"
              @blur="putd(item)"
              @keyup.enter="putd(item)"
            />
          </span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 数据
      arr: [
        { val: "吃饭", task: true, show: false },
        { val: "睡觉", task: false, show: false },
      ],
      seaaa: [],
      // 更改查询内容
      inVal: "",
      seVal: "",
      putVal: "",
      // 显示与隐藏
      tf: false,
      searh: false,
      focusState:false
    };
  },
  directives: {
    focus: {
      // 指令的定义
      inserted: function (el ,{value}) {
        if(value){
          el.focus();
        }
      },
    },
  },
  methods: {
    // 添加
    addVal() {
      if (this.inVal.trim() == "") return;
      for (var i = 0; i < this.arr.length; i++) {
        if (this.arr[i].val == this.inVal) return;
      }
      this.arr.push({ val: this.inVal, task: false, show: false });
      this.inVal = "";
    },
    // 删除
    del(a) {
      for (var i = 0; i < this.arr.length; i++) {
        if (a == this.arr[i]) {
          this.arr.splice(i, 1);
        }
      }
    },
    // 修改
    puts(a) {
      for (var i = 0; i < this.arr.length; i++) {
        if (this.arr[i].val == a.val) {
          this.arr[i].show = true;
          this.focusState = true;
        }
      }
    },
    putd(a) {
      for (var i = 0; i < this.arr.length; i++) {
        if (this.arr[i] == a) {
          this.arr[i].show = false;
          if (this.putVal.trim() != "") {
            this.arr[i].val = this.putVal;
            this.focusState = true;
          }
        }
      }
      this.putVal = "";
      // this.fn()
    },

    // 状态
    sta(a) {
      for (var i = 0; i < this.arr.length; i++) {
        if (this.arr[i] == a) {
          this.arr[i].task = true;
        }
      }
      // this.fn()
    },
    // 查询
    sear() {
      for (var i = 0; i < this.arr.length; i++) {
        if (
          this.arr[i].val.indexOf(this.seVal) != -1 &&
          this.arr[i].task == false
        ) {
          this.searh = true;
          this.seaaa.push(this.arr[i]);
        }
      }
    },
    guang() {
      this.searh = false;
      this.seaaa = [];
      this.seVal = "";
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
}

ul {
  list-style: none;
}

.box {
  position: absolute;
  left: 50%;
  top: 250px;
  transform: translateX(-50%);
}

input {
  border: 1px solid black;
  width: 250px;
  height: 40px;
}

button {
  width: 60px;
  height: 42px;
}

#tab li {
  float: left;
  width: 80px;
  height: 41px;
  line-height: 40px;
}

.bgc {
  background-color: #ccc;
}

i {
  float: right;
  margin-left: 20px;
}
.bbd {
  margin-top: 10px;
}
.bbd li {
  position: relative;
}
.put {
  position: absolute;
  left: 0px;
  height: 18px;
  width: 210px;
}
</style>
