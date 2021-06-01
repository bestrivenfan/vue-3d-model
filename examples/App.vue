<template>
    <div class="examples">
        <div class="examples-header">
            <img width="25px" height="25px" src="../assets/crealityLogo.png" alt="" srcset="">
            <a href="https://github.com/hujiulong/vue-3d-model" class="title">创想三维模型可视化组件</a>
            <!-- <a href="https://github.com/hujiulong/vue-3d-model" class="github">View on GitHub</a> -->
        </div>
        <div class="examples-content">
            <ul class="examples-menu">
                <li class="examples-menu-title">模型交互示例</li>
                <li class="examples-menu-item">
                    <router-link to="demo-basic">基础模型交互</router-link>
                </li>
                <li class="examples-menu-item">
                    <router-link to="demo-rotate">3D动画模型</router-link>
                </li>
                <!-- <li class="examples-menu-item">
                    <router-link to="demo-controls">基础模型可控优化</router-link>
                </li> -->
                <!-- <li class="examples-menu-item">
                    <router-link to="demo-background">background</router-link>
                </li> -->
                <li class="examples-menu-item">
                    <router-link to="demo-snapshot">模型缩略图</router-link>
                </li>
                <!-- <li class="examples-menu-title">基础模型事件</li> -->
                <!-- <li class="examples-menu-item">
                    <router-link to="demo-event">鼠标移入事件</router-link>
                </li> -->
                <li class="examples-menu-title">支持的模型上传格式</li>
                <!-- <li class="examples-menu-item">
                    <router-link to="demo-json">JSON 模型 (.json)</router-link>
                </li> -->
                <li class="examples-menu-item">
                    <router-link to="demo-obj">OBJ 模型 (.obj)</router-link>
                </li>
                <li class="examples-menu-item">
                    <router-link to="demo-obj-mtl">OBJ + MTL 模型 </router-link>
                </li>
                <li class="examples-menu-item">
                    <router-link to="demo-fbx">FBX 模型 (.fbx)</router-link>
                </li>
                <li class="examples-menu-item">
                    <router-link to="demo-stl">STL 模型 (.stl)</router-link>
                </li>
                <li class="examples-menu-item">
                    <router-link to="demo-collada">Collada 模型 (.dae)</router-link>
                </li>
                <li class="examples-menu-item">
                    <router-link to="demo-ply">PLY 模型 (.ply)</router-link>
                </li>
                <li class="examples-menu-item">
                    <router-link to="demo-gltf">glTF 模型 (.gltf)</router-link>
                </li>
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
};
</script>
<style>
* {
    margin: 0;
    border: 0;
    padding: 0;
    box-sizing: border-box;
}
ul, ol, li {
    list-style: none;
}
a {
    color: #657180;
    text-decoration: none;
}
html, body, .examples {
    width: 100%;
    height: 100%;
    color: #657180;
}
body {
    background: #eee;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
                 "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei",
                 SimSun, sans-serif;
    background: #fff;
}
.examples-header {
    position: absolute;
    top: 0;
    width: 100%;
    background-image: url('../assets/bg1.png');
    text-shadow: 0 1px 0 rgba(255,255,255,0.5);
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
}

.examples-menu .examples-menu-item {
    display: block;
    width: 100%;
    height: 30px;
    line-height: 30px;
    font-size: 14px;
    cursor: pointer;
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
    font-size: 14px;
    font-weight: bold;
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
    content: '';
    border-radius: 100%;
    margin: 2px;
    border: 3px solid #20a0ff;
    border-bottom-color: transparent;
    height: 36px;
    width: 36px;
    display: inline-block;
    animation: loading-rotate .75s 0s linear infinite;
    vertical-align: middle;
}

.example-loading::after {
    content: '';
    display: inline-block;
    height: 100%;
    width: 0;
    vertical-align: middle;
}

@keyframes loading-rotate{
    0% {
        transform: rotate(0) scale(1);
    }
    50% {
        transform: rotate(180deg) scale(.6);
    }
    100% {
        transform: rotate(360deg) scale(1);
    }
}

</style>
