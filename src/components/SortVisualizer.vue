<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-btn outlined color="primary" @click="newArray()" class="mr-2"
          >New array</v-btn
        >
        <v-btn color="primary" @click="sort()">Sort</v-btn>
      </v-col>
    </v-row>
    <v-row class="text-center">
      <v-col cols="12">
        <canvas class="board" ref="canvas" :width="width" :height="height" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "SortVisualizer",

  data: () => ({
    ctx: null,
    width: 800,
    height: 400,
    eltCount: 100,
    items: null,
    compare: {
      first: null,
      second: null,
      swap: false
    },
    sorted: []
  }),
  computed: {
    barWidth() {
      return this.width / this.eltCount;
    }
  },
  mounted() {
    this.ctx = this.$refs.canvas.getContext("2d");
    this.newArray();
    this.draw();
  },
  methods: {
    newArray() {
      let array = [];
      for (var i = 0; i < this.eltCount; i++) {
        array.push(Math.floor(Math.random() * this.height + 1));
      }
      this.items = array;
      this.sorted = [];
      this.draw();
    },
    draw() {
      // eslint-disable-next-line no-unused-vars
      const { sorted, ctx, width, height, eltCount, items, compare } = this;
      const barWidth = this.barWidth;
      const barColor = "#b2ebf2";
      const swapColor = "#ffb74d";
      const noSwapColor = "#c5e1a5";
      const sortedColor = "#4caf50";
      ctx.clearRect(0, 0, width, height);
      ctx.fillStyle = barColor;
      for (let x = 0; x < eltCount; x++) {
        if (x === compare.first || x === compare.second) {
          if (compare.swap) {
            ctx.fillStyle = swapColor;
          } else {
            ctx.fillStyle = noSwapColor;
          }
        } else if (sorted.indexOf(x) >= 0) {
          ctx.fillStyle = sortedColor;
        }
        ctx.fillRect(x * barWidth, height, barWidth, -items[x]);
        ctx.fillStyle = barColor;
      }
    },
    async sort() {
      // eslint-disable-next-line for-direction
      for (var last = this.items.length; last >= 0; last--) {
        for (var index = 1; index < last; index++) {
          this.compare.first = index;
          this.compare.second = index - 1;
          if (this.items[index - 1] > this.items[index]) {
            this.compare.swap = true;
            const swap = this.items[index - 1];
            this.items[index - 1] = this.items[index];
            this.items[index] = swap;
          } else {
            this.compare.swap = false;
          }
          await this.sleep(1);
          this.draw();
          this.compare = {
            first: null,
            second: null,
            swap: false
          };
        }
        this.sorted.push(last - 1);
        this.draw();
      }
    },
    async sleep(ms) {
      return new Promise(resolve => setTimeout(resolve, ms));
    }
  }
};
</script>
<style scoped>
.board {
  margin: auto;
  border: solid 1px black;
}
.bar {
  border: solid 1px white;
  background: rgb(97, 97, 241);
}
</style>
