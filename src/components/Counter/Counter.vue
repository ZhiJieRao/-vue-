<template>
  <div
    class="number-container d-flex justify-content-center align-items-center"
  >
    <!-- 减 1 的按钮 -->
    <button
      ref="sub"
      type="button"
      class="btn btn-light btn-sm"
      :id="'cid' + cid"
      @click="subCount"
    >
      -
    </button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ counterCount }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="addCount">
      +
    </button>
  </div>
</template>

<script>
export default {
  props: {
    //Count组件要实现数量的加减，除了传值给APP.vue 还要把对应的id传过去
    cid: {
      //商品id
      type: Number,
      require: true,
    },
    //接收到的商品数量
    counterCount: {
      require: true,
      type: Number,
    },
  },
  data() {
    return {};
  },
  methods: {
    //使商品数量加一 顺便把值传给APP.vue
    addCount() {
      //this.counterCount + 1 此时是没有直接修改props的值的 只不过是让他的值加1然后再传给APP.vue
      this.$bus.$emit("share", { value: this.counterCount + 1, id: this.cid });
      /*
        为什么要把移除disabled设置在这里呢？
        因为你一旦设置了disabled生效 ，那那个按钮就点击不了，即执行不了里面的代码的了
       */
      //取消掉button的disabled属性
      this.$refs.sub.removeAttribute("disabled");
    },
    // 使商品数量减一 顺便把值传给APP.vue
    subCount(e) {
      //当减少到1时禁止使用button
      if (this.counterCount - 1 === 0) {
        e.target.setAttribute("disabled", "true");
        return;
      }
      this.$bus.$emit("she", { value: this.counterCount - 1, id: this.cid });
    },
  },
};
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
