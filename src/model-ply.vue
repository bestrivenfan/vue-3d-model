<script>
import { PLYLoader } from 'three/examples/jsm/loaders/PLYLoader';
import {
  Mesh,
  MeshStandardMaterial,
} from 'three';
import mixin from './model-mixin.vue';

export default {
  name: 'model-ply',
  mixins: [mixin],
  props: {
    lights: {
      type: Array,
      default() {
        return [
          {
            type: 'HemisphereLight',
            position: { x: 20, y: 4, z: 2 },
            skyColor: 0xaaaaff,
            groundColor: 0x806060,
            intensity: 0.2,
          },
          {
            type: 'DirectionalLight',
            position: { x: 1, y: 1, z: 1 },
            color: 0xffffff,
            intensity: 0.8,
          },
          {
            type: 'DirectionalLight',
            position: { x: -1, y: -1, z: -1 },
            color: 0xffffff,
            intensity: 0.8,
          },
        ];
      },
    },
  },
  data() {
    return {
      loader: new PLYLoader(),
    };
  },
  methods: {
    getObject(geometry) {
      geometry.computeVertexNormals();
      return new Mesh(geometry, new MeshStandardMaterial());
    },
  },
};
</script>
