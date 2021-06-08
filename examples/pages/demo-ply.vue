<template>
  <demo-block>
    <template slot="preview">
      <model-ply
        :backgroundAlpha="0"
        @on-load="onLoad"
        :rotation="rotation"
        src="static/models/ply/binary/minas-tirith-by-clavenmoo.ply"
      ></model-ply>
      <div class="example-loading" v-show="loading"></div>
    </template>
  </demo-block>
</template>

<script>
import DemoBlock from '../components/demo-block.vue';
import ModelPly from '../../src/model-ply.vue';

export default {
  name: 'demo-ply',
  data() {
    return {
      loading: true,
      rotation: {
        x: 0,
        y: 0,
        z: 0,
      },
    };
  },
  methods: {
    onLoad() {
      this.loading = false;
    },
  },
  components: {
    ModelPly,
    DemoBlock,
  },
};
</script>
