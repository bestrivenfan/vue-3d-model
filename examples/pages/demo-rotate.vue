<template>
  <demo-block :vue-code="code" :html-code="htmlCode">
    <template slot="preview">
      <!-- 支持各文件格式的标签,引入到Rotate里面即可实现动效 -->
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
// import ModelCollada from '../../src/model-collada.vue';
import ModelPly from '../../src/model-ply.vue';

const code = `
<template>
    <model-collada
        :rotation="rotation"
        @on-load="onLoad"
        src="static/models/collada/elf.dae"></model-collada>
</template>

<script>
    import { ModelCollada } from 'vue-3d-model'

    export default {
        data: {
            rotation: {
                x: -Math.PI / 2,
                y: 0,
                z: 0
            }
        },
        methods: {
            onLoad () {
                this.rotate();
            },
            rotate () {
                this.rotation.z += 0.01;
                requestAnimationFrame( this.rotate );
            }
        },
        components: {
            ModelCollada
        }
    }
<\/script>
`;
const htmlCode = `
<body>
    <div id="app">
        <model-collada
            :rotation="rotation"
            @on-load="onLoad"
            src="static/models/collada/elf.dae"></model-collada>
    </div>
    #scripts#
    <script>
        new Vue({
            el: '#app',
            data: {
                rotation: {
                    x: -Math.PI / 2,
                    y: 0,
                    z: 0
                }
            },
            methods: {
                onLoad () {
                    this.rotate();
                },
                rotate () {
                    this.rotation.z += 0.01;
                    requestAnimationFrame( this.rotate );
                }
            }
        })
    <\/script>
</body>
`;

export default {
  name: 'demo-rotate',
  data() {
    return {
      code,
      htmlCode,
      loading: true,
      rotation: {
        x: -Math.PI / 2,
        y: 0,
        z: 0,
      },
    };
  },
  methods: {
    onLoad() {
      this.loading = false;
      this.rotate();
    },
    rotate() {
      requestAnimationFrame(this.rotate);
      this.rotation.z += 0.01;
    },
  },
  components: {
    ModelPly,
    DemoBlock,
  },
};
</script>
