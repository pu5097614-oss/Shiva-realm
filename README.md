<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Shiva's Realm</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;}
body{overflow:hidden;background:#000;font-family:'Courier New',monospace;cursor:crosshair;}
#canvas{display:block;width:100vw;height:100vh;}

/* ── LOADING ── */
#loading{position:fixed;inset:0;z-index:200;display:flex;flex-direction:column;align-items:center;justify-content:center;overflow:hidden;transition:opacity 1.4s;}
#loading::before{content:'';position:absolute;inset:0;background:linear-gradient(160deg,#0a0018,#0d1a3a 40%,#1a0a2e 70%,#000510);z-index:0;}
#ls{position:absolute;inset:0;z-index:1;pointer-events:none;}
#lm{position:absolute;bottom:0;left:0;right:0;height:45%;z-index:2;pointer-events:none;}
.lglow{position:absolute;z-index:2;width:480px;height:480px;border-radius:50%;background:radial-gradient(circle,rgba(255,130,0,.2),rgba(200,50,0,.07) 55%,transparent 75%);top:50%;left:50%;transform:translate(-50%,-56%);animation:pg 3s ease-in-out infinite;}
@keyframes pg{0%,100%{transform:translate(-50%,-56%) scale(1);opacity:.7}50%{transform:translate(-50%,-56%) scale(1.14);opacity:1}}
.lc{position:relative;z-index:3;text-align:center;}
.ltag{color:rgba(255,175,70,.6);font-size:11px;letter-spacing:6px;text-transform:uppercase;margin-bottom:16px;animation:fu 1s .2s both;}
.ltit{font-size:clamp(32px,9vw,72px);font-weight:100;letter-spacing:12px;color:#fff;text-shadow:0 0 55px rgba(255,130,0,.7),0 0 110px rgba(255,70,0,.3);margin-bottom:5px;animation:fu 1s .4s both;}
.lom{font-size:clamp(22px,5vw,46px);color:rgba(255,155,45,.85);letter-spacing:4px;margin-bottom:5px;animation:fu 1s .5s both;}
.lsub{font-size:10px;letter-spacing:5px;color:rgba(255,255,255,.2);margin-bottom:44px;animation:fu 1s .6s both;}
@keyframes fu{from{opacity:0;transform:translateY(16px)}to{opacity:1;transform:translateY(0)}}
.lpw{width:min(290px,68vw);margin:0 auto 10px;animation:fu 1s .8s both;}
.lpl{display:flex;justify-content:space-between;color:rgba(255,170,70,.4);font-size:9px;letter-spacing:2px;margin-bottom:6px;}
.lbg{width:100%;height:3px;background:rgba(255,255,255,.05);border-radius:2px;overflow:hidden;border:1px solid rgba(255,140,45,.12);}
.lbar{height:100%;background:linear-gradient(90deg,#ff6a00,#ffaa44,#ffdd88);border-radius:2px;width:0%;transition:width .3s;box-shadow:0 0 9px rgba(255,135,0,.9);}
.lst{color:rgba(255,155,55,.7);font-size:10px;letter-spacing:3px;min-height:16px;margin-bottom:32px;animation:fu 1s 1s both;}
#sbtn{display:none;padding:14px 46px;background:transparent;border:1px solid rgba(255,135,0,.5);color:rgba(255,215,140,.9);font-family:'Courier New',monospace;font-size:13px;letter-spacing:5px;cursor:pointer;position:relative;overflow:hidden;transition:all .3s;}
#sbtn::before{content:'';position:absolute;inset:0;background:linear-gradient(90deg,transparent,rgba(255,135,0,.14),transparent);transform:translateX(-100%);transition:transform .5s;}
#sbtn:hover{border-color:rgba(255,175,75,.9);box-shadow:0 0 26px rgba(255,115,0,.28);}
#sbtn:hover::before{transform:translateX(100%);}

/* ── HUD ── */
#ui{position:fixed;inset:0;pointer-events:none;z-index:10;}
#ch{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);width:14px;height:14px;}
#ch::before,#ch::after{content:'';position:absolute;background:rgba(255,255,255,.6);border-radius:2px;}
#ch::before{width:2px;height:100%;left:50%;transform:translateX(-50%);}
#ch::after{height:2px;width:100%;top:50%;transform:translateY(-50%);}
#hud{position:absolute;bottom:26px;left:50%;transform:translateX(-50%);display:flex;gap:16px;align-items:center;}
.hb{display:flex;flex-direction:column;align-items:center;gap:3px;}
.bl{color:rgba(255,255,255,.5);font-size:9px;letter-spacing:2px;text-transform:uppercase;}
.bt{width:110px;height:6px;background:rgba(255,255,255,.07);border-radius:3px;overflow:hidden;border:1px solid rgba(255,255,255,.1);}
.bf{height:100%;border-radius:3px;transition:width .2s;}
#hf{background:linear-gradient(90deg,#e63950,#ff7080);width:100%;}
#sf{background:linear-gradient(90deg,#2299ff,#66ccff);width:100%;}
#mm{position:absolute;top:18px;right:18px;width:135px;height:135px;border-radius:50%;background:rgba(0,0,0,.55);border:2px solid rgba(255,255,255,.17);overflow:hidden;}
#inf{position:absolute;top:18px;left:18px;color:rgba(255,255,255,.75);font-size:11px;line-height:1.9;text-shadow:1px 1px 3px rgba(0,0,0,.9);background:rgba(0,0,0,.35);padding:6px 10px;border-radius:6px;}
#inf span{color:#ffcc77;}
#dc{position:absolute;top:18px;left:50%;transform:translateX(-50%);color:rgba(255,255,255,.6);font-size:11px;letter-spacing:3px;text-transform:uppercase;background:rgba(0,0,0,.3);padding:4px 13px;border-radius:20px;}
#hint{position:absolute;top:54%;left:50%;transform:translate(-50%,-50%);color:rgba(255,255,255,.5);font-size:12px;letter-spacing:2px;background:rgba(0,0,0,.44);padding:9px 18px;border-radius:8px;transition:opacity 1.5s;pointer-events:none;}
#tlbl{position:absolute;display:none;background:rgba(0,0,0,.6);color:#ffcc55;padding:5px 13px;border-radius:6px;font-size:11px;letter-spacing:2px;top:40%;left:50%;transform:translateX(-50%);border:1px solid rgba(255,175,45,.3);}
#ch2{position:absolute;bottom:73px;right:18px;color:rgba(255,255,255,.27);font-size:10px;line-height:2.1;text-align:right;}
#ch2 b{color:rgba(255,255,255,.48);}

/* ── MOBILE ── */
#mob{display:none;position:fixed;inset:0;pointer-events:none;z-index:20;}
@media(max-width:900px),(pointer:coarse){
  #mob{display:block;}#ch2{display:none;}
  #inf{font-size:9px;padding:4px 7px;}
  #mm{width:88px;height:88px;top:9px;right:9px;}
  #hud{bottom:182px;}.bt{width:68px;}
  #dc{font-size:9px;top:9px;}
}
#dpad{position:absolute;left:13px;bottom:13px;width:158px;height:158px;pointer-events:all;}
.dp{position:absolute;width:50px;height:50px;background:rgba(255,255,255,.1);border:2px solid rgba(255,255,255,.28);border-radius:11px;color:#fff;font-size:20px;display:flex;align-items:center;justify-content:center;user-select:none;-webkit-user-select:none;cursor:pointer;transition:background .07s,transform .05s;box-shadow:0 2px 9px rgba(0,0,0,.5);}
.dp:active,.dp.on{background:rgba(255,255,255,.28);transform:scale(.9);}
#du{top:0;left:54px;}#dd{bottom:0;left:54px;}#dl{top:54px;left:0;}#dr{top:54px;right:0;}
#abtn{position:absolute;right:11px;bottom:11px;display:grid;grid-template-columns:1fr 1fr;gap:9px;pointer-events:all;}
.ab{width:55px;height:55px;border-radius:50%;color:#fff;font-size:9px;letter-spacing:1px;display:flex;align-items:center;justify-content:center;flex-direction:column;font-family:'Courier New',monospace;user-select:none;-webkit-user-select:none;cursor:pointer;transition:background .07s,transform .05s;box-shadow:0 2px 11px rgba(0,0,0,.5);border:2px solid;}
.ab:active,.ab.on{transform:scale(.87);}
#aj{background:rgba(75,150,255,.2);border-color:rgba(75,150,255,.7);}
#ar{background:rgba(255,145,28,.2);border-color:rgba(255,145,28,.7);}
#ac{background:rgba(75,215,115,.14);border-color:rgba(75,215,115,.6);}
.zb{width:38px;height:38px;border-radius:9px;background:rgba(255,255,255,.07);border:1px solid rgba(255,255,255,.2);color:rgba(255,255,255,.55);font-size:21px;line-height:36px;text-align:center;cursor:pointer;user-select:none;}
.zb:active{background:rgba(255,255,255,.2);}
#lz{position:absolute;right:0;top:0;width:50%;height:calc(100% - 195px);pointer-events:all;}
</style>
</head>
<body>

<!-- LOADING -->
<div id="loading">
  <canvas id="ls"></canvas>
  <svg id="lm" viewBox="0 0 1440 300" preserveAspectRatio="none">
    <path d="M0,300 L0,200 L120,100 L200,160 L320,60 L420,140 L520,40 L620,130 L720,20 L820,110 L920,50 L1020,140 L1120,70 L1220,150 L1320,90 L1440,160 L1440,300 Z" fill="rgba(28,9,0,.86)"/>
    <path d="M0,300 L0,240 L80,180 L160,220 L260,150 L380,210 L480,160 L580,200 L680,130 L780,190 L900,140 L1000,200 L1100,155 L1200,210 L1300,165 L1440,220 L1440,300 Z" fill="rgba(18,5,0,.91)"/>
    <path d="M0,300 L0,270 L200,255 L440,260 L680,250 L900,258 L1160,252 L1440,260 L1440,300 Z" fill="rgba(9,2,0,.96)"/>
  </svg>
  <div class="lglow"></div>
  <div class="lc">
    <div class="ltag">🕉 Three.js · Open World · 3D</div>
    <div class="ltit">SHIVA'S<br>REALM</div>
    <div class="lom">ॐ नमः शिवाय</div>
    <div class="lsub">EXPLORE · DISCOVER · REACH THE SACRED TEMPLE</div>
    <div class="lpw"><div class="lpl"><span>BUILDING WORLD</span><span id="lpct">0%</span></div><div class="lbg"><div class="lbar" id="lbar"></div></div></div>
    <div class="lst" id="lst">Awakening the realm...</div>
    <button id="sbtn" onclick="startGame()">🕉 &nbsp;ENTER THE REALM</button>
  </div>
</div>

<canvas id="canvas"></canvas>

<!-- HUD -->
<div id="ui">
  <div id="ch"></div>
  <div id="dc">Morning</div>
  <div id="inf">POS:<span id="pos"> 0,0,0</span><br>SPD:<span id="spd"> 0</span> km/h<br>ENV:<span id="env"> Forest</span></div>
  <div id="mm"><canvas id="mc" width="135" height="135"></canvas></div>
  <div id="hud">
    <div class="hb"><div class="bl">Health</div><div class="bt"><div class="bf" id="hf"></div></div></div>
    <div class="hb"><div class="bl">Stamina</div><div class="bt"><div class="bf" id="sf"></div></div></div>
  </div>
  <div id="ch2"><b>WASD</b> Move<br><b>Drag</b> Cam<br><b>Scroll</b> Zoom<br><b>SPACE</b> Jump<br><b>SHIFT</b> Sprint</div>
  <div id="hint">Drag to rotate camera · Scroll to zoom</div>
  <div id="tlbl">🕉 Lord Shiva's Temple</div>
</div>

<!-- MOBILE CONTROLS -->
<div id="mob">
  <div id="dpad">
    <div class="dp" id="du">▲</div><div class="dp" id="dd">▼</div>
    <div class="dp" id="dl">◀</div><div class="dp" id="dr">▶</div>
  </div>
  <div id="lz"></div>
  <div id="abtn">
    <div class="ab" id="aj">⬆<br>JUMP</div>
    <div class="ab" id="ar">⚡<br>RUN</div>
    <div class="ab" id="ac">⬇<br>DUCK</div>
    <div class="zb" id="zi">+</div>
    <div class="zb" id="zo">−</div>
  </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
/* ══ LOADING SCREEN ══ */
(function(){
  var c=document.getElementById('ls'),ctx=c.getContext('2d');
  c.width=innerWidth;c.height=innerHeight;
  var stars=[];for(var i=0;i<180;i++)stars.push({x:Math.random()*c.width,y:Math.random()*c.height,r:Math.random()*1.4+.2,a:Math.random(),da:(Math.random()-.5)*.007});
  var rf;function dSt(){ctx.clearRect(0,0,c.width,c.height);stars.forEach(function(s){s.a=Math.max(.05,Math.min(1,s.a+s.da));if(s.a<=.05||s.a>=1)s.da*=-1;ctx.beginPath();ctx.arc(s.x,s.y,s.r,0,Math.PI*2);ctx.fillStyle='rgba(255,220,175,'+s.a+')';ctx.fill();});rf=requestAnimationFrame(dSt);}dSt();
  window._ss=function(){cancelAnimationFrame(rf);};
  var steps=[{p:10,m:'Awakening the realm...'},{p:22,m:'Sculpting mountains...'},{p:36,m:'Planting 200 trees...'},{p:50,m:'Filling sacred lake...'},{p:63,m:'Summoning the devotee...'},{p:75,m:'Building Shiva Temple...'},{p:86,m:'Placing Shivalinga...'},{p:94,m:'Summoning animals...'},{p:100,m:'ॐ The realm is ready!'}];
  var bar=document.getElementById('lbar'),pct=document.getElementById('lpct'),st=document.getElementById('lst'),btn=document.getElementById('sbtn'),idx=0;
  function step(){if(idx>=steps.length)return;var s=steps[idx++];bar.style.width=s.p+'%';pct.textContent=s.p+'%';st.textContent=s.m;if(idx<steps.length)setTimeout(step,250+Math.random()*300);else setTimeout(function(){st.style.color='rgba(255,195,75,.9)';btn.style.display='inline-block';},400);}
  setTimeout(step,500);
})();

/* ══ GLOBALS ══ */
var scene,camera,renderer,clock;
var gameStarted=false,animFrame=0;
var player={x:0,y:0,z:0};
var velY=0,grounded=false;
var health=100,stamina=100;
var keys={};
var facing=Math.PI;
var camYaw=Math.PI,camPitch=.38,camDist=7;
var dragging=false,lmx=0,lmy=0;
var water,trees=[],clouds=[],animals=[];
var legT=0,lLeg,rLeg,lShoe,rShoe,lArm,rArm,pMesh;
var dayT=.3;
var TX=80,TZ=-120; /* temple world position */
var TBY=0;          /* terrain height at temple — set in init */

/* ══ TERRAIN ══ */
function noise(x,z){
  return Math.sin(x*.05)*Math.cos(z*.05)*15
        +Math.sin(x*.15+1.2)*Math.cos(z*.1)*8
        +Math.sin(x*.3)*Math.cos(z*.35+.7)*4
        +Math.sin(x*.6+2.1)*Math.cos(z*.5)*2;
}

/* ══ KEY INSIGHT: two separate height functions
   terrH  = raw terrain, used for placing world objects
   floorH = what the player stands on; inside temple zone
            it returns the platform top instead of terrain  ══ */
function terrH(x,z){return noise(x,z);}

/* Temple platform zones — player stands on these flat surfaces.
   NO multi-sampling inside these zones so the boundary is sharp
   and player can step in cleanly.                               */
function floorH(x,z){
  var dx=x-TX, dz=z-TZ;
  /* Innermost floor of sanctum — flat surface at step3 top */
  if(Math.abs(dx)<5.2 && Math.abs(dz)<5.2)
    return TBY+3.2;
  /* Step 2 */
  if(Math.abs(dx)<7.2 && Math.abs(dz)<7.2)
    return TBY+2.0;
  /* Step 1 */
  if(Math.abs(dx)<9.5 && Math.abs(dz)<9.5)
    return TBY+1.2;
  /* Outer platform (boundary wall base) */
  if(Math.abs(dx)<11.5 && Math.abs(dz)<11.5)
    return TBY+0.4;
  /* Approach path / stone steps — gentle ramp (z positive = south, toward player start) */
  if(Math.abs(dx)<3.5 && dz>5.2 && dz<20)
    return TBY+0.4*(1-(dz-5.2)/14.8);
  return terrH(x,z);
}

/* For terrain outside temple, multi-sample to prevent sinking into hills.
   Inside temple zone use single point so boundary works correctly.        */
function groundH(x,z){
  var dx=x-TX, dz=z-TZ;
  /* Inside temple courtyard — single point, no spreading */
  if(Math.abs(dx)<13 && Math.abs(dz)<13)
    return floorH(x,z);
  /* Outside — 5-point max sample for slope safety */
  var r=.45;
  return Math.max(floorH(x,z), floorH(x+r,z), floorH(x-r,z), floorH(x,z+r), floorH(x,z-r));
}

/* ══ INIT ══ */
function startGame(){
  var el=document.getElementById('loading');
  el.style.opacity='0';
  if(window._ss)window._ss();
  setTimeout(function(){el.style.display='none';init();},1400);
}
function init(){
  gameStarted=true;
  TBY=terrH(TX,TZ);
  scene=new THREE.Scene();
  clock=new THREE.Clock();
  camera=new THREE.PerspectiveCamera(65,innerWidth/innerHeight,.1,2000);
  renderer=new THREE.WebGLRenderer({canvas:document.getElementById('canvas'),antialias:true});
  renderer.setSize(innerWidth,innerHeight);
  renderer.setPixelRatio(Math.min(devicePixelRatio,2));
  renderer.shadowMap.enabled=true;
  renderer.shadowMap.type=THREE.PCFSoftShadowMap;
  renderer.toneMapping=THREE.ACESFilmicToneMapping;
  renderer.toneMappingExposure=1.1;
  buildWorld();
  buildTemple();
  buildAnimals();
  buildPlayer();
  buildLights();
  buildInput();
  setTimeout(function(){var h=document.getElementById('hint');if(h)h.style.opacity='0';},5000);
  animate();
}

/* ══ WORLD ══ */
function buildWorld(){
  /* Terrain mesh */
  var tg=new THREE.PlaneGeometry(800,800,140,140);
  tg.rotateX(-Math.PI/2);
  var pos=tg.attributes.position;
  for(var i=0;i<pos.count;i++)pos.setY(i,terrH(pos.getX(i),pos.getZ(i)));
  tg.computeVertexNormals();
  scene.add(new THREE.Mesh(tg,new THREE.MeshStandardMaterial({color:0x4e8044,roughness:.95})));
  /* Lake */
  var wg=new THREE.PlaneGeometry(80,80);wg.rotateX(-Math.PI/2);
  water=new THREE.Mesh(wg,new THREE.MeshStandardMaterial({color:0x0066bb,transparent:true,opacity:.78,roughness:.1,metalness:.25}));
  water.position.set(80,-2,80);scene.add(water);
  var sg=new THREE.PlaneGeometry(110,110);sg.rotateX(-Math.PI/2);
  var shore=new THREE.Mesh(sg,new THREE.MeshStandardMaterial({color:0xd4b483,roughness:1}));
  shore.position.set(80,-1.4,80);scene.add(shore);
  /* Distant mountains */
  for(var i=0;i<16;i++){
    var a=(i/16)*Math.PI*2,r=330+Math.random()*70,h=50+Math.random()*80;
    var mm=new THREE.Mesh(new THREE.ConeGeometry(24+Math.random()*20,h,8),new THREE.MeshStandardMaterial({color:0x8899aa,roughness:1}));
    mm.position.set(Math.cos(a)*r,h/2-5,Math.sin(a)*r);scene.add(mm);
    var sm=new THREE.Mesh(new THREE.ConeGeometry(10,h*.28,8),new THREE.MeshStandardMaterial({color:0xeeeeff,roughness:1}));
    sm.position.set(Math.cos(a)*r,h/2-5+h*.36,Math.sin(a)*r);scene.add(sm);
  }
  /* Trees */
  var trM=new THREE.MeshStandardMaterial({color:0x5c3a1e,roughness:1});
  var lfM=[new THREE.MeshStandardMaterial({color:0x2d5a27,roughness:.9}),new THREE.MeshStandardMaterial({color:0x3d7a35,roughness:.9}),new THREE.MeshStandardMaterial({color:0x1a4a18,roughness:.9})];
  for(var i=0;i<200;i++){
    var tx=(Math.random()-.5)*400,tz=(Math.random()-.5)*400;
    if(Math.abs(tx-80)<55&&Math.abs(tz-80)<55)continue;
    if(Math.abs(tx-TX)<40&&Math.abs(tz-TZ)<40)continue;
    if(Math.abs(tx)<12&&Math.abs(tz)<12)continue;
    var ty=terrH(tx,tz),th=4+Math.random()*6;
    var trk=new THREE.Mesh(new THREE.CylinderGeometry(.14,.28,th,6),trM);
    trk.position.set(tx,ty+th/2,tz);trk.castShadow=true;scene.add(trk);
    var fol=new THREE.Mesh(new THREE.ConeGeometry(2+Math.random()*1.5,th*.85,8),lfM[i%3]);
    fol.position.set(tx,ty+th+th*.28,tz);fol.castShadow=true;scene.add(fol);
    trees.push({x:tx,z:tz});
  }
  /* Rocks */
  var rM=new THREE.MeshStandardMaterial({color:0x778899,roughness:.9,metalness:.1});
  for(var i=0;i<80;i++){
    var rx=(Math.random()-.5)*380,rz=(Math.random()-.5)*380,rs=.5+Math.random()*2;
    var rk=new THREE.Mesh(new THREE.DodecahedronGeometry(rs,0),rM);
    rk.position.set(rx,terrH(rx,rz)+rs*.4,rz);
    rk.rotation.set(Math.random()*6,Math.random()*6,Math.random()*6);
    rk.castShadow=true;rk.receiveShadow=true;scene.add(rk);
  }
  /* Grass */
  var gM=new THREE.MeshStandardMaterial({color:0x5a9e4a,side:THREE.DoubleSide,roughness:1});
  for(var i=0;i<500;i++){
    var gx=(Math.random()-.5)*200,gz=(Math.random()-.5)*200,gy=terrH(gx,gz);
    var g1=new THREE.Mesh(new THREE.PlaneGeometry(.3,.9),gM);
    g1.position.set(gx,gy+.45,gz);g1.rotation.y=Math.random()*Math.PI;scene.add(g1);
    var g2=g1.clone();g2.rotation.y+=Math.PI/2;scene.add(g2);
  }
  /* Clouds */
  for(var i=0;i<25;i++){
    var cg=new THREE.Group();
    var cM=new THREE.MeshStandardMaterial({color:0xffffff,transparent:true,opacity:.85,roughness:1});
    for(var j=0;j<4+Math.floor(Math.random()*5);j++){
      var cc=new THREE.Mesh(new THREE.SphereGeometry(4+Math.random()*5,7,7),cM);
      cc.position.set((Math.random()-.5)*15,(Math.random()-.5)*4,(Math.random()-.5)*10);cg.add(cc);
    }
    cg.position.set((Math.random()-.5)*600,62+Math.random()*38,(Math.random()-.5)*600);
    cg._s=.02+Math.random()*.03;scene.add(cg);clouds.push(cg);
  }
}

/* ══ TEMPLE ══ */
function buildTemple(){
  var BY=TBY,B=TX,Z=TZ;
  var sto=new THREE.MeshStandardMaterial({color:0xc8a882,roughness:.85,metalness:.05});
  var drk=new THREE.MeshStandardMaterial({color:0x8a6a50,roughness:.9});
  var org=new THREE.MeshStandardMaterial({color:0xff8c00,roughness:.6,metalness:.2});
  var gld=new THREE.MeshStandardMaterial({color:0xffd700,roughness:.3,metalness:.8,emissive:new THREE.Color(0xffaa00),emissiveIntensity:.3});
  var blk=new THREE.MeshStandardMaterial({color:0x111111,roughness:.7,metalness:.3});
  var mrb=new THREE.MeshStandardMaterial({color:0xf0e8d8,roughness:.3,metalness:.1});
  var flg=new THREE.MeshStandardMaterial({color:0xff4400,roughness:.8,side:THREE.DoubleSide});
  var brs=new THREE.MeshStandardMaterial({color:0xb8860b,roughness:.4,metalness:.7});
  var bel=new THREE.MeshStandardMaterial({color:0x228B22,roughness:.8});
  var fir=new THREE.MeshBasicMaterial({color:0xff8800});

  function add(geo,mat,x,y,z,rx,ry,rz){
    var m=new THREE.Mesh(geo,mat);
    m.position.set(B+x,BY+y,Z+z);
    if(rx)m.rotation.x=rx;if(ry)m.rotation.y=ry;if(rz)m.rotation.z=rz;
    m.castShadow=true;m.receiveShadow=true;scene.add(m);return m;
  }

  /* PLATFORM — heights must match floorH() above exactly:
     step1 top = BY+1.2  (thickness 1.2, placed at BY+0.6)
     step2 top = BY+2.0  (thickness 0.8, placed at BY+1.6)
     step3 top = BY+3.2  (thickness 1.2, placed at BY+2.6)  */
  add(
