<template>
  <h1>Vue パレット</h1>
  <div class="app">
    <div
      class="palette"
      v-on:mousemove="changeColor"
      v-bind:style="paletteStyle"
    >
      <div>
        <div style="color: red">
          RED
          <input type="range" min="0" max="255" step="1" v-model="redSlider" />
        </div>
        <div style="color: green">
          GREEN
          <input
            type="range"
            min="0"
            max="255"
            step="1"
            v-model="greenSlider"
          />
        </div>
        <div style="color: blue">
          BLUE
          <input type="range" min="0" max="255" step="1" v-model="blueSlider" />
        </div>
        <div style="color: white">
          Alpha
          <input
            type="range"
            min="0.00"
            max="1.00"
            step="0.05"
            v-model="alphaSlider"
          />
        </div>
      </div>
    </div>
    <p>
      rgba( {{ redSlider }}, {{ greenSlider }}, {{ blueSlider }},
      {{ alphaSlider }} )
    </p>

    <div>
      <button v-on:click="pickColor()">色をパレットに追加する</button>
    </div>
    <div class="colors-container">
      <div
        class="mini-palette"
        v-for="color in colors"
        :key="color.red"
        v-bind:style="{
          backgroundColor: `rgba(${color.red}, ${color.green}, ${color.blue}, ${color.alpha})`,
        }"
        v-on:click="showColor(color)"
      ></div>
    </div>
  </div>
</template>

<style>
.app {
  display: flex;
  width: 100%;
  flex-direction: column;
  align-items: center;
}
.palette {
  width: 255px;
  height: 255px;
}
.mini-palette {
  min-width: 60px;
  height: 60px;
}
.colors-container {
  display: flex;
  flex-wrap: wrap;
  width: 300px;
  padding-top: 8px;
}
</style>

<script>
export default {
  data() {
    return {
      red: 0,
      green: 0,
      blue: 0,
      alpha: 0.5,
      colors: [
        // { red: 0, green: 0 }
      ],
      redSlider: 0,
      greenSlider: 0,
      blueSlider: 0,
      alphaSlider: 0.5,
    }
  },
  methods: {
    // マウスの位置に応じて色を変える
    /* 引数eはイベントmousemove */
    changeColor() {
      this.red = this.redSlider
      this.green = this.greenSlider
      this.blue = this.blueSlider
      this.alpha = this.alphaSlider
    },
    // 色を選んでミニパレットに追加する
    pickColor() {
      const newColor = {
        red: this.redSlider, //変更前red
        green: this.greenSlider, //変更前green
        blue: this.blueSlider, //追加
        alpha: this.alphaSlider,
      }
      this.colors.push(newColor)
    },
    // パレットに指定した色を表示する
    showColor(color) {
      this.red = color.red
      this.green = color.green
      this.blue = color.blue
      this.alpha = color.alpha
    },
  },
  computed: {
    paletteStyle() {
      return {
        backgroundColor: `rgba(${this.red}, ${this.green}, ${this.blue}, ${this.alpha})`, //変更前blue=200
      }
    },
  },
}
</script>
