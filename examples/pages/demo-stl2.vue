<template>
  <demo-block>
    <!-- 原本的Stl -->
    <template slot="preview">
      <model-stl :backgroundAlpha="0" @on-load="onLoad" :src="queryParms"></model-stl>
      <div class="example-loading" v-show="loading"></div>
    </template>
    <!-- 压缩后的stl转ply -->
    <template slot="preview2">
       <model-ply :backgroundAlpha="plyValue" @on-load="onLoad" :src="GuipPath"
      ></model-ply>
      <div class="example-loading" v-show="loading"></div>
      <div class="block">
        <el-slider
          v-model="plyValue"
          :min="0"
          :max="1"
          vertical
          height="100px">
        </el-slider>
      </div>
    </template>
  </demo-block>
</template>

<script>
import DemoBlock from '../components/demo-block.vue';
import ModelStl from '../../src/model-stl.vue';
import ModelPly from '../../src/model-ply.vue';

export default {
  name: 'demo-stl2',
  data() {
    return {
      queryParms: '', // 压缩前的STL格式
      GuipPath: '', // 压缩后的PLY格式
      loading: true,
      rotation: {
        x: 0,
        y: 0,
        z: 0,
      },
      plybg: 0,
      plyValue: 0, // ply滑块的值
    };
  },
  beforeRouteEnter: (to, from, next) => {
    console.log('准备进入路由模板');
    next();
  },
  created() {
    this.queryParms = this.$route.query.soursePath;
    this.GuipPath = this.$route.query.GuipPath;
    console.log(this.queryParms, '参数是什么');
  },
  methods: {
    onLoad() {
      this.loading = false;
    },
    // 改变背景颜色
    changebg() {
      this.plybg = 3;
    },
  },
  components: {
    ModelStl,
    DemoBlock,
    ModelPly,
  },
};
</script>
<style scoped>
  .block {
    position: absolute;
    top: 70px;
    /* height: 50px; */
    right: 15px;
  }
</style>
