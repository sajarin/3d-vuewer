<script setup>
  import Button from './Button.vue';
    defineProps({
        contents: {
            type: String,
            required: false 
        },
        key: {
            type: Number,
            required: true
        }
         
    })
</script>

<template>
    <div class="container">
        <div id="canvas-container"> </div>
        <Button id="return" v-on:click="$emit('btn-click')" stylings="btn highlighted ripple" text="Back to Upload"/>
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
        container.innerText = ""

        this.camera = new Three.PerspectiveCamera(45, container.clientWidth/container.clientHeight, 0.01, 100);
        this.camera.position.set(7.5,7.5,7.5)
        this.scene = new Three.Scene();

        const size = 10;
        const divisions = 10;
        const gridHelper = new Three.GridHelper( size, divisions, 0xff0000, 0x0000ff);
        this.scene.add( gridHelper );

        this.loader = new ColladaLoader()
        this.object = this.loader.parse(this.$props.contents)
        this.object.scene.traverse( function ( child ) {
            if ( child instanceof Three.Mesh ) {
                child.geometry.center();
            }
        })
        this.scene.add(this.object.scene)
        this.scene.background = new Three.Color( 0xe8e8e8)

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
      console.log(this.$props.key)
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
    #return {
        width: 100%;
        margin: 0 auto; 
        margin-top: 2em;
    }
    @media (min-width: 481px) and (max-width: 767px) {
        .container{
            width: 95%;
            margin: 0 auto; 
            margin-top: 2em;
        }
        #canvas-conatiner {
            width: 95%;
            min-height: 55%;
        }
    }

    @media (min-width: 320px) and (max-width: 480px) {
        .container{
            width: 95%;
            margin: 0 auto; 
            margin-top: 2em;
        }
        #canvas-conatiner {
            width: 95%;
            min-height: 55%;
        }
    }

</style>