<template>
  <div class="demo-block">
    <div class="demo-block-preview">
       <h3 style="color:#fff;padding-left:1rem;font-size:14px">源模型 (STL) :  <span style="color: #657180;">{{beforeFile}}</span></h3>
      <slot name="preview"></slot>
    </div>
    <div class="demo-block-preview">
      <h3 style="color:#fff;padding-left:1rem;font-size:14px">压缩后 (PLY) : <span style="color: #657180;">{{afterFile}}</span></h3>
      <slot name="preview2"></slot>
    </div>
  </div>
</template>

<script>
// import 'highlight.js/styles/github.css';
export default {
  name: 'app',
  props: {
    vueCode: String,
  },
  data() {
    return {
      beforeFile: '', // 压缩前的STL格式
      afterFile: '', // 压缩后的PLY格式
    };
  },
  mounted() {
  },
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
  background: #182c4a;
  margin-top: 0.4rem;
  /* border-left: 3px solid #657180; */
  /* border-right: 3px solid #657180; */
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
  background: #182c4a;
  margin-top: -0.1rem;
  color: #fff;
  /* border: 2px solid #657180; */
  /* border-bottom: none; */
  /* border-right: none; */
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
  padding: 18px 20px;
  font-size: 18.72px;
  font-weight: bold;
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
  transition: 0.1s;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  padding: 12px 20px;
  font-size: 14px;
  border-radius: 4px;
  margin-left: 1rem;
}
.demo-block .demo-block-code-box-content .myspeUldemo {
  padding: 15px 1rem;
  /* color: #657180; */
  cursor: pointer;
}
.demo-block .demo-block-code-box-content .myspeUldemo li {
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
