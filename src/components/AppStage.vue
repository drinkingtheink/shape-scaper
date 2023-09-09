<template>
  <div class="hello">
    <h1>Shape-Scaper</h1>
    <section class="settings">
      <h2>Settings:</h2>
      <div class="setting circle-count">
        <label for="circle-count-input"># of Circles:</label>
        <input value="10" id="circle-count-input" type="number" max="100" />
      </div>
      <div class="setting tri-count">
        <label for="tri-count-input"># of Triangles:</label>
        <input value="10" id="tri-count-input" type="number" max="100" />
      </div>
      <div class="setting rect-count">
        <label for="rect-count-input"># of Rectangles:</label>
        <input value="10" id="rext-count-input" type="number" max="100" />
      </div>
    </section>
    <Shaper
      :circCount="circCount"
      :rectCount="rectCount"
      :triCount="triCount"
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
    }
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
