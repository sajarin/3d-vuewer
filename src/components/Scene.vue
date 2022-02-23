<script setup>
  import Button from '../../Button.vue';
    defineProps({
        contents: {
            type: String,
            required: true 
        }, 
    })
</script>

<template>
    <div class="container">
        <div id="canvas-container">
        </div>
        <Button v-on:click="this.$emit('btn-click')" stylings="btn highlighted ripple" text="Back to Upload"/>
    </div>
</template>

<script>
import * as Three from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import { ColladaLoader} from 'three/examples/jsm/loaders/ColladaLoader.js';

export default {
  data() {
    return {
        camera: null,
        renderer: null,
    }
  },
  methods: {
    init: function() {
        let container = document.getElementById('canvas-container');

        this.camera = new Three.PerspectiveCamera(75, container.clientWidth/container.clientHeight, 0.0001, 10);
        this.camera.position.z = 1;

        this.scene = new Three.Scene();
        console.log(this.camera)
        // const manager = new Three.LoadingManager();
        // const objectURLs = [];

        // manager.setURLModifier( ( url ) => {
        //     url = URL.createObjectURL( blobs[ url ] );
        //     objectURLs.push( url );
        //     return url;
        // } );

        // this.loader = new ColladaLoader(manager)
        // // this.collada = this.loader.parse(this.$props.contents.blob)
        // this.loader.load(this.$props.contents.filename, (model) => {
        //     this.scene.add(model.scene)
        //     objectURLs.forEach( (url) => URL.revokeObjectURL(url));
        // }, undefined, function (e) {
        //     console.log(e);
        // });

        this.loader = new ColladaLoader()
        this.object = this.loader.parse(this.$props.contents)
        this.object.scene.traverse( function ( child ) {
            if ( child instanceof Three.Mesh ) {
                child.geometry.center();

            }
        })
        console.log(this.object)
        
        this.scene.add(this.object.scene)

        let geometry = new Three.BoxGeometry(0.2, 0.2, 0.2);
        let material = new Three.MeshNormalMaterial();

        this.mesh = new Three.Mesh(geometry, material);
        console.log(this.mesh)
        // this.scene.add(this.mesh);
        this.scene.background = new Three.Color( 0xF2E7F0)

        this.renderer = new Three.WebGLRenderer({antialias: true});
        this.controls = new OrbitControls(this.camera, this.renderer.domElement);
        this.renderer.setSize(container.clientWidth, container.clientHeight);
        container.appendChild(this.renderer.domElement);
    },
    animate: function() {
        requestAnimationFrame(this.animate);
        this.controls.update()
        this.renderer.render(this.scene, this.camera);
    }, 
    handleResize: function(){

        let container = document.getElementById('canvas-container');
        this.camera.aspect = container.clientWidth/ container.clientHeight;
        this.camera.updateProjectionMatrix();

        this.renderer.setSize( container.clientWidth, container.clientHeight);

    }
  },
  mounted() {
      this.init();
      this.animate();
      window.addEventListener('resize', this.handleResize);
  }
}
</script>

<style scoped>
    .container {
        width: 65%; 
        margin: 0 auto; 
        margin-top: 2em;
    }
    #canvas-container {
        min-height: 65vh; 
        cursor: move;
    }
</style>