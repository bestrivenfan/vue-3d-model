<template>
  <div style="width: 100%; height: 100%; margin: 0; border: 0; padding: 0;">
    <canvas v-if="suportWebGL" ref="canvas" style="width: 100%; height: 100%;"></canvas>
    <div v-else>
      <slot>
        Your browser does not seem to support <a href="http://khronos.org/webgl/wiki/Getting_a_WebGL_Implementation" style="color:#000">WebGL</a>.<br/>'
      </slot>
    </div>
  </div>
</template>

<script>

import {
  Object3D,
  Vector2,
  Vector3,
  Color,
  Scene,
  Raycaster,
  WebGLRenderer,
  PerspectiveCamera,
  AmbientLight,
  PointLight,
  HemisphereLight,
  DirectionalLight,
  LinearEncoding,
} from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import { getSize, getCenter } from './util';

const suportWebGL = (() => {
  try {
    const canvas = document.createElement('canvas');
    return !!(window.WebGLRenderingContext && (canvas.getContext('webgl') || canvas.getContext('experimental-webgl')));
  } catch (e) {
    return false;
  }
})();

const DEFAULT_GL_OPTIONS = {
  antialias: true,
  alpha: true,
};

export default {
  props: {
    src: {
      type: String,
    },
    width: {
      type: Number,
    },
    height: {
      type: Number,
    },
    position: {
      type: Object,
      default() {
        return { x: 0, y: 0, z: 0 };
      },
    },
    rotation: {
      type: Object,
      default() {
        return { x: 0, y: 0, z: 0 };
      },
    },
    scale: {
      type: Object,
      default() {
        return { x: 1, y: 1, z: 1 };
      },
    },
    lights: {
      type: Array,
      default() {
        return [];
      },
    },
    cameraPosition: {
      type: Object,
      default() {
        return { x: 0, y: 0, z: 0 };
      },
    },
    cameraRotation: {
      type: Object,
      default() {
        return { x: 0, y: 0, z: 0 };
      },
    },
    cameraUp: {
      type: Object,
    },
    cameraLookAt: {
      type: Object,
    },
    backgroundColor: {
      default: 0xffffff,
    },
    backgroundAlpha: {
      type: Number,
      default: 1,
    },
    controlsOptions: {
      type: Object,
    },
    crossOrigin: {
      default: 'anonymous',
    },
    outputEncoding: {
      type: Number,
      default: LinearEncoding,
    },
    glOptions: {
      type: Object,
    },
  },
  data() {
    return {
      suportWebGL,
      size: {
        width: this.width,
        height: this.height,
      },
      object: null,
      raycaster: new Raycaster(),
      mouse: new Vector2(),
      // params1 :摄像机视锥体垂直视野角度，最小值为0，最大值为180，默认值为50
      // params2 :表示摄像机视锥体长宽比，默认长宽比为1，即表示看到的是正方形，实际项目中使用的是屏幕的宽高比
      // params3 :表示摄像机视锥体近端面，这个值默认为0.1
      // params4 :far表示摄像机视锥体远端面，默认为2000，这个值可以是无限的，说的简单点就是我们视觉所能看到的最远距离。
      camera: new PerspectiveCamera(45, 1, 0.1, 100000),
      scene: new Scene(),
      wrapper: new Object3D(),
      renderer: null,
      controls: null,
      allLights: [],
      clock: typeof performance === 'undefined' ? Date : performance,
      reqId: null, // requestAnimationFrame id
    };
  },
  computed: {
    hasListener() {
      // 判断是否有鼠标事件监听，用于减少不必要的拾取判断
      /* eslint-disable no-underscore-dangle */
      const events = this._events;
      const result = {};

      ['on-mousemove', 'on-mouseup', 'on-mousedown', 'on-click'].forEach((name) => {
        result[name] = !!events[name] && events[name].length > 0;
      });

      return result;
    },
  },
  mounted() {
    if (this.width === undefined || this.height === undefined) {
      this.size = {
        width: this.$el.offsetWidth,
        height: this.$el.offsetHeight,
      };
    }

    const options = Object.assign({}, DEFAULT_GL_OPTIONS, this.glOptions, {
      canvas: this.$refs.canvas,
    });

    this.renderer = new WebGLRenderer(options);
    this.renderer.shadowMap.enabled = true;
    this.renderer.outputEncoding = this.outputEncoding;

    this.controls = new OrbitControls(this.camera, this.$el);
    this.controls.type = 'orbit';

    this.scene.add(this.wrapper);

    this.load();
    this.update();

    this.$el.addEventListener('mousedown', this.onMouseDown, false);
    this.$el.addEventListener('mousemove', this.onMouseMove, false);
    this.$el.addEventListener('mouseup', this.onMouseUp, false);
    this.$el.addEventListener('click', this.onClick, false);

    window.addEventListener('resize', this.onResize, false);

    this.animate();
  },
  beforeDestroy() {
    cancelAnimationFrame(this.reqId);

    this.scene.dispose();
    this.renderer.dispose();

    if (this.controls) {
      this.controls.dispose();
    }

    this.$el.removeEventListener('mousedown', this.onMouseDown, false);
    this.$el.removeEventListener('mousemove', this.onMouseMove, false);
    this.$el.removeEventListener('mouseup', this.onMouseUp, false);
    this.$el.removeEventListener('click', this.onClick, false);

    window.removeEventListener('resize', this.onResize, false);
  },
  watch: {
    src() {
      this.load();
    },
    rotation: {
      deep: true,
      handler(val) {
        if (!this.object) return;
        this.object.rotation.set(val.x, val.y, val.z);
      },
    },
    position: {
      deep: true,
      handler(val) {
        if (!this.object) return;
        this.object.position.set(val.x, val.y, val.z);
      },
    },
    scale: {
      deep: true,
      handler(val) {
        if (!this.object) return;
        this.object.scale.set(val.x, val.y, val.z);
      },
    },
    lights: {
      deep: true,
      handler() {
        this.updateLights();
      },
    },
    size: {
      deep: true,
      handler() {
        this.updateCamera();
        this.updateRenderer();
      },
    },
    controlsOptions: {
      deep: true,
      handler() {
        this.updateControls();
      },
    },
    backgroundAlpha() {
      this.updateRenderer();
    },
    backgroundColor() {
      this.updateRenderer();
    },
  },
  methods: {
    onResize() {
      if (this.width === undefined || this.height === undefined) {
        this.$nextTick(() => {
          this.size = {
            width: this.$el.offsetWidth,
            height: this.$el.offsetHeight,
          };
        });
      }
    },
    onMouseDown(event) {
      if (!this.hasListener['on-mousedown']) return;

      const intersected = this.pick(event.clientX, event.clientY);
      this.$emit('on-mousedown', intersected);
    },
    onMouseMove(event) {
      if (!this.hasListener['on-mousemove']) return;

      const intersected = this.pick(event.clientX, event.clientY);
      this.$emit('on-mousemove', intersected);
    },
    onMouseUp(event) {
      if (!this.hasListener['on-mouseup']) return;

      const intersected = this.pick(event.clientX, event.clientY);
      this.$emit('on-mouseup', intersected);
    },
    onClick(event) {
      if (!this.hasListener['on-click']) return;

      const intersected = this.pick(event.clientX, event.clientY);
      this.$emit('on-click', intersected);
    },
    pick(x, y) {
      if (!this.object) return null;

      const rect = this.$el.getBoundingClientRect();

      x -= rect.left;
      y -= rect.top;

      this.mouse.x = (x / this.size.width) * 2 - 1;
      this.mouse.y = -(y / this.size.height) * 2 + 1;

      this.raycaster.setFromCamera(this.mouse, this.camera);

      const intersects = this.raycaster.intersectObject(this.object, true);

      return (intersects && intersects.length) > 0 ? intersects[0] : null;
    },
    update() {
      this.updateRenderer();
      this.updateCamera();
      this.updateLights();
      this.updateControls();
    },
    updateModel() {
      const { object } = this;

      if (!object) return;

      const { position } = this;
      const { rotation } = this;
      const { scale } = this;

      object.position.set(position.x, position.y, position.z);
      object.rotation.set(rotation.x, rotation.y, rotation.z);
      object.scale.set(scale.x, scale.y, scale.z);
    },
    updateRenderer() {
      const { renderer } = this;

      renderer.setSize(this.size.width, this.size.height);
      renderer.setPixelRatio(window.devicePixelRatio || 1);
      renderer.setClearColor(new Color(this.backgroundColor).getHex());
      renderer.setClearAlpha(this.backgroundAlpha);
    },
    updateCamera() {
      const { camera } = this;
      const { object } = this;

      camera.aspect = this.size.width / this.size.height;
      camera.updateProjectionMatrix();

      if (!this.cameraLookAt && !this.cameraUp) {
        if (!object) return;

        const distance = getSize(object).length();

        camera.position.set(this.cameraPosition.x, this.cameraPosition.y, this.cameraPosition.z);
        camera.rotation.set(this.cameraRotation.x, this.cameraRotation.y, this.cameraRotation.z);

        if (this.cameraPosition.x === 0 && this.cameraPosition.y === 0 && this.cameraPosition.z === 0) {
          camera.position.z = distance;
        }

        camera.lookAt(new Vector3());
      } else {
        camera.position.set(this.cameraPosition.x, this.cameraPosition.y, this.cameraPosition.z);
        camera.rotation.set(this.cameraRotation.x, this.cameraRotation.y, this.cameraRotation.z);
        camera.up.set(this.cameraUp.x, this.cameraUp.y, this.cameraUp.z);

        camera.lookAt(new Vector3(this.cameraLookAt.x, this.cameraLookAt.y, this.cameraLookAt.z));
      }
    },
    updateLights() {
      this.scene.remove(...this.allLights);

      this.allLights = [];

      this.lights.forEach((item) => {
        if (!item.type) return;

        const type = item.type.toLowerCase();

        let light = null;

        if (type === 'ambient' || type === 'ambientlight') {
          const color = item.color === 0x000000 ? item.color : item.color || 0x404040;
          const intensity = item.intensity === 0 ? item.intensity : item.intensity || 1;

          light = new AmbientLight(color, intensity);
        } else if (type === 'point' || type === 'pointlight') {
          const color = item.color === 0x000000 ? item.color : item.color || 0xffffff;
          const intensity = item.intensity === 0 ? item.intensity : item.intensity || 1;
          const distance = item.distance || 0;
          const decay = item.decay === 0 ? item.decay : item.decay || 1;

          light = new PointLight(color, intensity, distance, decay);

          if (item.position) {
            light.position.copy(item.position);
          }
        } else if (type === 'directional' || type === 'directionallight') {
          const color = item.color === 0x000000 ? item.color : item.color || 0xffffff;
          const intensity = item.intensity === 0 ? item.intensity : item.intensity || 1;

          light = new DirectionalLight(color, intensity);

          if (item.position) {
            light.position.copy(item.position);
          }

          if (item.target) {
            light.target.copy(item.target);
          }
        } else if (type === 'hemisphere' || type === 'hemispherelight') {
          const skyColor = item.skyColor === 0x000000 ? item.skyColor : item.skyColor || 0xffffff;
          const groundColor = item.groundColor === 0x000000 ? item.groundColor : item.groundColor || 0xffffff;
          const intensity = item.intensity === 0 ? item.intensity : item.intensity || 1;

          light = new HemisphereLight(skyColor, groundColor, intensity);

          if (item.position) {
            light.position.copy(item.position);
          }
        }

        this.allLights.push(light);
        this.scene.add(light);
      });
    },
    updateControls() {
      if (this.controlsOptions) {
        Object.assign(this.controls, this.controlsOptions);
      }
    },
    load() {
      if (!this.src) return;
      // const query = JSON.stringify(this.src);
      // query = window.encodeURIComponent(query);
      if (this.object) {
        this.wrapper.remove(this.object);
      }
      // this.loader.setRequestHeader(this.requestHeader);
      // console.log('模型加载前地址是什么', query);
      this.loader.load(this.src, (...args) => {
        const object = this.getObject(...args);

        if (this.process) {
          this.process(object);
        }

        this.addObject(object);

        this.$emit('on-load');
      }, (xhr) => {
        this.$emit('on-progress', xhr);
      }, (err) => {
        this.$emit('on-error', err);
      });
    },
    getObject(object) {
      return object;
    },
    addObject(object) {
      const center = getCenter(object);

      // correction position
      this.wrapper.position.copy(center.negate());

      this.object = object;
      this.wrapper.add(object);

      this.updateCamera();
      this.updateModel();
    },
    animate() {
      this.reqId = requestAnimationFrame(this.animate);
      this.render();
    },
    render() {
      this.renderer.render(this.scene, this.camera);
    },
  },
};

</script>
