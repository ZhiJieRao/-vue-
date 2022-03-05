<template>
  <div class="app-container">
    <!-- Header头部区域 -->
    <!--如果在title前面不加:则是字符串,如果加了:里面的值代表是表达式 -->
    <Header :title="headersValue"></Header>
    <!-- 中间区域 -->
    <!-- 
      做大项目的时候，不要直接传一个对象去给子组件，应该像下面这样把对象拆解开
      黑马vue视频 P134
      https://www.bilibili.com/video/BV1zq4y1p7ga?p=134&spm_id_from=pageDriver
    -->
    <!--要循环谁 v-for就加给谁 -->
    <!-- 直接那key传递给props会报错的 因为key是自定义属性 是给虚拟内存的 -->
    <Goods
      v-for="item in list"
      :key="item.id"
      :id="item.id"
      :name="item.goods_name"
      :price="item.goods_price"
      :picture="item.goods_img"
      :status="item.goods_state"
      :count="item.goods_count"
      @state-change="getNewState"
    ></Goods>
    <!-- 底部区域 -->
    <Footer
      :footState="fullState"
      :allPrice="amount"
      :aCheckNums="allChecknums"
      @all-Checkbox="getNewsState"
    ></Footer>
  </div>
</template>

<script>
import axios from "axios"; //导入axios包
import Header from "@/components/Header/Header.vue";
import Goods from "@/components/Goods/Goods.vue";
import Footer from "@/components/Footer/Footer.vue";
export default {
  created() {
    //为什么要在created里发ajax请求?  因为最早能拿到数据的时候就是在created(),所以在实际开发中 往往都在created发ajax请求
    this.initShopList();
    //接收Count.vue传过来的值 目的是让goods_count加1
    this.$bus.$on("share", (val) => {
      this.list.some((item) => {
        if (item.id === val.id) {
          item.goods_count = val.value; //把按钮的数量赋值给goods_count
          return true; //return true 表示整个if语句返回的是true 表明找成功了 some就会停止寻找，如果返回的false则会继续寻找
        }
      });
    });
    //接收Count.vue传过来的值 目的是让goods_count减1
    this.$bus.$on("she", (val) => {
      this.list.some((item) => {
        if (item.id === val.id) {
          item.goods_count = val.value; //把按钮的数量赋值给goods_count
          return true;
        }
      });
    });
  },
  data() {
    return {
      headersValue: "购物车案例",
      list: [], //用来存axios.get回来的数据
      // allSelect: this.fullState, //全选属性  计算属性只能用在插值表达式或者方法中使用 不能用在data中
    };
  },
  methods: {
    //根据api接口获得数据
    async initShopList() {
      //data是这个大对象的一个属性，res是对这个data的重命名
      const { data: res } = await axios.get("https://www.escook.cn/api/cart"); //为什么要用async/await???
      //只要请求回来的数据，如果渲染页面要用到，必须转存到data里面
      if (res.status === 200) {
        //status===200表示获取信息成功
        this.list = res.list;
      }
    },
    //子向父传值 Goods把每个商品的状态传递给APP
    getNewState(val) {
      this.list.some((item) => {
        if (item.id === val.id) {
          item.goods_state = val.value;
          return true; //返回一个true表明已经找到了 some就会停止寻找
        }
      });
    },
    //子向父传值 Footer把每个商品的id传递给APP
    getNewsState(val) {
      this.list.forEach((item) => {
        item.goods_state = val;
      });
    },
  },
  //计算属性定义的时候是方法 使用的时候是属性
  computed: {
    //动态的计算全选状态是 true 还是 false
    fullState() {
      return this.list.every((item) => item.goods_state === true); //可以省略为item.goods_state,因为当箭头函数只有一行的时候就代表直接返回该行的结果
    },
    //计算已勾选的商品价格
    amount() {
      //1.用filter计算已勾选的商品
      //2.用reduce累加商品总和
      return this.list
        .filter((item) => item.goods_state === true)
        .reduce(
          (total, item) => (total += item.goods_price * item.goods_count), //价格乘于数量
          0
        );
    },
    //获得已勾选的商品数量的个数
    allChecknums() {
      return this.list.filter((item) => item.goods_state).length;
    },
  },
  components: {
    Header,
    Goods,
    Footer,
  },
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
