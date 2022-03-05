<template>
  <div class="footer-container">
    <!-- 左侧的全选 -->
    <div class="custom-control custom-checkbox">
      <input
        type="checkbox"
        class="custom-control-input"
        id="cbFull"
        :checked="footState"
        @change="fullChange"
      />
      <label class="custom-control-label" for="cbFull">全选</label>
    </div>

    <!-- 中间的合计 -->
    <div>
      <span>合计：</span>
      <!-- Number.toFixed() 参数传递的是保留多少位小数 返回值是字符串-->
      <span class="total-price">￥{{ allPrice.toFixed(2) }}</span>
    </div>

    <!-- 结算按钮 -->
    <button type="button" class="btn btn-primary btn-settle">
      结算（{{ aCheckNums }}）
    </button>
  </div>
</template>

<script>
export default {
  // created() {
  //   //接收来自
  //   // this.$bus.$on("share-footer", (val) => (this.allnums = val));
  // },
  props: {
    //最底下全选的状态
    footState: {
      default: true,
      type: Boolean,
    },
    //总价格
    allPrice: {
      default: 0,
      type: Number,
    },
    //全选数量
    aCheckNums: {
      default: 0,
      type: Number,
    },
  },
  data() {
    return {};
  },
  methods: {
    //监听到全选的状态
    fullChange(e) {
      // console.log(e.target);  获取到的是触发事件的对象 即全选按钮
      //最新的状态不是通过this.footState获取 而是要通过事件去获取 因为footState是当商品列表全选时候才会改变值的
      this.$emit("all-Checkbox", e.target.checked);
    },
  },
};
</script>

<style lang="less" scoped>
.footer-container {
  font-size: 12px;
  height: 50px;
  width: 100%;
  border-top: 1px solid #efefef;
  position: fixed;
  bottom: 0;
  background-color: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
}

.custom-checkbox {
  display: flex;
  align-items: center;
}

#cbFull {
  margin-right: 5px;
}

.btn-settle {
  height: 80%;
  min-width: 110px;
  border-radius: 25px;
  font-size: 12px;
}

.total-price {
  font-weight: bold;
  font-size: 14px;
  color: red;
}
</style>
