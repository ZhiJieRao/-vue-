<template>
  <div class="goods-container">
    <!-- 左侧图片 -->
    <div class="thumb">
      <div class="custom-control custom-checkbox">
        <!-- 复选框 -->
        <!-- 类型为checkbook的本身就有change事件 只要复选框的状态发生了变化就会自动触发change事件 -->
        <input
          class="custom-control-input"
          type="checkbox"
          @change="stateChange"
          :id="'cb' + id"
          :checked="status"
        />
        <label class="custom-control-label" :for="'cb' + id">
          <!-- 商品的缩略图 -->
          <img
            :src="picture"
            alt="可能是API失效了"
          /><!-- 注意！！！插值表达式不能用在属性里面，应该用v-bind去绑定属性并进行传值-->
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="goods-info">
      <!-- 商品标题 -->
      <h6 class="goods-title">{{ name }}</h6>
      <div class="goods-info-bottom">
        <!-- 商品价格 -->
        <span class="goods-price">￥{{ price }}</span>
        <!-- 商品的数量 -->
        <Counter :counterCount="count" :cid="id"></Counter>
      </div>
    </div>
  </div>
</template>

<script>
import Counter from "@/components/Counter/Counter.vue";

export default {
  props: {
    //要根据id来获得勾选状态 传递给父组件 通知父组件根据id修改对应商品的勾选状态
    //商品id
    id: {
      type: Number,
      require: true, //id为必填项 不传过来会报错
    },
    //商品名字
    name: {
      default: "",
      type: String,
    },
    //商品价格
    price: {
      default: 0,
      type: Number,
    },
    //商品图片
    picture: {
      default: "",
      type: String,
    },
    //商品状态
    status: {
      default: true,
      type: Boolean,
    },
    //商品的数量
    count: {
      type: Number,
      require: true,
    },
  },

  methods: {
    //只要复选框状态变化就会触发这个函数
    stateChange(e) {
      const newState = e.target.checked; //复选框的状态
      //自定义事件state-change
      this.$emit("state-change", { value: newState, id: this.id });
    },
    // getCount(val) {
    //   return this.price * val;
    // },
  },
  components: {
    Counter,
  },
};
</script>

<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  padding: 10px;
  display: flex;
  .thumb {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      margin: 0 10px;
    }
  }

  .goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
      font-weight: bold;
      font-size: 12px;
    }
    .goods-info-bottom {
      display: flex;
      justify-content: space-between;
      .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
      }
    }
  }
}
</style>
