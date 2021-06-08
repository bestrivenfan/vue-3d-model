<template>
  <div class="examples">
    <div class="examples-header">
      <img
        width="25px"
        height="25px"
        src="../assets/crealityLogo.png"
        alt=""
        srcset=""
      />
      <a href="https://github.com/hujiulong/vue-3d-model" class="title"
        >创想三维模型可视化组件</a
      >
      <!-- <a href='https://github.com/hujiulong/vue-3d-model' class='github'>View on GitHub</a> -->
    </div>
    <div class="examples-content">
      <ul class="examples-menu">
        <li class="examples-menu-title">模型功能列表</li>
        <!-- <li class="examples-menu-item">
          <router-link to="demo-basic">基础模型交互</router-link>
        </li> -->
        <!-- <li class="examples-menu-item">
          <router-link to="demo-rotate">3D动画模型</router-link>
        </li> -->
        <!-- <li class="examples-menu-item">
          <router-link to="demo-controls">基础模型可控优化</router-link>
        </li> -->
        <!-- <li class="examples-menu-item">
          <router-link to="demo-background">background</router-link>
        </li> -->
        <!-- <li class="examples-menu-item">
          <router-link to="demo-snapshot">模型缩略图</router-link>
        </li> -->
        <!-- <li class="examples-menu-title">基础模型事件</li> -->
        <!-- <li class="examples-menu-item">
          <router-link to="demo-event">鼠标移入事件</router-link>
        </li> -->
        <!-- <li class="examples-menu-title">支持的模型上传格式</li> -->
        <!-- <li class="examples-menu-item">
          <router-link to="demo-json">JSON 模型 (.json)</router-link>
        </li> -->
        <!-- <li class="examples-menu-item">
          <router-link to="demo-obj">OBJ 模型 (.obj)</router-link>
        </li> -->
        <!-- <li class="examples-menu-item">
          <router-link to="demo-obj-mtl">OBJ + MTL 模型 </router-link>
        </li> -->
        <!-- <li class="examples-menu-item">
          <router-link to="demo-fbx">FBX 模型 (.fbx)</router-link>
        </li> -->
        <li class="examples-menu-item">
          <div class="leftbanner">
            <div class="leftbanner-inner">模型压缩对比(.stl)</div>
            <!-- 佛像 -->
            <div class="leftbanner-next">
              <router-link @click.native="flashPage(0)" :to="{name:'demo-stl',query:{soursePath: 'static/models/stl/pusa.stl', GuipPath:'static/models/ply/foxiang/simplify_foxiang.ply', id:1}}">佛像(STL) 44M</router-link>
            </div>
            <!-- 乌龟 -->
            <div class="leftbanner-next">
              <router-link @click.native="flashPage(1)" :to="{name:'demo-stl',query:{soursePath: 'static/models/stl/wugui.stl', GuipPath:'static/models/ply/wugui/simplify_wugui.ply', id:2}}">乌龟(STL) 7M</router-link>
            </div>
            <!-- 鱼叉 -->
            <div class="leftbanner-next">
              <router-link @click.native="flashPage(2)" :to="{name:'demo-stl',query:{soursePath: 'static/models/stl/yucha.stl', GuipPath:'static/models/ply/yucha/simplify_yucha.ply', id:3}}">鱼叉(STL) 14M</router-link>
            </div>
             <!-- 心脏 -->
            <div class="leftbanner-next">
              <router-link @click.native="flashPage(3)" :to="{name:'demo-stl',query:{soursePath: 'static/models/stl/xinzhang.stl', GuipPath:'static/models/ply/xinzhang/simplify_xinzhang.ply', id:4}}">心脏(STL) 11M</router-link>
            </div>
             <!-- 飞行器 -->
            <div class="leftbanner-next">
              <router-link @click.native="flashPage(4)" :to="{name:'demo-stl',query:{soursePath: 'static/models/stl/feixingqi.stl', GuipPath:'static/models/ply/feixingqi/simplify_feixingqi.ply', id:5}}">飞行器(STL) 25M</router-link>
            </div>
             <!-- DNA -->
            <div class="leftbanner-next">
              <router-link @click.native="flashPage(5)" :to="{name:'demo-stl',query:{soursePath: 'static/models/stl/dna.stl', GuipPath:'static/models/ply/dna/simplify_dna.ply', id:6}}">DNA(STL) 1M</router-link>
            </div>
            <!-- 轮胎 -->
            <div class="leftbanner-next">
              <router-link @click.native="flashPage(6)" :to="{name:'demo-stl',query:{soursePath: 'static/models/stl/luntai.stl', GuipPath:'static/models/ply/luntai/simplify_luntai.ply', id:7}}">轮胎(STL) 23M</router-link>
            </div>
            <!-- 右脚 -->
            <div class="leftbanner-next">
              <router-link @click.native="flashPage(7)" :to="{name:'demo-stl',query:{soursePath: 'static/models/stl/youjiao.stl', GuipPath:'static/models/ply/youjiao/simplify_youjiao.ply', id:8}}">右脚骨骼(STL) 8M</router-link>
            </div>
            <!-- 狮子 -->
            <div class="leftbanner-next">
              <router-link @click.native="flashPage(8)" :to="{name:'demo-stl',query:{soursePath: 'static/models/stl/shizi.stl', GuipPath:'static/models/ply/shizi/simplify_shizi.ply', id:9}}">石狮子 (STL) 12M</router-link>
            </div>
            <!-- 月球等 -->
            <div class="leftbanner-next">
              <router-link @click.native="flashPage(9)" :to="{name:'demo-stl',query:{soursePath: 'static/models/stl/yueqiud.stl', GuipPath:'static/models/ply/yueqiud/simplify_yueqiud.ply', id:10}}">月球灯(STL) 9M</router-link>
            </div>
          </div>
        </li>
        <!-- <li class="examples-menu-item">
          <router-link to="demo-collada">Collada 模型 (.dae)</router-link>
        </li> -->
        <!-- <li class="examples-menu-item">
          <router-link to="demo-ply">PLY 模型 (.ply)</router-link>
        </li> -->
        <!-- <li class="examples-menu-item">
          <router-link to="demo-gltf">glTF 模型 (.gltf)</router-link>
        </li> -->
      </ul>
      <div class="examples-pages">
        <router-view></router-view>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'app',
  data() {
    return {
    };
  },
  mounted() {
    const index = localStorage.getItem('indexFlag');
    if (index) {
      this.$children[Number(index)].$el.style.color = '#fff';
    } else {
      this.$children[0].$el.style.color = '#fff';
    }
  },
  methods: {
    flashPage(index) {
      console.log('this.$router', this.$children[0]);
      localStorage.setItem('indexFlag', index);
      this.$router.go(0);
    },
  },
};
</script>
<style>
* {
  margin: 0;
  border: 0;
  padding: 0;
  box-sizing: border-box;
}
ul,
ol,
li {
  list-style: none;
}
a {
  color: #657180;
  text-decoration: none;
}
html,
body,
.examples {
  width: 100%;
  height: 100%;
  color: #657180;
}
body {
  background: #eee;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB",
    "Microsoft YaHei", SimSun, sans-serif;
  background: #fff;
}
.examples-header {
  position: absolute;
  top: 0;
  width: 100%;
  background-image: url("../assets/bg1.png");
  text-shadow: 0 1px 0 rgba(255, 255, 255, 0.5);
  /* border-bottom: solid 1px #dfe2e7; */
  padding: 15px 15px 15px 30px;
  line-height: 20px;
}
.examples-header .title {
  font-size: 18px;
  color: #fff;
  display: inline-block;
  vertical-align: 7px;
  margin-left: 20px;
}

