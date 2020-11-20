<template>
  <div id="app">
    <div
      class="input"
      v-for="input in inputs"
      :key="input.id"
    >
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
import mqtt from 'mqtt';
import VueSlider from 'vue-slider-component';
import 'vue-slider-component/theme/antd.css';

export default {
  name: 'App',
  components: {
    VueSlider
  },
  data() {
    return {
      mqtt: {
        client: null,
        host: '10.142.106.136',
        port: 9002,
        path: '/ws',
        clean: true,
        protocol: 'ws',
        username: 'robotarm',
        password: 'robotarm',
        clientId: 'web_' + parseInt(Math.random() * 100, 10),
      },
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
          value: 70,
          adsorb: true,
          interval: 5,
          min: 0,
          max: 150,
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
          max: 150,
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
          max: 80,
          marks: true,
        },
        {
          id: 'SPEED',
          type: 'SPEED',
          title: 'Servo speed',
          value: 8,
          adsorb: true,
          interval: 1,
          min: 3,
          max: 50,
          marks: false,
        },
      ],
    };
  },
  mounted() {
    this.connectMQTT();

    this.resetAll();
  },
  methods: {
    connectMQTT() {
      this.mqtt.client = mqtt.connect(
        `${this.mqtt.protocol}://${this.mqtt.host}:${this.mqtt.port}${this.mqtt.path}`,
        {
          host: this.mqtt.host,
          port: this.mqtt.port,
          path: this.mqtt.path,
          clientId: this.mqtt.clientId,
          clean: this.mqtt.clean,
          username: this.mqtt.username,
          password: this.mqtt.password,
          protocol: this.mqtt.protocol,

          keepalive: 30,
          reconnectPeriod: 1000,
          connectTimeout: 30 * 1000,
        });

      this.mqtt.client.on('error', (err) => {
        console.log(err);
        this.mqtt.client.end();
      });

      this.mqtt.client.on('connect', () => {
        console.log('client connected:' + this.mqtt.clientId);
        this.mqtt.client.subscribe('#');
      });

      this.mqtt.client.on('message', (topic, message) => {
        console.log('Received Message:= ' + message.toString() + '\nOn topic:= ' + topic);
      });

      this.mqtt.client.on('close', () => {
        console.log(this.mqtt.clientId + ' disconnected');
      });
    },

    resetAll() {
      for (const input of this.inputs) {
        this.setInput(input);
      }
    },

    setInput(input) {
      switch (input.type) {
        case 'SPEED':
          this.mqtt.client.publish(`setting/speed`, String(input.value));
          break;
        case 'SERVO':
          this.mqtt.client.publish(`servo/${input.id}`, String(input.value));
          break;
      }
    }
  },
};
</script>

<style lang="css">
  html,
  body {
      margin: 0;
      padding: 0;
      font-family: 'Helvetica Neue';
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
