<template>
  <div id="app">
    <router-view />
  </div>
</template>

<style lang="less">
* {
  margin: 0;
  padding: 0;
  list-style: none;
  color: #000;
  font-family: "PingFang SC", "PingFang-SC-Medium", "微软雅黑", "Avenir",
    Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-decoration: none;
  font-style: normal;
}
html,
body {
  width: 100%;
  height: 100%;
  overflow-x: auto;
  overflow-y: auto;
  &::-webkit-scrollbar {
    display: block;
    width: 15px; /*滚动条宽度*/
    height: 8px;
  }
  /*定义滚动条轨道 内阴影+圆角*/
  &::-webkit-scrollbar-track {
    border-radius: 10px;
  }
  /*定义滑块 内阴影+圆角*/
  &::-webkit-scrollbar-thumb {
    border-radius: 10px;
    background-color: #6c6c6c;
  }
}
#app {
  height: 100%;
  min-width: 1200px;
  overflow: hidden;
  position: relative;
  &::-webkit-scrollbar {
    display: block;
    width: 10px; /*滚动条宽度*/
    height: 8px;
  }
  /*定义滚动条轨道 内阴影+圆角*/
  &::-webkit-scrollbar-track {
    border-radius: 10px;
  }
  /*定义滑块 内阴影+圆角*/
  &::-webkit-scrollbar-thumb {
    border-radius: 10px;
    background-color: #6c6c6c;
  }
}
</style>
