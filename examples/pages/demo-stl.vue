<template>
  <demo-block ref="demoBlock">
    <!-- 原本的Stl -->
    <template slot="preview">
      <model-stl :backgroundAlpha="0" @on-load="onLoad" :src="queryParms"></model-stl>
      <div class="example-loading" v-show="loading"></div>
    </template>
    <!-- 压缩后的stl转ply -->
    <template slot="preview2">
       <model-ply v-if="GuipPath" :backgroundAlpha="plyValue" @on-load="onLoad" :src="GuipPath"
      ></model-ply>
      <model-stl v-if="GuipPath2" :backgroundAlpha="plyValue" @on-load="onLoad" :src="GuipPath2"></model-stl>
      <div class="example-loading" v-show="loading1"></div>
      <div class="block">
        <el-slider
          v-model="plyValue"
          :min="0"
          :max="1"
          vertical
          height="100px">
        </el-slider>
      </div>
      <!-- 选择框 -->
      <el-select  @change ="changeSelect" v-model="selectValue" placeholder="请选择">
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value">
        </el-option>
      </el-select>
    </template>
  </demo-block>
</template>

<script>
import DemoBlock from '../components/demo-block.vue';
import ModelStl from '../../src/model-stl.vue';
import ModelPly from '../../src/model-ply.vue';

export default {
  name: 'demo-stl',
  data() {
    return {
      options: [{
        value: 'PLY',
        label: 'PLY文件',
      },
      {
        value: 'STL',
        label: 'STL文件',
      }],
      selectValue: 'PLY', // 筛选的值
      queryParms: '', // 压缩前的STL格式
      GuipPath: '', // 压缩后的PLY格式
      GuipPath2: '', // 压缩后的STL格式
      loading: true,
      loading1: true,
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
    console.log(this.$route.query.id, '参数是什么');
  },
  mounted() {
    switch (this.$route.query.id) {
      case '1': {
        this.$refs.demoBlock.beforeFile = '451519个顶点, 903148 个三角形 (44 M)';
        this.$refs.demoBlock.afterFile = '225737个顶点, 451574 个三角形 (19 M)';
        break;
      }
      case '2': {
        this.$refs.demoBlock.beforeFile = '74995 个顶点, 150006 个三角形 (7 M)';
        this.$refs.demoBlock.afterFile = '37494 个顶点, 75002 个三角形 (3 M)';
        break;
      }
      case '3': {
        this.$refs.demoBlock.beforeFile = '14999 个顶点, 299996 个三角形 (14 M)';
        this.$refs.demoBlock.afterFile = '75001 个顶点, 149998 个三角形 (6 M)';
        break;
      }
      case '4': {
        this.$refs.demoBlock.beforeFile = '108778 个顶点, 217600 个三角形 (11 M)';
        this.$refs.demoBlock.afterFile = '54378 个顶点, 108800 个三角形 (3 M)';
        break;
      }
      case '5': {
        this.$refs.demoBlock.beforeFile = '261903 个顶点, 523878 个三角形 (25 M)';
        this.$refs.demoBlock.afterFile = '130934 个顶点, 261939 个三角形 (5 M)';
        break;
      }
      case '6': {
        this.$refs.demoBlock.beforeFile = '10797 个顶点, 22538 个三角形 (1 M)';
        this.$refs.demoBlock.afterFile = '5172 个顶点, 11268 个三角形 (0.2 M)';
        break;
      }
      case '7': {
        this.$refs.demoBlock.beforeFile = '45207 个顶点, 90414 个三角形 (23 M)';
        this.$refs.demoBlock.afterFile = '22603 个顶点, 45206 个三角形 (1 M)';
        break;
      }
      case '8': {
        this.$refs.demoBlock.beforeFile = '90523 个顶点, 181042 个三角形 (8 M)';
        this.$refs.demoBlock.afterFile = '45262 个顶点, 90520 个三角形 (2 M)';
        break;
      }
      case '9': {
        this.$refs.demoBlock.beforeFile = '128072个顶点, 256144个三角形 (12 M)';
        this.$refs.demoBlock.afterFile = '64036 个顶点, 128072 个三角形 (5 M)';
        break;
      }
      case '10': {
        this.$refs.demoBlock.beforeFile = '102161 个顶点, 204318 个三角形 (9 M)';
        this.$refs.demoBlock.afterFile = '51081 个顶点, 102158 个三角形 (2 M)';
        break;
      }
      default:
        break;
    }
  },
  methods: {
    lookup(obj, keyName) {
      // 首先判断keyName是否含有.
      if (keyName.indexOf('.') !== -1 && keyName !== '.') {
        // 有就拆分
        const keys = keyName.split('.');
        let temp = obj;
        for (let index = 0; index < keys.length; index += 1) {
          temp = temp[keys[index]];
        }
        // 循环结束后返回temp
        return temp;
      }
      // 没有. 就直接使用
      return obj[keyName];
    },
    onLoad() {
      this.loading = false;
      this.loading1 = false;
    },
    // 改变背景颜色
    changebg() {
      this.plybg = 3;
    },
    changeStyle(val, obj) {
      const a = obj.split('.');
      if (val === 'PLY') {
        a[1] = '.ply';
      } else {
        a[1] = '.stl';
      }
      const b = a.join('').replace(',', '');
      return b;
    },
    changeSelect(val) {
      console.log('val', val);
      this.loading1 = true;
      setTimeout(() => {
        this.loading1 = false;
      }, 2000);
      if (val === 'PLY') {
        this.GuipPath = this.changeStyle(val, this.GuipPath ? this.GuipPath : this.GuipPath2);
        this.GuipPath2 = '';
      } else {
        this.GuipPath2 = this.changeStyle(val, this.GuipPath ? this.GuipPath : this.GuipPath2);
        this.GuipPath = '';
      }
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
  .el-select {
    position: absolute;
    top: 72px;
    right: 72px;
  }
</style>
