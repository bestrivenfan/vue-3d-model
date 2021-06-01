<template>
  <div class="demo-block">
    <div class="demo-block-preview">
      <slot name="preview"></slot>
    </div>
    <div class="demo-block-code">
      <div class="demo-block-code-box">
        <h3>模型信息</h3>
        <div class="demo-block-code-box-content">
           <ul class="myspeUldemo">
             <li>模型名称 : 阿丽塔手办模型</li>
             <li>模型尺寸 : 10 * 10 * 15</li>
             <li>模型材质 : FDM耗材</li>
             <li>模型来源 : 创想云模型圈</li>
             <li>模型评分 : ☆☆☆☆</li>
             <li>模型打印难度 : ☆☆☆</li>
             <li>模型定位 : 手办卖品</li>
           </ul>
           <button class="speFunbtn" type="submit"> 导入模型 </button>
        </div>
      </div>
      <div class="demo-block-code-box">
        <h3>模型数据</h3>
        <div class="demo-block-code-box-content">
          <pre>
            <!-- <code ref="htmlCode" class="html">{{ htmlCode | code }}</code> -->
          </pre>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import 'highlight.js/styles/github.css';
// import hljs from 'highlight.js';

/* eslint-disable no-irregular-whitespace */

const codeHandler = (code) => {
  const scripts = `<script src="vue.js"><\/script>
    <script src="vue-3d-model.min.js"><\/script>`;

  return code.replace(/#scripts#/gi, scripts);
};

export default {
  name: 'app',
  props: {
    vueCode: String,
    htmlCode: {
      type: String,
      default: '// TODO',
    },
  },
  mounted() {
    // hljs.highlightBlock(this.$refs.vueCode);
    // hljs.highlightBlock(this.$refs.htmlCode);
  },
  data() {
    return {};
  },
  filters: {
    code(val) {
      return codeHandler(val);
    },
  },
  components: {},
};
</script>
<style>
.demo-block {
  width: 100%;
  height: 100%;
  background-color: #fff;
  border-radius: 3px;
}

.demo-block .demo-block-preview {
  width: 50%;
  height: 100%;
  float: left;
  position: relative;
  padding-top: 2rem;
}

.demo-block .demo-block-code {
  width: 50%;
  height: 100%;
  margin-top: 0.46rem;
  font-size: 13px;
  float: left;
}

.demo-block .demo-block-code pre code {
  font: normal 10pt Consolas, Monaco, monospace;
}

.demo-block .demo-block-code .demo-block-code-box {
  width: 100%;
  height: 50%;
  position: relative;
  overflow: hidden;
  padding-top: 0.8rem;
  background: #182c4a;
  margin-top: -0.1rem;
  color:#fff;
}

.demo-block .demo-block-code-box::after {
  content: "";
  display: block;
  position: absolute;
  bottom: 0;
  left: 0;
  width: 90%;
  height: 1px;
  background: -webkit-linear-gradient(
    left,
    #dfe2e7 80%,
    rgba(223, 226, 231, 0) 100%
  );
}

.demo-block .demo-block-code-box h3 {
  width: 100%;
  padding: 10px 20px;
  font-size: 16px;
  font-weight: lighter;
  position: absolute;
}

.demo-block .demo-block-code-box-content {
  position: absolute;
  top: 40px;
  bottom: 10px;
  left: 20px;
  right: 20px;
  overflow: auto;
}
.demo-block .demo-block-code-box-content .speFunbtn {
  white-space: nowrap;
  cursor: pointer;
  color: #fff;
  background-color: #409eff;
  border-color: #409eff;
  outline: none;
  margin: 0;
  transition: .1s;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  padding: 12px 20px;
  font-size: 14px;
  border-radius: 4px;
}
.demo-block .demo-block-code-box-content .myspeUldemo{
  padding: 15px 1rem;
  /* color: #657180; */
  cursor: pointer;
}
.demo-block .demo-block-code-box-content .myspeUldemo li{
  padding: 6px 0px;
  color: #657180;
}

.demo-block .hljs {
  background: transparent;
}

.demo-block .demo-block-code code {
  position: absolute;
  top: 0;
  left: 0;
}
</style>
