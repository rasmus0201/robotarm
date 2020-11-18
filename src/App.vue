<template>
  <div id="app">
    <div class="input" v-for="input in inputs" :key="input.id">
      <div>{{ input.title }}</div>
      <vue-slider
        v-model="input.value"
        :adsorb="input.adsorb"
        :interval="input.interval"
        :marks="input.marks"
        :process="true"
        :lazy="true"
        :min="input.min"
        :max="input.max"
        @change="setInput(input)"
      />
    </div>
  </div>
</template>

<script>
import VueSlider from 'vue-slider-component';
import 'vue-slider-component/theme/antd.css';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    VueSlider
  },
  data () {
    return {
      inputs: [
        {
          id: 'S1',
          type: 'SERVO',
          title: 'Servo 1 (Waist)',
          value: 0,
          adsorb: true,
          interval: 5,
          min: 0,
          max: 180,
          marks: true,
        },
        {
          id: 'S2',
          type: 'SERVO',
          title: 'Servo 2 (Shoulder)',
          value: 80,
          adsorb: true,
          interval: 5,
          min: 0,
          max: 180,
          marks: true,
        },
        {
          id: 'S3',
          type: 'SERVO',
          title: 'Servo 3 (Elbow)',
          value: 45,
          adsorb: true,
          interval: 5,
          min: 0,
          max: 180,
          marks: true,
        },
        {
          id: 'S4',
          type: 'SERVO',
          title: 'Servo 4 (Wrist roll)',
          value: 0,
          adsorb: true,
          interval: 5,
          min: 0,
          max: 180,
          marks: true,
        },
        {
          id: 'S5',
          type: 'SERVO',
          title: 'Servo 5 (Wrist pitch)',
          value: 0,
          adsorb: true,
          interval: 5,
          min: 0,
          max: 180,
          marks: true,
        },
        {
          id: 'S6',
          type: 'SERVO',
          title: 'Servo 6 (Gripper)',
          value: 0,
          adsorb: true,
          interval: 5,
          min: 0,
          max: 180,
          marks: true,
        },
        {
          id: 'SPEED',
          type: 'speed',
          title: 'Servo speed',
          value: 20,
          adsorb: true,
          interval: 1,
          min: 5,
          max: 50,
          marks: false,
        },
      ],
    };
  },
  methods: {
    setInput(input) {
      axios.get(`/input?type=${input.type}&id=${input.id}&value=${input.value}`);
    }
  },
};
</script>

<style lang="css">
html, body {
  margin: 0;
  padding: 0;
  font-family: "Helvetica Neue";
}

#app {
  margin: 0;
  padding: 3rem 1.5rem;
}

.input {
  margin-top: 2em;
  margin-bottom: 4em;
}
</style>
