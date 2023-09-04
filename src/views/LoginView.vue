<template>
  <div class="gSfgE">
    <div class="login-container">
      <div class="title">
        <div>
          <h3 class="ant-typography">应急救援管理系统</h3>
          <div class="underline"></div>
        </div>
      </div>
      <div class="login-form">
        <login-form/>
      </div>
    </div>
  </div>
  
  <div id="wall-bg">
  </div>
  <a href="https://beian.miit.gov.cn/" target="_blank"
     style="position:absolute; bottom: 20px;z-index: 20;left: 50%; width:200px; margin-left:-100px;text-align:center">
    陕ICP备2023000763号-1</a>
</template>


<script>
import LoginForm from "@/components/LoginForm";
import THREE from "@/assets/js/three.min";

export default {
  components: {
    LoginForm
  },
  created() {
  },
  mounted() {
    setTimeout(() => {
      this.init();
    }, 20);
  },
  unmounted() {
  },
  methods: {
    init() {
      const SEPARATION = 100, AMOUNTX = 50, AMOUNTY = 50;
      let container;
      let camera, scene, renderer;
      let particles, particle, count = 0;
      let mouseX = 0, mouseY = 0;
      let windowHalfX = window.innerWidth / 2;
      let windowHalfY = window.innerHeight;
      init();
      animate();
      
      function init() {
        container = document.getElementById('wall-bg');
        let overlay = document.createElement('span');
        container.appendChild(overlay);
        camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 1, 10000);
        camera.position.z = 1000;
        scene = new THREE.Scene();
        particles = [];
        const PI2 = Math.PI * 2;
        const material = new THREE.ParticleCanvasMaterial({
          color: '#e6eaea',
          program: function (context) {
            context.beginPath();
            context.arc(0, 0, 1, 0, PI2, true);
            context.fill();
          }
        });
        let i = 0;
        for (let ix = 0; ix < AMOUNTX; ix++) {
          for (let iy = 0; iy < AMOUNTY; iy++) {
            particle = particles[i++] = new THREE.Particle(material);
            particle.position.x = ix * SEPARATION - ((AMOUNTX * SEPARATION) / 2);
            particle.position.z = iy * SEPARATION - ((AMOUNTY * SEPARATION) / 2);
            scene.add(particle);
          }
        }
        renderer = new THREE.CanvasRenderer();
        renderer.setSize(window.innerWidth, window.innerHeight);
        container.appendChild(renderer.domElement);
        document.addEventListener('mousemove', onDocumentMouseMove, false);
        window.addEventListener('resize', onWindowResize, false);
      }
      
      function onWindowResize() {
        windowHalfX = window.innerWidth / 2;
        windowHalfY = window.innerHeight / 2;
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        renderer.setSize(window.innerWidth, window.innerHeight);
      }
      
      function onDocumentMouseMove(event) {
        mouseX = event.clientX - windowHalfX;
        mouseY = event.clientY - windowHalfY;
      }
      
      function animate() {
        requestAnimationFrame(animate);
        render();
      }
      
      function render() {
        camera.position.x += (mouseX - camera.position.x) * .05;
        camera.position.y += (-mouseY - camera.position.y) * .05;
        camera.lookAt(scene.position);
        let i = 0;
        for (let ix = 0; ix < AMOUNTX; ix++) {
          for (let iy = 0; iy < AMOUNTY; iy++) {
            particle = particles[i++];
            particle.position.y = (Math.sin((ix + count) * 0.3) * 50) + (Math.sin((iy + count) * 0.5) * 50);
            particle.scale.x = particle.scale.y = (Math.sin((ix + count) * 0.3) + 1) * 2 + (Math.sin((iy + count) * 0.5) + 1) * 2;
          }
        }
        renderer.render(scene, camera);
        count += 0.08;
      }
    },
    
  },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

.login-form {
  width: 250px;
  height: 166px;
  margin: auto;
  
}

.login-container {
  display: flex;
  width: auto;
  flex-direction: row;
  -webkit-box-pack: center;
  justify-content: center;
  -webkit-box-align: center;
  align-items: center;
  padding: 0px 30px 0px 0px;
  height: 130px;
  border: #0a58ca 10px;
  min-height: 400px;
  background: rgba(255, 255, 255, 0%);
  border-radius: 15px;
}

.gSfgE {
  width: 100%;
  /*min-height: 70vh;*/
  height: 100%;
  box-sizing: border-box;
  display: flex;
  -webkit-box-pack: center;
  justify-content: center;
  -webkit-box-align: center;
  align-items: center;
  box-shadow: rgb(0 0 0 / 16%) 0 3px 40px;
  position: absolute;
  z-index: 1;
}

.title {
  flex: 1 1 0;
  background: rgba(97, 161, 255, 75%);
  /*height: 130px;*/
  height: 100%;
  border-radius: 15px 200px 200px 13px;
  padding: 2em 4em;
  margin-right: 1rem;
  transform: scaleY(1.05);
  transform-origin: center bottom;
  display: flex;
  flex-direction: column;
  -webkit-box-align: center;
  align-items: center;
  -webkit-box-pack: center;
  justify-content: center;
  box-shadow: rgb(0 0 0 / 35%) -1px 0px 15px;
}

.ant-typography {
  color: white;
  margin-bottom: 0;
}

.underline {
  width: 182.86px;
  height: 2px;
  background: linear-gradient(337.39deg, rgb(255, 255, 255) 0%, rgb(255, 255, 255) 12.68%, rgb(19, 115, 252) 100%);
}


</style>