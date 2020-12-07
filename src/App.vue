<template>
  <div class="box">
    <div :style="{ height: topHeight + 'px' }" class="top"></div>
    <div
      class="item"
      :style="{ height: itemHeight + 'px' }"
      :key="item"
      v-for="item in list"
    >
      {{ item }}
    </div>
    <div :style="{ height: bottomHeight + 'px' }" class="bottom"></div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      originList: new Array(100000).fill(0).map((it, i) => i + 1),
      list: [],
      maxItems: 0,
      topHeight: 0,
      bottomHeight: 0,
      itemHeight: 60
    };
  },
  mounted() {
    this.maxItems = Math.ceil(
      document.documentElement.clientHeight / this.itemHeight
    );
    if (this.originList.length <= this.maxItems) {
      this.list = this.originList;
      return;
    }
    window.addEventListener("scroll", this.calculate);
    window.addEventListener("resize", this.resize);
    this.calculate();
  },
  methods: {
    resize() {
      this.maxItems = Math.ceil(
        document.documentElement.clientHeight / this.itemHeight
      );
      this.calculate();
    },
    calculate() {
      const scrollTop = document.documentElement.scrollTop;
      const len = this.originList.length;
      let start = 0;
      let end = 0;

      start = Math.floor((scrollTop - this.$el.offsetTop) / this.itemHeight);
      if (start + this.maxItems > len) {
        start = len - this.maxItems;
      }
      end = start + this.maxItems;

      this.list = this.originList.slice(start, end);

      this.topHeight = start * this.itemHeight;

      this.bottomHeight =
        this.originList.length * this.itemHeight -
        (this.maxItems * this.itemHeight + this.topHeight);
      console.log(
        `start`,
        start,
        `end`,
        end,
        "topHeight",
        this.topHeight,
        "bottomHeight",
        this.bottomHeight
      );
    }
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.calculate);
    window.removeEventListener("resize", this.resize);
  }
};
</script>
<style>
* {
  padding: 0;
  margin: 0;
}
.item {
  display: flex;
  align-items: center;
  justify-content: center;
  box-sizing: border-box;
  border: 1px solid #ccc;
  color: blueviolet;
}
</style>