.examples-header .github {
  color: #39f;
  display: inline-block;
  font-size: 12px;
  float: right;
}

.examples-content {
  width: 100%;
  height: 100%;
  padding-top: 52px;
  overflow: hidden;
}

.examples-menu {
  float: left;
  width: 230px;
  height: 100%;
  padding: 15px 30px;
  background: #182c4a;
  color: aliceblue;
  border-right: 2px solid rgb(101, 113, 128);
  margin-top: 0.4rem;
}

.examples-menu .examples-menu-item {
  display: block;
  width: 100%;
  height: 30px;
  line-height: 30px;
  font-size: 18px;
  cursor: pointer;
}
/* 压缩功能模块 */
.examples-menu .examples-menu-item .leftbanner {
  text-indent: 1.5em;
  font-size: 16px;
}

.examples-menu .examples-menu-item .leftbanner .leftbanner-next {
  text-indent: 3em;
  font-size: 14px;
}
.examples-menu .examples-menu-item .leftbanner .leftbanner-next a:hover{
  /* background-color: cyan; */
  color: white;
}

.examples-menu .examples-menu-item > a {
  display: block;
  width: 100%;
  height: 100%;
  text-indent: 1rem;
}

.examples-menu .examples-menu-title {
  display: block;
  width: 100%;
  padding: 6px 0;
  margin-top: 10px;
  font-size: 17px;
  font-weight: bold;
  cursor: pointer;
}

.examples-menu .examples-menu-item:hover > a {
  color: #39f;
}

.examples-pages {
  height: 100%;
  overflow: hidden;
}

.example-loading {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  text-align: center;
}
.example-loading::before {
  content: "";
  border-radius: 100%;
  margin: 2px;
  border: 3px solid #20a0ff;
  border-bottom-color: transparent;
  height: 36px;
  width: 36px;
  display: inline-block;
  animation: loading-rotate 0.75s 0s linear infinite;
  vertical-align: middle;
}

.example-loading::after {
  content: "";
  display: inline-block;
  height: 100%;
  width: 0;
  vertical-align: middle;
}

@keyframes loading-rotate {
  0% {
    transform: rotate(0) scale(1);
  }
  50% {
    transform: rotate(180deg) scale(0.6);
  }
  100% {
    transform: rotate(360deg) scale(1);
  }
}
</style>
