<template>
  <div class="header-container">{{ title }}</div>
</template>

<script>
export default {
  props: {
    //声明title属性,允许使用者使用自定义标题
    title: {
      default: "1", //如果用户在父组件的<Header>中没有添加title属性,则默认值为空
      type: String, //title为String类型
    },
  },
};
</script>

<style lang="less" scoped>
.header-container {
  font-size: 12px;
  height: 45px;
  width: 100%;
  background-color: #1d7bff;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff;
  position: fixed;
  top: 0;
  z-index: 999;
}
</style>
