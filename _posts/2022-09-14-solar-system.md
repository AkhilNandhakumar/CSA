<script type="module" defer>
      "use strict";let style=document.createElement("style");style.type="text/css",style.innerHTML=".dg.ac {z-index:2 !important;} body {margin: 0;padding: 0;} #text {z-index: 2;pointer-events: none;} canvas {position: fixed;width: 100vw;height: 100vh;overflow: hidden;z-index: 1;} #button {z-index:2;position:fixed;bottom:10px;left:10px;background-color: #fff;border: 1px solid #d5d9d9;border-radius: 8px;box-shadow: rgba(213, 217, 217, .5) 0 2px 5px 0;box-sizing: border-box;color: #0f1111;cursor: pointer;display: inline-block;font-family: \"Amazon Ember\",sans-serif;font-size: 13px;line-height: 29px;padding: 0 10px 0 11px;text-align: center;text-decoration: none;user-select: none;-webkit-user-select: none;touch-action: manipulation;vertical-align: middle;width: 100px;};#button:hover {background-color: #f7fafa;};#button:focus {border-color: #008296;box-shadow: rgba(213, 217, 217, .5) 0 2px 5px 0;outline: 0;};",document.getElementsByTagName("head")[0].appendChild(style);let button = document.createElement("input");button.id="button";button.type='button';button.value="Go back";button.onclick=()=>{history.back()};document.getElementsByTagName("body")[0].appendChild(button);import*as e from"https://cdnjs.cloudflare.com/ajax/libs/three.js/r99/three.module.min.js";import{OrbitControls as t}from"https://cdn.jsdelivr.net/npm/three@0.121.1/examples/jsm/controls/OrbitControls.js";import*as i from"https://unpkg.com/dat.gui@0.7.7/build/dat.gui.module.js";let gui=new i.GUI;gui.domElement.id='gui';function rotateVector(t,i,o,s){let a=new e.Vector3(t.x,t.y,t.z),n=a.x;a.x=a.x*Math.cos(s)-a.y*Math.sin(s),a.y=n*Math.sin(s)+a.y*Math.cos(s),n=a.x,a.x=a.x*Math.cos(o)+a.z*Math.sin(o),a.z=-n*Math.sin(o)+a.z*Math.cos(o);let r=a.y;return a.y=a.y*Math.cos(i)-a.z*Math.sin(i),a.z=r*Math.sin(i)+a.z*Math.cos(i),a}function distance(e,t){let i=e.x-t.x,o=e.y-t.y,s;return Math.sqrt(i**2+o**2+(e.z-t.z)**2)||0}function onWindowResize(){camera.aspect=window.innerWidth/window.innerHeight,camera.updateProjectionMatrix(),renderer.setSize(window.innerWidth,window.innerHeight)}class AstronomicalObject{constructor(t,i,o,s,a){this.trailPoints=[],this.line=!1,Object.defineProperty(this,"radius",{value:i*constants.scale,writable:!1}),Object.defineProperty(this,"name",{value:t,writable:!1}),this.position=new e.Vector3(0,0,0),this.velocity=new e.Vector3(0,0,0),this.acceleration=new e.Vector3(0,0,0),this.mass=o||1,s&&(this.position=new e.Vector3(...s)),a&&(this.velocity=new e.Vector3(...a));let n=new e.SphereGeometry(this.radius,32,32),r=new e.MeshPhongMaterial({color:16777215});return this.body=new e.Mesh(n,r),this.color={set:e=>{this.body.material.color.set(e)}},space[this.name.toLowerCase()]=this,bodies.push(this),scene.add(this.body),this}}let space={},bodies=[],elapseSpeed=0,dt=0,oldTime=0,simulationTime=9466848e5,objectTextVector=new e.Vector3,constants={simSpeed:0,gravity:66743e-15,scale:3e-10},tick=0,timePerStep=1,stepsPerTick=1,scene=new e.Scene,camera=new e.PerspectiveCamera(75,window.innerWidth/window.innerHeight,.01,1e4);camera.target=void 0,camera.position.z=200;let renderer=new e.WebGLRenderer;renderer.setSize(window.innerWidth,window.innerHeight),renderer.shadowMap.enabled=!0,document.body.appendChild(renderer.domElement);let textCanvas=document.createElement("canvas");textCanvas.id="text",document.body.appendChild(textCanvas);let txtctx=textCanvas.getContext("2d");function createPathStrings(e){let t=["right","left","top","bottom","front","back"].map(e=>"https://tristancopley.github.io/dnhs-blog/images/skybox/"+e+".png");return t}window.addEventListener("resize",onWindowResize,!1);let skyboxImage="space";function createMaterialArray(t){let i=createPathStrings(t),o=i.map(t=>{let i=new e.TextureLoader().load(t);return new e.MeshBasicMaterial({map:i,side:e.BackSide})});return o}let materialArray=createMaterialArray(skyboxImage),skyboxGeo=new e.BoxGeometry(5e14*constants.scale,5e14*constants.scale,5e14*constants.scale),skybox=new e.Mesh(skyboxGeo,materialArray);scene.add(skybox);let pointLight=new e.PointLight(16777215,3);pointLight.position.x=0,pointLight.position.y=0,pointLight.position.z=0,pointLight.castShadows=!0,scene.add(pointLight);let aLight=new e.AmbientLight(16777215,.8);scene.add(aLight);let controls=new t(camera,renderer.domElement);function end(){let e=gui.addFolder("Time");e.add(constants,"simSpeed",0,950),e.open(),camera.target=Object.keys(space)[0].toString();let t=gui.addFolder("Camera");t.add(camera,"target",Object.getOwnPropertyNames(space)),t.open()}function animate(){if(0===oldTime)return oldTime=Date.now(),requestAnimationFrame(animate);dt=Date.now()-oldTime||1,oldTime=Date.now(),tick++,simulationTime+=(stepsPerTick=37e3)*(timePerStep=500*(constants.simSpeed/1e5*1.00015**(86400*constants.simSpeed/1e3)))*1e3/(1e3/(dt||1)),timePerStep/=1e3/(dt||1),tick%10==1&&(elapseSpeed=stepsPerTick*timePerStep*1e3/(1e3/dt));for(let t=0;t<stepsPerTick;t++)if(0!==stepsPerTick){for(let i=0;i<bodies.length;i++)for(let o=i+1;o<bodies.length;o++){let s=bodies[i].position.x-bodies[o].position.x,a=bodies[i].position.y-bodies[o].position.y,n=bodies[i].position.z-bodies[o].position.z,r=Math.sqrt(s**2+a**2+n**2)||1,$=constants.gravity*(bodies[i].mass*bodies[o].mass)/r**2;bodies[i].acceleration.x-=s/r*$/bodies[i].mass,bodies[i].acceleration.y-=a/r*$/bodies[i].mass,bodies[i].acceleration.z-=n/r*$/bodies[i].mass,bodies[o].acceleration.x+=s/r*$/bodies[o].mass,bodies[o].acceleration.y+=a/r*$/bodies[o].mass,bodies[o].acceleration.z+=n/r*$/bodies[o].mass}for(let c=0;c<bodies.length;c++)bodies[c].velocity.x+=bodies[c].acceleration.x*timePerStep,bodies[c].velocity.y+=bodies[c].acceleration.y*timePerStep,bodies[c].velocity.z+=bodies[c].acceleration.z*timePerStep,bodies[c].position.x+=bodies[c].velocity.x*timePerStep,bodies[c].position.y+=bodies[c].velocity.y*timePerStep,bodies[c].position.z+=bodies[c].velocity.z*timePerStep,bodies[c].acceleration=new e.Vector3(0,0,0);if(t%200==0)for(let d=0;d<bodies.length;d++)bodies[d].body.position.x=bodies[d].position.x*constants.scale,bodies[d].body.position.y=bodies[d].position.y*constants.scale,bodies[d].body.position.z=bodies[d].position.z*constants.scale,bodies[d].trailPoints.push(new e.Vector3(bodies[d].body.position.x,bodies[d].body.position.y,bodies[d].body.position.z)),bodies[d].trailPoints.length>1e4&&bodies[d].trailPoints.shift()}for(let l=0;l<bodies.length;l++)if(stepsPerTick>0){"object"==typeof bodies[l].line&&scene.remove(bodies[l].line);let p=new e.Line(new e.BufferGeometry().setFromPoints(bodies[l].trailPoints),new e.LineBasicMaterial({color:16711935}));p.material.color.set(bodies[l].body.material.color),bodies[l].line=p,scene.add(p)}camera.position.x-=controls.target.x-space[camera.target].body.position.x,camera.position.y-=controls.target.y-space[camera.target].body.position.y,camera.position.z-=controls.target.z-space[camera.target].body.position.z,pointLight.position.x=space[camera.target].body.position.x,pointLight.position.y=space[camera.target].body.position.y,pointLight.position.z=space[camera.target].body.position.z,controls.target=new e.Vector3(space[camera.target].body.position.x,space[camera.target].body.position.y,space[camera.target].body.position.z),controls.update(),skybox.position.x=camera.position.x,skybox.position.y=camera.position.y,skybox.position.z=camera.position.z,textCanvas.width=renderer.domElement.width,textCanvas.height=renderer.domElement.height,txtctx.font="12px Courier New",txtctx.fillStyle="white";for(let m=0;m<bodies.length;m++)objectTextVector.set(bodies[m].body.position.x+bodies[m].radius,bodies[m].body.position.y+bodies[m].radius,bodies[m].body.position.z+bodies[m].radius),objectTextVector.project(camera),objectTextVector.x=Math.round((objectTextVector.x+1)*textCanvas.width/2),objectTextVector.y=Math.round((-objectTextVector.y+1)*textCanvas.height/2),objectTextVector.z<1&&txtctx.fillText(bodies[m].name,objectTextVector.x,objectTextVector.y);txtctx.font="20px Courier New",txtctx.fillStyle="white",txtctx.fillText(863e13>simulationTime?new Date(simulationTime):`Around ${new Intl.NumberFormat("en",{notation:"compact",minimumSignificantDigits:3,maximumSignificantDigits:3}).format(simulationTime/315576e5+30)} years into the future. . .`,20,20);let b="Error",y=1;elapseSpeed<1?b="false":elapseSpeed>=1&&elapseSpeed<1.05?b="second":elapseSpeed<60?b="seconds":elapseSpeed>=60&&elapseSpeed<63?(b="minute",y=60):elapseSpeed<3600?(b="minutes",y=60):elapseSpeed>=3600&&elapseSpeed<3780?(b="hour",y=3600):elapseSpeed<86500?(b="hours",y=3600):elapseSpeed>=86500&&elapseSpeed<90825?(b="day",y=86500):elapseSpeed<31572500?(b="days",y=86500):elapseSpeed>=31572500&&elapseSpeed<33151125?(b="year",y=31572500):elapseSpeed>31572500&&(b="years",y=31572500),"false"!=b?txtctx.fillText(new Intl.NumberFormat("en",{notation:"compact",minimumSignificantDigits:1,maximumSignificantDigits:3}).format(elapseSpeed/y)+" "+b+" per second",20,45):txtctx.fillText("No time is elapsing.",20,45),constants.simSpeed>600&&(txtctx.fillStyle="yellow",txtctx.fillText("Warning: Simulation may not stable at this speed",20,70));let x=distance(camera.position,space[camera.target].body.position);skybox.scale.set(x/200,x/200,x/200),camera.far=1e3*x,camera.near=.001*x,camera.updateProjectionMatrix(),renderer.render(scene,camera),requestAnimationFrame(animate)}animate();
      
      // constants = {

      //   simSpeed:0,
      //   gravity:66743e-15,
      //   scale:3e-10

      // }

      // gui.destroy();

      // You can edit the values here and remove planets and such
      const sun = new AstronomicalObject('Sun', 6.95700e8, 1.989e30, [0, 0, 0], [0, 0, 0]);
      sun.color.set('yellow');
  
      const mercury = new AstronomicalObject('Mercury', 2440500, 3.3010e23, [68426000000, 0, 0], [0, 47360, 0]);
      mercury.color.set('brown');
      mercury.position = rotateVector(mercury.position, 0, 0, 245.5 * Math.PI / 180);
      mercury.velocity = rotateVector(mercury.velocity, 0, 7.005 * Math.PI / 180, 245.5 * Math.PI / 180);
  
      const venus = new AstronomicalObject('Venus', 6051800, 4.8673e24, [107480000000, 0, 0], [0, 35020, 0]);
      venus.color.set('orange');
      venus.position = rotateVector(venus.position, 0, 0, 182 * Math.PI / 180);
      venus.velocity = rotateVector(venus.velocity, 0, 3.3947 * Math.PI / 180, 182 * Math.PI / 180);
      
      const earth = new AstronomicalObject('Earth', 6371000, 5.972e24, [151000000000, 0, 0], [0, 2.978589e4, 0]); //m/s  
      earth.color.set('green');
      earth.position = rotateVector(earth.position, 0, 0, 109 * Math.PI / 180);
      earth.velocity = rotateVector(earth.velocity, 0, 0, 109 * Math.PI / 180);
  
      // Uncomment to add the Earth's moon
      // let moon = new AstronomicalObject(1737400, 7.347e22, [151000000000 + 384399000, 0, 0], [0, 29800, 1022]);
      // moon.color.set('white');
  
      const mars = new AstronomicalObject('Mars', 3389500, 6.39e23, [213140000000, 0, 0], [0, 24070, 0]);
      mars.color.set('red');
      mars.position = rotateVector(mars.position, 0, 0, -1 * Math.PI / 180);
      mars.velocity = rotateVector(mars.velocity, 0, 1.851 * Math.PI / 180, -1 * Math.PI / 180);
  
      const jupiter = new AstronomicalObject('Jupiter', 66854000, 1.89813e27, [741690000000, 0, 0], [0, 13060, 0]);
      jupiter.color.set('pink');
      jupiter.position = rotateVector(jupiter.position, 0, 0, 40 * Math.PI / 180);
      jupiter.velocity = rotateVector(jupiter.velocity, 0, 1.305 * Math.PI / 180, 40 * Math.PI / 180);
  
      const saturn = new AstronomicalObject('Saturn', 60268000, 5.6832e26, [1.4737e12, 0, 0], [0, 9680, 0]);
      saturn.color.set('purple');
      saturn.position = rotateVector(saturn.position, 0, 0, 50 * Math.PI / 180);
      saturn.velocity = rotateVector(saturn.velocity, 0, 2.484 * Math.PI / 180, 50 * Math.PI / 180);
  
      const uranus = new AstronomicalObject('Uranus', 25559000, 8.6811e25, [2.9444e+12, 0, 0], [0, 6800, 0]);
      uranus.color.set('blue');
      uranus.position = rotateVector(uranus.position, 0, 0, 325 * Math.PI / 180);
      uranus.velocity = rotateVector(uranus.velocity, 0, 0.770 * Math.PI / 180, 325 * Math.PI / 180);
  
      const neptune = new AstronomicalObject('Neptune', 24341000, 1.02409e26, [4.558857e12, 0, 0], [0, 5430, 0]);
      neptune.color.set('magenta');
      neptune.position = rotateVector(neptune.position, 0, 0, 315 * Math.PI / 180);
      neptune.velocity = rotateVector(neptune.velocity, 0, 1.769 * Math.PI / 180, 315 * Math.PI / 180);
  
      const pluto = new AstronomicalObject('Pluto', 1188000, 1.02409e26, [7.304326e12, 0, 0], [0, 3710, 0]);
      pluto.color.set('white');
      pluto.position = rotateVector(pluto.position, 0, 0, 270 * Math.PI / 180);
      pluto.velocity = rotateVector(pluto.velocity, 0, 17.14 * Math.PI / 180, 270 * Math.PI / 180);

      end();
    </script>