<template>
  <div class="my-header">
    <div></div>
    <div class="tips">Just press your keyboard!</div>
    <a href="https://github.com/SpacesoulsL/JustPressYourKeyboard" target="_blank" class="share">
      <span style="margin-right: 10px">STAR IT</span>
      <svg
        t="1686754273379"
        class="icon"
        viewBox="0 0 1024 1024"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        p-id="2688"
        width="48"
        height="48"
      >
        <path
          d="M64 512c0 195.2 124.8 361.6 300.8 422.4 22.4 6.4 19.2-9.6 19.2-22.4v-76.8c-134.4 16-140.8-73.6-150.4-89.6-19.2-32-60.8-38.4-48-54.4 32-16 64 3.2 99.2 57.6 25.6 38.4 76.8 32 105.6 25.6 6.4-22.4 19.2-44.8 35.2-60.8-144-22.4-201.6-108.8-201.6-211.2 0-48 16-96 48-131.2-22.4-60.8 0-115.2 3.2-121.6 57.6-6.4 118.4 41.6 124.8 44.8 32-9.6 70.4-12.8 112-12.8 41.6 0 80 6.4 112 12.8 12.8-9.6 67.2-48 121.6-44.8 3.2 6.4 25.6 57.6 6.4 118.4 32 38.4 48 83.2 48 131.2 0 102.4-57.6 188.8-201.6 214.4 22.4 22.4 38.4 54.4 38.4 92.8v112c0 9.6 0 19.2 16 19.2C832 876.8 960 710.4 960 512c0-246.4-201.6-448-448-448S64 265.6 64 512z"
          fill="#ffffff"
          p-id="2689"
        ></path>
      </svg>
    </a>
  </div>
</template>

<script setup></script>

<style lang="less" scoped>
.my-header {
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 40px;
  background-color: rgb(34, 34, 34);
  color: #ffffff;
  .logo {
    width: 30px;
    height: 30px;
  }

  .tips {
    font-size: 18px;
    font-weight: bold;
    margin-left: 100px;
  }
  .share {
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    font-weight: bold;
    font-size: 14px;
    text-decoration: none;
    color: #fff;
    transition: all 0.2s ease-in-out;
    &:hover {
      text-shadow: 0 0 15px yellow;
      text-shadow: 0 0 18px yellow;
    }
  }
}
</style>
