<template>
  <div class="hello">
    <h1>Shape-Scaper</h1>
    <section class="settings">
      <h2>Settings:</h2>
      <button @click="this.randomMode = !this.randomMode">{{ randomBtnText }}</button>
      <section v-show="!randomMode" class="non-random-config">
        <div class="setting circle-count">
          <label for="circle-count-input"># of Circles:</label>
          <input v-model="circCount" id="circle-count-input" type="number" max="100" />
        </div>
        <div class="setting tri-count">
          <label for="tri-count-input"># of Triangles:</label>
          <input v-model="triCount" id="tri-count-input" type="number" max="100" />
        </div>
        <div class="setting rect-count">
          <label for="rect-count-input"># of Rectangles:</label>
          <input v-model="rectCount" id="rext-count-input" type="number" max="100" />
        </div>
      </section>
      <button @click="handleShapeButtonPress">Make the Shapes</button>
    </section>
    <Shaper
      :circCount="circCount"
      :rectCount="rectCount"
      :triCount="triCount"
      :runShaper="runShaper"
      :randomMode="randomMode"
    />
  </div>
</template>

<script>
import Shaper from './Shaper.vue';

export default {
  name: 'AppStage',
  components: {
    Shaper
  },
  props: {
    msg: String
  },
  data() {
    return {
      circCount: 0,
      triCount: 0,
      rectCount: 0,
      runShaper: false,
      randomMode: true,
    }
  },
  watch: {
    createShapes() {
      if (this.createShapes === true) {
        this.shapeUp();

        this.createShapes = false;
      }
    },
    runShaper() {
      if (this.runShaper) {
        setTimeout(() => this.runShaper = false, 500);
      }
    }
  },
  mounted() {
    this.initiateVals();
  },
  computed: {
    randomBtnText() {
      return this.randomMode ? 'Control Shapes' : 'Randomize Shapes';
    }
  },
  methods: {
    initiateVals() {
      this.circCount = this.generateRandom(3, 12);
      this.triCount = this.generateRandom(3, 12);
      this.rectCount = this.generateRandom(3, 12);
    },
    handleShapeButtonPress() {
      this.runShaper = true;
    },
    generateRandom(min = 0, max = 100) {
      // find diff
      let difference = max - min;

      // generate random number 
      let rand = Math.random();

      // multiply with difference 
      rand = Math.floor( rand * difference);

      // add with min value 
      rand = rand + min;

      return rand;
    },
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
