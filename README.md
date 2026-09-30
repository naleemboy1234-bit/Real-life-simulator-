<!DOCTYPE html>
<html lang="en"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<meta name="theme-color" content="#0e1014">
<title>Real Life Simulator</title>
<style>
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent;-webkit-user-select:none;user-select:none;touch-action:none}
html,body{margin:0;height:100%;overflow:hidden;background:#0e1014;color:#fff;font-family:"Trebuchet MS",system-ui,sans-serif}
canvas#g{display:block;background:#0e1014}
.h{position:fixed;pointer-events:none;z-index:2}
:root{--top:calc(env(safe-area-inset-top,0px) + 6px)}
#money{top:var(--top);left:50%;transform:translateX(-50%);font-weight:800;font-size:clamp(18px,5vh,24px);line-height:1;color:#8cf59a;background:rgba(8,12,10,.6);padding:7px 16px;border-radius:8px;border-left:4px solid #8cf59a}
#lv{top:calc(var(--top) + 40px);left:50%;transform:translateX(-50%);width:130px;text-align:center;font-size:12px;font-weight:700}
.bar{height:6px;background:rgba(0,0,0,.6);border-radius:3px;overflow:hidden;margin-top:3px}
.bar i{display:block;height:100%;width:100%}
#xb{background:#74c0fc}#sb{background:#f2c230}#hb{background:#ff9f43}
#clock{top:var(--top);left:calc(10px + env(safe-area-inset-left,0px));font-size:13px;background:rgba(0,0,0,.5);padding:5px 9px;border-radius:6px}
#bars{top:calc(var(--top) + 30px);left:calc(10px + env(safe-area-inset-left,0px));width:110px;font-size:10px;color:#ccd}
#wt{top:calc(var(--top) + 84px);left:calc(10px + env(safe-area-inset-left,0px));font-weight:800;color:#ff5c5c;background:rgba(0,0,0,.6);padding:4px 10px;border-radius:6px;display:none;animation:bl .6s infinite alternate}
@keyframes bl{to{opacity:.35}}
#siren{inset:0;display:none;animation:sr .5s infinite alternate}
@keyframes sr{from{box-shadow:inset 0 0 90px 10px rgba(255,0,0,.45)}to{box-shadow:inset 0 0 90px 10px rgba(0,80,255,.45)}}
#map{top:var(--top);right:calc(10px + env(safe-area-inset-right,0px));width:96px;height:96px;border-radius:50%;border:2px solid rgba(255,255,255,.6)}
#job{top:calc(var(--top) + 64px);left:50%;transform:translateX(-50%);font-size:13px;background:rgba(0,0,0,.6);padding:5px 12px;border-radius:6px;white-space:nowrap;display:none}
#arr{display:inline-block;color:#f2c230}
#hint{bottom:5vh;left:50%;transform:translateX(-50%);font-size:13px;background:rgba(0,0,0,.65);padding:6px 12px;border-radius:6px;opacity:0;transition:opacity .2s;white-space:nowrap}
#toast{top:30%;left:50%;transform:translateX(-50%);font-size:clamp(15px,4.5vh,20px);font-weight:700;text-shadow:0 2px 4px #000;opacity:0;transition:opacity .3s;text-align:center;max-width:80%}
#joy{position:fixed;z-index:3;left:calc(3vw + env(safe-area-inset-left,0px));bottom:6vh;width:min(34vh,150px);height:min(34vh,150px);border-radius:50%;background:rgba(255,255,255,.08);border:2px solid rgba(255,255,255,.3)}
#knob{position:absolute;left:30%;top:30%;width:40%;height:40%;border-radius:50%;background:rgba(255,255,255,.4);border:2px solid rgba(255,255,255,.6)}
#pad{position:fixed;z-index:3;right:calc(3vw + env(safe-area-inset-right,0px));bottom:6vh;width:0;height:0;--d:min(17vh,72px)}
.b{position:absolute;width:var(--d);height:var(--d);border-radius:50%;background:rgba(14,16,20,.5);border:2px solid rgba(255,255,255,.45);display:flex;align-items:center;justify-content:center;font-weight:700;font-size:12px}
.b.on{background:rgba(242,194,48,.5);border-color:#f2c230}
#bJ{right:0;bottom:0}#bR{right:calc(var(--d)*1.2);bottom:0}
#bA{right:0;bottom:calc(var(--d)*1.2);border-color:#5ad1ff}#bK{right:calc(var(--d)*1.2);bottom:calc(var(--d)*1.2);border-color:#8cf59a}
#menu{position:fixed;z-index:8;left:50%;top:50%;transform:translate(-50%,-50%);width:min(560px,92vw);max-height:84vh;overflow:auto;background:rgba(16,18,22,.97);border:1px solid #3a3f4a;border-radius:10px;padding:14px;display:none}
#menu,#menu *{touch-action:pan-y}
#menu h2{margin:0 0 8px;font-size:20px;color:#f2c230}
.row{display:flex;justify-content:space-between;align-items:center;gap:10px;padding:9px 0;border-top:1px solid #2a2e37}
.row small{display:block;color:#aab;margin-top:2px}
#menu button{font:inherit;font-weight:700;padding:8px 14px;border:0;border-radius:6px;background:#f2c230;color:#15171b;min-width:80px}
#menu button:disabled{background:#3a3f4a;color:#889}
#menu .close{width:100%;margin-top:10px;background:#2b303a;color:#fff}
#start{position:fixed;inset:0;z-index:9;display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center;padding:16px;background:rgba(14,16,20,.82)}
#start h1{margin:0;font:400 clamp(38px,13vh,84px)/.92 Impact,Haettenschweiler,"Arial Narrow Bold",sans-serif;letter-spacing:.04em;color:#f2c230;text-shadow:0 4px 0 #000}
#start p{margin:10px 0 0;max-width:520px;line-height:1.45;color:#d8d8d8;font-size:14px}
#go{margin-top:18px;font:inherit;font-size:19px;font-weight:800;padding:12px 44px;border:0;border-radius:6px;background:#f2c230;color:#15171b}
#rot{display:none;position:fixed;inset:0;z-index:20;background:#0e1014;align-items:center;justify-content:center;text-align:center;padding:30px;font-size:20px;line-height:1.5}
@media (orientation:portrait){#rot{display:flex}}

:root{--top:calc(env(safe-area-inset-top,0px) + 4px)}
@supports(height:100dvh){html,body{height:100dvh}}
#pad{--d:min(15vh,64px)}
#bF{right:calc(var(--d)*2.4);bottom:0;border-color:#ff6b6b}
#bD{right:calc(var(--d)*2.4);bottom:calc(var(--d)*1.2);border-color:#f2c230}
#bars{top:calc(var(--top) + 28px)}#wt{top:calc(var(--top) + 104px)}
#mb{position:fixed;z-index:3;top:var(--top);right:calc(112px + env(safe-area-inset-right,0px));width:36px;height:36px;border-radius:8px;background:rgba(14,16,20,.55);border:1px solid rgba(255,255,255,.4);display:flex;align-items:center;justify-content:center;font-size:18px}
#menu{z-index:12}
#start{justify-content:flex-start;overflow:auto;gap:8px}
#start>:first-child{margin-top:auto}#start>:last-child{margin-bottom:auto}
#start h1{font-size:clamp(24px,8vh,50px);line-height:1;margin:0}
#start p{margin:0;font-size:13px}#start p.s{font-size:11px;color:#999}
#nm{font:inherit;font-size:17px;text-align:center;padding:10px 14px;border-radius:6px;border:2px solid #f2c230;background:#181b21;color:#fff;width:min(300px,80vw);user-select:text;-webkit-user-select:text;touch-action:auto}
.sr{display:flex;gap:10px;justify-content:center}#go{margin-top:0;padding:10px 44px}
.sm{font:inherit;font-weight:700;padding:8px 20px;border:1px solid #666;border-radius:6px;background:#22262e;color:#fff}
</style></head><body>
<canvas id="g"></canvas>
<div id="siren" class="h"></div>
<div id="money" class="h">$100</div>
<div id="lv" class="h"><span id="lvt">LV 1</span><div class="bar"><i id="xb"></i></div></div>
<div id="clock" class="h"></div>
<div id="bars" class="h">Health<div class="bar"><i id="pb" style="background:#e5484d"></i></div>Stamina<div class="bar"><i id="sb"></i></div>Hunger<div class="bar"><i id="hb"></i></div></div>
<div id="wt" class="h">WANTED</div>
<canvas id="map" class="h" width="192" height="192"></canvas><div id="mb">&#9776;</div>
<div id="job" class="h"><span id="arr">&#9650;</span> <span id="jt"></span></div>
<div id="hint" class="h"></div>
<div id="toast" class="h"></div>
<div id="joy"><div id="knob"></div></div>
<div id="pad"><div class="b" id="bJ">Jump</div><div class="b" id="bR">Run</div><div class="b" id="bA">Use</div><div class="b" id="bK">Ask</div><div class="b" id="bF">Fight</div><div class="b" id="bD">Drive</div></div>
<div id="menu"></div>
<div id="start"><h1>Real Life Simulator</h1>
<p>Work jobs, level up, buy clothes and vehicles, fight, drive and survive in a living 3D city.</p>
<input id="nm" maxlength="14" placeholder="Enter your character name" autocomplete="off">
<div class="sr"><button id="go">Play</button></div>
<div class="sr"><button id="sh" class="sm">Share</button><button id="dn" class="sm">Donate</button></div>
<p class="s">Stick: move. Drag: look. Keys: WASD, Shift run, Space jump, E use, F ask, Q fight, R drive</p></div>
<div id="rot">Rotate your phone to landscape to play</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
const T=THREE,$=i=>document.getElementById(i);
const R=(a,b)=>a+Math.random()*(b-a),RI=(a,b)=>Math.floor(R(a,b+1)),PK=a=>a[RI(0,a.length-1)];
const el=(t,c,x)=>{const e=document.createElement(t);if(c)e.className=c;if(x!==undefined)e.textContent=x;return e};
/* ---------- data ---------- */
const SITES=[
 {id:'hotel',n:'Grand Hotel',i:0,j:0,h:48,s:0,col:'#ff7ad9',jobs:['hotel']},
 {id:'depot',n:'Delivery Depot',i:-1,j:0,h:10,s:3,col:'#ffb84d',jobs:['deliv']},
 {id:'clean',n:'CleanPro Services',i:0,j:-1,h:20,s:4,col:'#5ad1ff',jobs:['clean']},
 {id:'taxi',n:'City Taxi Stand',i:1,j:0,h:9,s:1,col:'#f2c230',jobs:['auto','cab']},
 {id:'guard',n:'Secure Guard Agency',i:-1,j:-1,h:14,s:1,col:'#9aa7ff',jobs:['guard']},
 {id:'shop',n:'Sharma Kirana Store',i:1,j:-1,h:8,s:2,col:'#ff9f43',jobs:['shop']},
 {id:'dhaba',n:'Highway Dhaba',i:-1,j:1,h:8,s:2,col:'#ff6b6b',jobs:['chai'],shop:'food'},
 {id:'cloth',n:'Style Clothing Store',i:0,j:1,h:12,s:0,col:'#c77dff',shop:'cloth'},
 {id:'build',n:'BuildRight Constructions',i:1,j:1,h:14,s:3,col:'#ffd166',jobs:['build']},
 {id:'office',n:'TechPark Office',i:2,j:0,h:40,s:1,col:'#74c0fc',jobs:['clerk','teach']},
 {id:'bike',n:'Bike Showroom',i:2,j:-1,h:9,s:4,col:'#69db7c',shop:'bike'},
 {id:'home',n:'Your Home',mgr:'Family',i:-2,j:0,h:12,s:2,col:'#ffffff',shop:'home'}];
const JOBS={
 hotel:{n:'Hotel Servant',lvl:1,xp:15,k:'fix',pay:15,w:2,lab:'Room service, guest'},
 clean:{n:'Cleaning',lvl:1,xp:15,k:'spot',cnt:4,pay:12,w:2.5,lab:'Clean the mess',dirt:1},
 chai:{n:'Chai Wala',lvl:1,xp:12,k:'stay',pay:40,w:20,lab:'Serve tea and snacks'},
 deliv:{n:'Food Delivery (bike)',lvl:2,xp:20,k:'far',veh:'bike',pay:25,rate:.2,w:1,lab:'Deliver the order'},
 guard:{n:'Security Guard',lvl:2,xp:20,k:'spot',cnt:5,pay:14,w:1.5,lab:'Patrol checkpoint'},
 shop:{n:'Shopkeeper',lvl:2,xp:20,k:'stay',pay:55,w:25,lab:'Serve customers'},
 auto:{n:'Auto Rickshaw Driver',lvl:3,xp:22,k:'ride',veh:'auto',pay:18,rate:.22,lab:'passenger'},
 clerk:{n:'Office Clerk',lvl:3,xp:25,k:'stay',pay:80,w:30,lab:'Handle files at your desk'},
 cab:{n:'Cab Driver',lvl:4,xp:30,k:'ride',veh:'cab',pay:25,rate:.3,lab:'passenger'},
 build:{n:'Construction Worker',lvl:4,xp:30,k:'spot',cnt:3,pay:38,w:3,lab:'Carry bricks'},
 teach:{n:'Tuition Teacher',lvl:5,xp:40,k:'stay',pay:120,w:35,lab:'Teach the class'}};
const FOOD=[{n:'Water bottle',p:1,h:4},{n:'Masala Chai',p:3,h:8},{n:'Samosa',p:4,h:12},{n:'Veg Thali',p:12,h:40},{n:'Chicken Biryani',p:18,h:55}];
const OUT=[{n:'Casual Tee and Jeans',p:0,sh:0xc8352f,pa:0x2c4a7c,cap:0x1c1c22},{n:'Kurta and Pajama',p:120,sh:0xf3e9d2,pa:0xe8e2d0},{n:'Blue Hoodie',p:180,sh:0x2f5fd0,pa:0x2b2b30,cap:0x222222},{n:'Formal Suit',p:350,sh:0x1c2333,pa:0x14171f},{n:'Leather Style',p:420,sh:0x3a2a22,pa:0x1a1a1a,cap:0x0f0f0f},{n:'Golden Sherwani',p:600,sh:0xc9a227,pa:0xf0e6c8}];
const BIKES=[{k:'scooter',n:'Scooter',p:350,sp:12,col:0x8e44ad},{k:'moto',n:'Sports Motorbike',p:900,sp:18,col:0xe53935},{k:'car',n:'Hatchback Car',p:2500,sp:20,col:0x1e88e5,car:1}];
const VS={bike:15,auto:12,cab:14};

/* ---------- state ---------- */
let money=100,level=1,xp=0,hunger=100,outOwned=[0],outCur=0,shades=0,shadesOn=0,own=[];
let playing=false,menuOpen=false,yaw=0,pitch=.32,cd=6.5,sprint=false,wantJump=false,hour=9,jx=0,jy=0,veh=null,job=null,wanted=0,far=0,hw=0,hp=100,tlock=0,rain=0,rainOn=0,rainT=70,fc=0,pt=0,pname='',nosave=0;
const parked=[],cops=[],need=l=>40+30*l;
try{const s=JSON.parse(localStorage.getItem('rls2')||'null');if(s){money=s.money;level=s.level;xp=s.xp;hunger=s.hunger;outOwned=s.oo;outCur=s.oc;shades=s.sh;own=s.own||[];pname=s.name||''}}catch(e){}
const save=()=>{if(nosave)return;try{localStorage.setItem('rls2',JSON.stringify({money,level,xp,hunger,oo:outOwned,oc:outCur,sh:shades,own,name:pname}))}catch(e){}};
setInterval(save,3000);

/* ---------- renderer ---------- */
const cvs=$('g'),renderer=new T.WebGLRenderer({canvas:cvs,antialias:true});
renderer.setPixelRatio(Math.min(devicePixelRatio,2));renderer.shadowMap.enabled=true;renderer.shadowMap.type=T.PCFSoftShadowMap;
const scene=new T.Scene();scene.background=new T.Color(0x8ecdf5);scene.fog=new T.Fog(0x8ecdf5,80,420);
const cam=new T.PerspectiveCamera(60,1,.3,800);
function rs(){renderer.setSize(innerWidth,innerHeight);cam.aspect=innerWidth/innerHeight;cam.updateProjectionMatrix()}
addEventListener('resize',rs);rs();
const hemi=new T.HemisphereLight(0xbfdcff,0x556644,.7),sun=new T.DirectionalLight(0xffffff,1);
sun.castShadow=true;sun.shadow.mapSize.set(innerWidth>900?2048:1024,innerWidth>900?2048:1024);sun.shadow.bias=-.0006;
const sc=sun.shadow.camera;sc.left=sc.bottom=-65;sc.right=sc.top=65;sc.near=1;sc.far=320;
scene.add(hemi,sun,sun.target);

function cv(w,h,f){const c=document.createElement('canvas');c.width=w;c.height=h;f(c.getContext('2d'),w,h);return c}
function tx(c,rx,ry){const t=new T.CanvasTexture(c);t.wrapS=t.wrapT=T.RepeatWrapping;t.repeat.set(rx,ry);t.anisotropy=4;return t}
function lab(t,col,w,fs){const c=cv(512,96,(g,W)=>{g.font='bold '+fs+'px Trebuchet MS,sans-serif';g.textAlign='center';g.textBaseline='middle';g.fillStyle='rgba(0,0,0,.55)';g.fillRect(0,14,W,68);g.fillStyle=col;g.fillText(t,W/2,50)});
 const s=new T.Sprite(new T.SpriteMaterial({map:new T.CanvasTexture(c),depthTest:false,transparent:true}));s.scale.set(w,w*96/512,1);s.renderOrder=10;return s}
const blob=(rgb)=>new T.CanvasTexture(cv(64,64,(g)=>{const q=g.createRadialGradient(32,32,0,32,32,32);q.addColorStop(0,'rgba('+rgb+',1)');q.addColorStop(1,'rgba('+rgb+',0)');g.fillStyle=q;g.fillRect(0,0,64,64)}));
/* ---------- world ---------- */
const P=64,RW=14,H=3*P,LEN=2*H+120,cols=[];
const gr=cv(128,128,(g)=>{g.fillStyle='#3f7a34';g.fillRect(0,0,128,128);for(let i=0;i<1400;i++){g.fillStyle='hsl('+RI(85,120)+',45%,'+RI(24,40)+'%)';g.fillRect(R(0,128),R(0,128),2,2)}});
const ground=new T.Mesh(new T.PlaneGeometry(2400,2400),new T.MeshLambertMaterial({map:tx(gr,480,480)}));
ground.rotation.x=-Math.PI/2;ground.receiveShadow=true;scene.add(ground);
const rm=new T.MeshLambertMaterial({map:tx(cv(128,128,(g)=>{g.fillStyle='#34363a';g.fillRect(0,0,128,128);for(let i=0;i<1200;i++){g.fillStyle='rgba('+(RI(0,1)?255:0)+','+(RI(0,1)?255:0)+',255,.05)';g.fillRect(R(0,128),R(0,128),2,2)}
 g.fillStyle='#e8d98a';g.fillRect(0,62,64,4);g.fillStyle='#ddd';g.fillRect(0,5,128,3);g.fillRect(0,120,128,3)}),LEN/16,1)});
for(let k=-3;k<=3;k++){
 const a=new T.Mesh(new T.PlaneGeometry(LEN,RW),rm);a.rotation.x=-Math.PI/2;a.position.set(0,.02,k*P);a.receiveShadow=true;scene.add(a);
 const b=new T.Mesh(new T.PlaneGeometry(LEN,RW),rm);b.rotation.set(-Math.PI/2,0,Math.PI/2);b.position.set(k*P,.05,0);b.receiveShadow=true;scene.add(b)}
function gy(x,z){const u=((x%P)+P)%P,v=((z%P)+P)%P;return(u>7&&u<P-7&&v>7&&v<P-7&&Math.abs(x)<H&&Math.abs(z)<H)?.4:0}
function beacon(x,z,col,h,r){const m=new T.Mesh(new T.CylinderGeometry(r,r,h,16,1,true),new T.MeshBasicMaterial({color:col,transparent:true,opacity:.28,depthWrite:false,side:T.DoubleSide}));m.position.set(x,gy(x,z)+h/2,z);scene.add(m);return m}

const wall=[0xc9b8a3,0x93a4b4,0xa8695c,0xd5d8dc,0x7d8f83].map(col=>{const c='#'+col.toString(16).padStart(6,'0');
 const m=cv(256,256,(g)=>{g.fillStyle=c;g.fillRect(0,0,256,256);for(let y=0;y<4;y++)for(let x=0;x<4;x++){const q=g.createLinearGradient(0,y*64,0,y*64+64);q.addColorStop(0,'#a9cbe3');q.addColorStop(1,'#2c3e50');
  g.fillStyle='rgba(0,0,0,.28)';g.fillRect(x*64+11,y*64+9,42,46);g.fillStyle=q;g.fillRect(x*64+14,y*64+12,36,40);g.fillStyle='rgba(255,255,255,.22)';g.fillRect(x*64+14,y*64+12,36,5);g.fillStyle='rgba(0,0,0,.35)';g.fillRect(x*64+31,y*64+12,2,40)}});
 const e=cv(256,256,(g)=>{g.fillStyle='#000';g.fillRect(0,0,256,256);for(let y=0;y<4;y++)for(let x=0;x<4;x++)if(Math.random()<.55){g.fillStyle=PK(['#ffd27a','#ffe9b0','#ffc266']);g.fillRect(x*64+14,y*64+12,36,40)}});
 return new T.MeshLambertMaterial({map:tx(m,1,1),emissiveMap:tx(e,1,1),emissive:0xffffff,emissiveIntensity:0})});
const roofM=new T.MeshLambertMaterial({color:0x4d4f52}),roofs=[];
function bld(x,z,w,d,h,s){const g=new T.BoxGeometry(w,h,d),u=g.attributes.uv;
 for(let f=0;f<6;f++){const hz=f<2?d:w;for(let k=0;k<4;k++){const i=f*4+k;if(f===2||f===3)u.setXY(i,0,0);else u.setXY(i,u.getX(i)*hz/16,u.getY(i)*h/16)}}
 const m=new T.Mesh(g,[wall[s],wall[s],roofM,roofM,wall[s],wall[s]]);m.position.set(x,.4+h/2,z);m.castShadow=m.receiveShadow=true;scene.add(m);
 if(Math.random()<.7)roofs.push({x:x+R(-w/3,w/3),y:.4+h,z:z+R(-d/3,d/3)});
 cols.push({x,z,hx:w/2+.1,hz:d/2+.1,b:1})}

SITES.forEach(s=>{s.cx=(s.i+.5)*P;s.cz=(s.j+.5)*P;s.x=s.cx;s.z=s.cz+21});
const trees=[],lamps=[],baseG=new T.BoxGeometry(P-RW,.4,P-RW),baseM=new T.MeshLambertMaterial({color:0x9b9a94});
for(let i=-3;i<=2;i++)for(let j=-3;j<=2;j++){
 const cx=(i+.5)*P,cz=(j+.5)*P,sp=SITES.find(s=>s.i===i&&s.j===j);
 const b=new T.Mesh(baseG,baseM);b.position.set(cx,.2,cz);b.receiveShadow=true;scene.add(b);
 if(sp)bld(cx,cz+2,26,28,sp.h,sp.s);
 else if(Math.random()<.18){
  const p=new T.Mesh(new T.PlaneGeometry(46,46),new T.MeshLambertMaterial({color:0x4c8a3c}));p.rotation.x=-Math.PI/2;p.position.set(cx,.41,cz);p.receiveShadow=true;scene.add(p);
  const pd=new T.Mesh(new T.CircleGeometry(5,24),new T.MeshLambertMaterial({color:0x3d7fa8}));pd.rotation.x=-Math.PI/2;pd.position.set(cx,.43,cz);scene.add(pd);
  for(let n=0;n<14;n++){let x,z;do{x=R(-20,20);z=R(-20,20)}while(Math.hypot(x,z)<9);trees.push({x:cx+x,z:cz+z})}}
 else for(const[a,c]of[[-9,-9],[9,-9],[-9,9],[9,9]])if(Math.random()<.85)bld(cx+a,cz+c,R(11,16),R(11,16),R(10,55),RI(0,4));
 for(const s of[-1,1])for(let a=-18;a<=18;a+=12){
  if(Math.random()<.65)trees.push({x:cx+s*23.5,z:cz+a+R(-2,2)});
  if(Math.random()<.65)trees.push({x:cx+a+R(-2,2),z:cz+s*23.5})}
 for(const a of[-24,24])for(const c of[-24,24])lamps.push([cx+a,cz+c])}
{const n=trees.length,tm=new T.InstancedMesh(new T.CylinderGeometry(.18,.28,2.6,6),new T.MeshLambertMaterial({color:0x5a3d28}),n),
 cm=new T.InstancedMesh(new T.IcosahedronGeometry(1,1),new T.MeshLambertMaterial({color:0xffffff}),n),d=new T.Object3D(),c=new T.Color();
 trees.forEach((t,i)=>{const s=R(.9,1.5);d.rotation.set(0,0,0);d.position.set(t.x,.4+1.3*s,t.z);d.scale.set(s,s,s);d.updateMatrix();tm.setMatrixAt(i,d.matrix);
  d.position.y=.4+3.2*s;d.scale.set(1.8*s,1.6*s,1.8*s);d.updateMatrix();cm.setMatrixAt(i,d.matrix);cm.setColorAt(i,c.setHSL(R(.22,.32),.5,R(.2,.34)));
  cols.push({x:t.x,z:t.z,hx:.35,hz:.35})});
 tm.castShadow=cm.castShadow=true;tm.frustumCulled=cm.frustumCulled=false;scene.add(tm,cm)}
const lampM=new T.MeshBasicMaterial({color:0x444444}),glowM=new T.MeshBasicMaterial({map:blob('255,210,130'),transparent:true,depthWrite:false,blending:T.AdditiveBlending,opacity:0});
{const n=lamps.length,a=new T.InstancedMesh(new T.CylinderGeometry(.08,.12,6.5,6),new T.MeshLambertMaterial({color:0x33363b}),n),
 b=new T.InstancedMesh(new T.SphereGeometry(.32,8,6),lampM,n),c=new T.InstancedMesh(new T.PlaneGeometry(16,16).rotateX(-Math.PI/2),glowM,n),d=new T.Object3D();
 lamps.forEach((l,i)=>{d.position.set(l[0],3.65,l[1]);d.updateMatrix();a.setMatrixAt(i,d.matrix);d.position.y=7;d.updateMatrix();b.setMatrixAt(i,d.matrix);d.position.y=.47;d.updateMatrix();c.setMatrixAt(i,d.matrix);cols.push({x:l[0],z:l[1],hx:.2,hz:.2})});
 [a,b,c].forEach(m=>{m.frustumCulled=false;scene.add(m)})}
 /* extra world details */
{const d=new T.Object3D(),W=new T.MeshLambertMaterial({color:0xdddddd}),zs=[];
 for(let a=-3;a<=3;a++)for(let b=-3;b<=3;b++)for(const sg of[-1,1])for(let q=-5;q<=5;q+=2){zs.push([a*P+q,b*P+sg*9,0]);zs.push([a*P+sg*9,b*P+q,1])}
 const zm=new T.InstancedMesh(new T.BoxGeometry(.7,.03,3),W,zs.length);
 zs.forEach((z,i)=>{d.position.set(z[0],.09,z[1]);d.rotation.set(0,z[2]?Math.PI/2:0,0);d.updateMatrix();zm.setMatrixAt(i,d.matrix)});zm.frustumCulled=false;scene.add(zm);d.rotation.set(0,0,0);
 const tk=new T.InstancedMesh(new T.CylinderGeometry(.9,.9,1.6,10),new T.MeshLambertMaterial({color:0x222a35}),roofs.length),ac=new T.InstancedMesh(new T.BoxGeometry(1.2,.8,.9),new T.MeshLambertMaterial({color:0xc9ccd1}),roofs.length);
 roofs.forEach((r,i)=>{d.position.set(r.x,r.y+.8,r.z);d.updateMatrix();tk.setMatrixAt(i,d.matrix);d.position.set(r.x+2.5,r.y+.4,r.z+1.5);d.updateMatrix();ac.setMatrixAt(i,d.matrix)});
 tk.castShadow=ac.castShadow=true;tk.frustumCulled=ac.frustumCulled=false;scene.add(tk,ac);
 const bins=[],lit=[];for(let i=-3;i<=2;i++)for(let j=-3;j<=2;j++){const cx=(i+.5)*P,cz=(j+.5)*P;
  for(let n=0;n<2;n++){const sd=PK([-19.3,19.3]);bins.push(RI(0,1)?[cx+sd,cz+R(-14,14)]:[cx+R(-14,14),cz+sd])}
  for(let n=0;n<7;n++){const sd=PK([-1,1])*R(18,24);lit.push(RI(0,1)?[cx+sd,cz+R(-20,20)]:[cx+R(-20,20),cz+sd])}}
 const bm=new T.InstancedMesh(new T.CylinderGeometry(.32,.28,.9,10),new T.MeshLambertMaterial({color:0x2e7d32}),bins.length),lm=new T.InstancedMesh(new T.BoxGeometry(.25,.04,.18),new T.MeshLambertMaterial({color:0xffffff}),lit.length),c=new T.Color();
 bins.forEach((b,i)=>{d.position.set(b[0],.85,b[1]);d.updateMatrix();bm.setMatrixAt(i,d.matrix);cols.push({x:b[0],z:b[1],hx:.35,hz:.35})});
 lit.forEach((l,i)=>{d.position.set(l[0],.43,l[1]);d.rotation.y=R(0,6);d.updateMatrix();lm.setMatrixAt(i,d.matrix);lm.setColorAt(i,c.setHSL(R(0,1),.5,R(.4,.8)))});
 bm.castShadow=true;bm.frustumCulled=lm.frustumCulled=false;scene.add(bm,lm)}

/* sky objects */
const sunM=new T.Mesh(new T.SphereGeometry(16,16,12),new T.MeshBasicMaterial({color:0xfff2c0,fog:false})),moonM=new T.Mesh(new T.SphereGeometry(10,16,12),new T.MeshBasicMaterial({color:0xdfe8ff,fog:false}));
scene.add(sunM,moonM);
const sg=new T.BufferGeometry(),spos=[];for(let i=0;i<500;i++){const a=R(0,6.28),b=R(.05,1.4);spos.push(Math.cos(a)*Math.cos(b)*500,Math.sin(b)*500,Math.sin(a)*Math.cos(b)*500)}
sg.setAttribute('position',new T.Float32BufferAttribute(spos,3));
const stars=new T.Points(sg,new T.PointsMaterial({color:0xffffff,size:2,fog:false,transparent:true,sizeAttenuation:false}));stars.frustumCulled=false;scene.add(stars);
const cloudM=new T.SpriteMaterial({map:blob('255,255,255'),transparent:true,depthWrite:false,fog:false,opacity:.6}),clouds=[];
for(let i=0;i<18;i++){const c=new T.Sprite(cloudM);c.scale.set(R(80,140),R(30,50),1);c.position.set(R(-450,450),R(90,140),R(-450,450));scene.add(c);clouds.push(c)}

const domeG=new T.SphereGeometry(600,24,14),dcol=new Float32Array(domeG.attributes.position.count*3);domeG.setAttribute('color',new T.BufferAttribute(dcol,3));
const dome=new T.Mesh(domeG,new T.MeshBasicMaterial({vertexColors:true,side:T.BackSide,fog:false,depthWrite:false}));dome.renderOrder=-1;dome.frustumCulled=false;scene.add(dome);
const greyC=new T.Color(0x7d8794),nightZ=new T.Color(0x02040c),dayZ=new T.Color(0x3f86d6),zen=new T.Color(),dpos=domeG.attributes.position,dn=dpos.count;
const RN=600,rg=new T.BufferGeometry(),rpz=new Float32Array(RN*6);for(let i=0;i<RN;i++){const x=R(-40,40),y=R(0,30),z=R(-40,40);rpz.set([x,y,z,x,y-1.2,z],i*6)}
rg.setAttribute('position',new T.BufferAttribute(rpz,3));const rainM=new T.LineSegments(rg,new T.LineBasicMaterial({color:0xaac4e0,transparent:true,opacity:.45,fog:false}));rainM.frustumCulled=false;rainM.visible=false;scene.add(rainM);

/* ---------- people ---------- */
const pl={x:22,y:.4,z:-12,vy:0,ry:Math.PI,st:100,tired:false,ph:0};
function human(o){
 const g=new T.Group(),L=c=>new T.MeshLambertMaterial({color:c}),sk=L(o.skin),sh=L(o.shirt),pa=L(o.pants||0x333844),dk=L(0x15151a),hrm=L(o.cap||o.hair);
 const bx=(w,h,d,m,x,y,z,p)=>{const b=new T.Mesh(new T.BoxGeometry(w,h,d),m);b.position.set(x,y,z);b.castShadow=true;(p||g).add(b);return b};
 bx(.5,.62,.28,sh,0,1.22,0);
 const hd=new T.Mesh(new T.SphereGeometry(.16,14,10),sk);hd.position.y=1.68;hd.castShadow=true;g.add(hd);
 const hr=new T.Mesh(new T.SphereGeometry(.172,14,8,0,Math.PI*2,0,Math.PI*.55),hrm);hr.position.y=1.69;g.add(hr);
 const brim=bx(.3,.02,.17,hrm,0,1.76,.2);brim.visible=!!o.cap;g.m={sh,pa,hr:hrm,brim};
 if(o.eyes){bx(.03,.03,.02,dk,-.06,1.7,.155);bx(.03,.03,.02,dk,.06,1.7,.155);bx(.055,.012,.02,dk,-.06,1.74,.158);bx(.055,.012,.02,dk,.06,1.74,.158);
  bx(.03,.04,.03,sk,0,1.67,.17);bx(.06,.012,.01,L(0x8a3b3b),0,1.62,.16);bx(.02,.05,.04,sk,-.165,1.68,0);bx(.02,.05,.04,sk,.165,1.68,0);
  bx(.52,.06,.3,dk,0,.93,0);bx(.06,.05,.01,L(0xd4af37),0,.93,.155);const s=bx(.3,.05,.03,dk,0,1.7,.165);s.visible=false;g.m.shades=s}
 if(o.dress){const s=new T.Mesh(new T.CylinderGeometry(.22,.32,.5,12),sh);s.position.y=1;s.castShadow=true;g.add(s)}
 const leg=x=>{const p=new T.Group();p.position.set(x,.9,0);g.add(p);bx(.18,.85,.2,o.dress?sk:pa,0,-.42,0,p);bx(.2,.09,.3,dk,0,-.85,.04,p);return p};
 const arm=x=>{const p=new T.Group();p.position.set(x,1.48,0);g.add(p);bx(.14,.3,.15,sh,0,-.15,0,p);bx(.11,.32,.12,sk,0,-.46,0,p);return p};
 g.u={lL:leg(-.13),lR:leg(.13),aL:arm(-.34),aR:arm(.34)};g.scale.setScalar(o.sc||1);return g}
function swing(g,p,a){const u=g.u,s=Math.sin(p)*a;u.lL.rotation.x=s;u.lR.rotation.x=-s;u.aL.rotation.x=-s;u.aR.rotation.x=s}
const turn=(a,b,k)=>a+((((b-a+Math.PI)%(2*Math.PI))+2*Math.PI)%(2*Math.PI)-Math.PI)*k;
const me=human({shirt:0xc8352f,pants:0x2c4a7c,skin:0xb07850,hair:0x111111,cap:0x1c1c22,eyes:1});
scene.add(me);let tag=null;
function setName(n){pname=n;if(tag)me.remove(tag);tag=lab(n,'#ffffff',Math.max(2.2,n.length*.34),44);tag.position.y=2.15;me.add(tag)}
const hadName=!!pname;setName(pname||'Player');
function applyOut(){const o=OUT[outCur],m=me.m;m.sh.color.setHex(o.sh);m.pa.color.setHex(o.pa);if(o.cap){m.hr.color.setHex(o.cap);m.brim.visible=true}else{m.hr.color.setHex(0x111111);m.brim.visible=false}m.shades.visible=!!(shades&&shadesOn)}
applyOut();

const skins=[0xf1c8a5,0xd9a37c,0xb07850,0x7a4b2e,0x4f3020],hairs=[0x1a1208,0x3a2416,0x6b4a2a,0xb8894d,0x666666];
const types=[{shirt:0x1f2a44,pants:0x1a1a1f},{shirt:0xe8e8e8,pants:0x3b5b8c},{shirt:0xd94f4f,pants:0x2b2b2b},{shirt:0xf0c94d,pants:0x4a4a4a},{shirt:0x4fa36b,pants:0x6b5b3b},{dress:1,shirt:0xc9508b},{dress:1,shirt:0x5b7fd6},{sc:.75,shirt:0xff9f43,pants:0x2d4a8a},{shirt:0x7e57c2,pants:0x37474f},{shirt:0x00897b,pants:0x263238},{sc:.95,shirt:0xd7ccc8,pants:0x5d4037},{dress:1,shirt:0xffb300},{dress:1,shirt:0x8e24aa},{sc:.8,shirt:0x42a5f5,pants:0x1a237e},{shirt:0xf5f5f5,pants:0x455a64}];
const npcs=[];
for(let i=0;i<80;i++){
 const g=human(Object.assign({skin:PK(skins),hair:PK(hairs)},PK(types))),axis=RI(0,1),side=PK([-1,1]);
 const n={g,axis,c:RI(-3,3)*P+side*11,t:R(-H,H),dir:PK([-1,1]),sp:R(1.2,2),ph:R(0,6),cool:0,pause:0,bt:0,b:null,hp:3,agg:0,ko:0,hit:0};
 scene.add(g);npcs.push(n)}
function say(n,t){if(n.b)n.g.remove(n.b);n.b=lab(t,'#fff',4.4/n.g.scale.x,34);n.b.position.y=2.4;n.g.add(n.b);n.bt=3}
/* job sites: counter + manager + beacon */
SITES.forEach(s=>{
 beacon(s.x,s.z,new T.Color(s.col),40,1.3);const l=lab(s.n,s.col,9,40);l.position.set(s.x,9,s.z);scene.add(l);
 const c=new T.Mesh(new T.BoxGeometry(5,1.1,.9),new T.MeshLambertMaterial({color:0x6b4a2a}));c.position.set(s.x,.95,s.z-2);c.castShadow=true;scene.add(c);cols.push({x:s.x,z:s.z-2,hx:2.6,hz:.5});
 const m=human({skin:PK(skins),hair:0x222222,shirt:0xf2f2f2,pants:0x1a1a22});m.position.set(s.x,.4,s.z-3.6);scene.add(m);
 const t=lab(s.mgr||'Manager','#ffffff',2.6,40);t.position.y=2.2;m.add(t)});
const vendors=[],vendorSite={n:'Street Vendor',shop:'food'};
for(let n=0;n<10;n++){const i=RI(-3,2),j=RI(-3,2),cx=(i+.5)*P,cz=(j+.5)*P,sd=PK([-19.5,19.5]),h=RI(0,1),x=h?cx+sd:cx+R(-12,12),z=h?cz+R(-12,12):cz+sd;
 const g=new T.Group(),M=c=>new T.MeshLambertMaterial({color:c}),b=new T.Mesh(new T.BoxGeometry(1.8,.9,1),M(0x8d5a2b));b.position.y=.85;b.castShadow=true;g.add(b);
 const u=new T.Mesh(new T.ConeGeometry(1.7,.6,12),M(PK([0xe53935,0xfdd835,0x1e88e5,0x43a047])));u.position.y=2.6;g.add(u);
 const po=new T.Mesh(new T.CylinderGeometry(.04,.04,1.6,6),M(0x555555));po.position.y=1.8;g.add(po);g.position.set(x,.4,z);scene.add(g);
 const v=human({skin:PK(skins),hair:0x222222,shirt:PK([0xffffff,0xffcc80,0x90caf9]),pants:0x333333});v.position.set(x,.4,z-1.1);scene.add(v);
 vendors.push({x,z});cols.push({x,z,hx:.95,hz:.55})}

/* ---------- vehicles ---------- */
const carCols=[0xc0392b,0x2c3e50,0xf1c40f,0xecf0f1,0x27ae60,0x7f8c8d,0xe67e22,0x2980b9],cars=[];
function mkCar(col){const g=new T.Group(),M=c=>new T.MeshLambertMaterial({color:c}),add=(w,h,d,m,x,y,z,sh)=>{const b=new T.Mesh(new T.BoxGeometry(w,h,d),m);b.position.set(x,y,z);b.castShadow=!!sh;g.add(b)};
 const body=M(col);add(1.9,.7,4.3,body,0,.65,0,1);add(1.6,.5,2.2,M(0x1b2733),0,1.25,-.2,1);add(1.5,.08,1.9,body,0,1.53,-.2);
 add(1.5,.14,.05,new T.MeshBasicMaterial({color:0xfff2b0}),0,.75,2.16);add(1.5,.14,.05,new T.MeshBasicMaterial({color:0xd01818}),0,.75,-2.16);
 const w=new T.CylinderGeometry(.36,.36,2,12),wm=M(0x111111);for(const z of[-1.4,1.4]){const m=new T.Mesh(w,wm);m.rotation.z=Math.PI/2;m.position.set(0,.36,z);g.add(m)}return g}
function mkBike(col){const g=new T.Group(),M=c=>new T.MeshLambertMaterial({color:c}),add=(w,h,d,c,x,y,z,sh)=>{const b=new T.Mesh(new T.BoxGeometry(w,h,d),M(c));b.position.set(x,y,z);b.castShadow=!!sh;g.add(b)};
 add(.22,.32,1.3,col,0,.6,0,1);add(.3,.24,.55,col,0,.88,.2,1);add(.26,.1,.6,0x111111,0,.8,-.3);add(.75,.05,.05,0x222222,0,1.05,.55);add(.16,.14,.08,0xfff2b0,0,.95,.72);
 const w=new T.CylinderGeometry(.34,.34,.14,14),wm=M(0x111111);for(const z of[-.72,.72]){const m=new T.Mesh(w,wm);m.rotation.z=Math.PI/2;m.position.set(0,.34,z);g.add(m)}return g}
for(let axis=0;axis<2;axis++)for(let k=-3;k<=3;k++)for(const dir of[1,-1]){
 const n=RI(1,2);for(let q=0;q<n;q++){const g=mkCar(PK(carCols));scene.add(g);
  cars.push({g,axis,dir,off:k*P+(axis?-dir:dir)*3.5,t:-H-60+(q+R(0,.5))*(2*H+120)/n,sp:R(10,13),v:11});
  g.rotation.y=axis?(dir>0?0:Math.PI):(dir>0?Math.PI/2:-Math.PI/2)}}
for(let q=0;q<12;q++){const axis=RI(0,1),k=RI(-3,3),dir=PK([1,-1]),g=mkBike(PK(carCols)),r=human({skin:PK(skins),hair:PK(hairs),shirt:PK([0x2f5fd0,0xd94f4f,0x4fa36b,0xeeeeee]),pants:0x222831,cap:PK([0xb71c1c,0x222222,0x1565c0])});
 r.position.set(0,-.05,-.1);r.u.lL.rotation.x=r.u.lR.rotation.x=-1.15;r.u.aL.rotation.x=r.u.aR.rotation.x=-.9;g.add(r);scene.add(g);
 cars.push({g,axis,dir,off:k*P+(axis?-dir:dir)*3.5+(dir>0?1:-1)*.2,t:R(-H,H),sp:R(11,14),v:11,bike:1,rider:r});
 g.rotation.y=axis?(dir>0?0:Math.PI):(dir>0?Math.PI/2:-Math.PI/2)}
function spawnOwn(k,i){const b=BIKES.find(x=>x.k===k),g=b.car?mkCar(b.col):mkBike(b.col);g.position.set(pl.x+(b.car?5:3)+i*2.6,gy(pl.x,pl.z),pl.z+2.5);g.rotation.y=pl.ry;scene.add(g);parked.push({g,sp:b.sp,own:1,car:!!b.car})}
own.forEach((k,i)=>spawnOwn(k,i));
function mount(v){const i=parked.indexOf(v);if(i>=0)parked.splice(i,1);veh=v;me.visible=!v.car;pl.ry=v.g.rotation.y}
function dismount(rm){if(!veh)return;const v=veh;veh=null;me.visible=true;swing(me,0,0);
 if(rm||v.rent)scene.remove(v.g);else{pl.x+=-Math.cos(pl.ry)*1.5;pl.z+=Math.sin(pl.ry)*1.5;parked.push(v)}}
function steal(c){if(veh)return;cars.splice(cars.indexOf(c),1);const p=c.g.position,r=c.rider||human({skin:PK(skins),hair:PK(hairs),shirt:0x6d4c41,pants:0x222222});c.g.remove(r);
 r.position.set(p.x+(c.bike?1.4:2.4),gy(p.x,p.z),p.z);r.rotation.set(0,c.g.rotation.y+2.5,0);swing(r,0,0);scene.add(r);
 const b=lab('Hey! Thief! Police!','#ff6b6b',5,34);b.position.y=2.3;r.add(b);setTimeout(()=>scene.remove(r),7000);
 mount({g:c.g,car:!c.bike,sp:c.bike?17:19,stolen:1});startWanted()}

/* ---------- police ---------- */
function startWanted(){if(wanted)return;wanted=1;far=0;toast('Police are chasing you! Escape!');
 for(let i=0;i<3;i++){const a=R(0,6.28),d=40+i*10,g=human({skin:PK(skins),hair:0x111111,shirt:0x1e3a8a,pants:0x111827,cap:0x111827});
  g.position.set(pl.x+Math.cos(a)*d,0,pl.z+Math.sin(a)*d);scene.add(g);cops.push({g,ph:0})}}
function endWanted(){cops.forEach(c=>scene.remove(c.g));cops.length=0;wanted=0;far=0}
function busted(){const l=Math.floor(money*.3);money-=l;endWanted();clearJob();
 if(veh){const v=veh;dismount(v.stolen)}
 pl.x=22;pl.z=-12;pl.y=.4;toast('Busted! Police took $'+l+' (30% of your money)')}

/* ---------- jobs ---------- */
const rp=()=>{const i=RI(-3,2),j=RI(-3,2),cx=(i+.5)*P,cz=(j+.5)*P,s=PK([-21,21]);return RI(0,1)?{x:cx+s,z:cz+R(-15,15)}:{x:cx+R(-15,15),z:cz+s}};
function clearJob(){if(!job)return;job.t.forEach(t=>{scene.remove(t.m);if(t.prop)scene.remove(t.prop)});job=null;if(veh&&veh.rent)dismount(1)}
function addXp(x){xp+=x;while(xp>=need(level)){xp-=need(level);level++;toast('LEVEL UP! You are level '+level)}}
function startJob(id,s){if(wanted)return toast('Lose the police first');const d=JOBS[id];let ts=[];
 const dd=p=>Math.hypot(p.x-s.x,p.z-s.z),near=(mx,mn)=>{let p;do p=rp();while(dd(p)>mx||dd(p)<mn);return p};
 if(d.k==='fix')ts=[{x:s.cx-21,z:s.cz+R(-14,14)},{x:s.cx+21,z:s.cz+R(-14,14)},{x:s.cx+R(-14,14),z:s.cz-21}].map((p,i)=>({...p,pay:d.pay,w:d.w,lab:d.lab+' '+(i+1)+'/3'}));
 if(d.k==='spot')for(let i=0;i<d.cnt;i++){const p=near(90,12);ts.push({...p,pay:d.pay,w:d.w,lab:d.lab+' '+(i+1)+'/'+d.cnt,dirt:d.dirt})}
 if(d.k==='stay')ts=[{x:s.x,z:s.z,pay:d.pay,w:d.w,lab:d.lab}];
 if(d.k==='far'){const p=near(400,70);ts=[{...p,pay:Math.round(d.pay+dd(p)*d.rate),w:d.w,lab:d.lab}]}
 if(d.k==='ride'){const a=near(80,15),b=near(400,90);ts=[{...a,pay:0,w:1,lab:'Pick up the passenger'},{...b,pay:Math.round(d.pay+Math.hypot(a.x-b.x,a.z-b.z)*d.rate),w:1,lab:'Drop the passenger'}]}
 if(d.veh){if(veh)dismount();const car=d.veh!=='bike',g=car?mkCar(d.veh==='auto'?0x27ae60:0xf2c230):mkBike(0xff7043);g.position.set(s.x,gy(s.x,s.z),s.z+2);scene.add(g);mount({g,car,sp:VS[d.veh],rent:1})}
 const pk=id==='clean'?'trash':id==='build'?'brick':id==='guard'?'post':id==='deliv'?'parcel':null;
 ts.forEach((t,i)=>{t.prog=0;t.m=beacon(t.x,t.z,new T.Color(s.col),14,.9);t.k=pk;t.prop=mkProp(pk||(d.k==='ride'&&i===0?'pax':null),t.x,t.z)});
 job={id,d,site:s,t:ts,i:0};toast(d.n+' started')}

/* ---------- menus ---------- */
function menu(title,rows){const m=$('menu');m.innerHTML='';m.appendChild(el('h2','',title));
 rows.forEach(r=>{const w=el('div','row'),l=el('div');l.appendChild(el('b','',r.t));l.appendChild(el('small','',r.d||''));w.appendChild(l);
  const b=el('button','',r.b);if(r.dis)b.disabled=true;b.onclick=()=>r.f&&r.f();w.appendChild(b);m.appendChild(w)});
 const c=el('button','close','Close');c.onclick=closeMenu;m.appendChild(c);m.style.display='block';menuOpen=true;jx=jy=0;$('knob').style.transform=''}
function closeMenu(){$('menu').style.display='none';menuOpen=false}
const payTxt=d=>d.k==='stay'?'$'+d.pay+' per shift':(d.k==='far'||d.k==='ride')?'$'+d.pay+'+ by distance':'$'+d.pay+' each, '+d.xp+' XP';
function openSite(s){const rows=[];
 if(job&&job.site===s)rows.push({t:'Quit current job',d:job.d.n,b:'Quit',f:()=>{clearJob();closeMenu();toast('You quit the job')}});
 else if(s.jobs)s.jobs.forEach(id=>{const d=JOBS[id];rows.push({t:d.n,d:'Level '+d.lvl+' · '+payTxt(d),b:level<d.lvl?'Lv '+d.lvl:job?'Busy':'Start',dis:level<d.lvl||!!job,f:()=>{closeMenu();startJob(id,s)}})});
 if(s.shop==='food')FOOD.forEach(f=>rows.push({t:f.n,d:'Hunger +'+f.h,b:'$'+f.p,dis:money<f.p,f:()=>{money-=f.p;hunger=Math.min(100,hunger+f.h);toast('Yum! '+f.n);openSite(s)}}));
 if(s.shop==='cloth'){OUT.forEach((o,i)=>{const ow=outOwned.includes(i);rows.push({t:o.n,d:i===outCur?'Wearing now':'',b:i===outCur?'Worn':ow?'Wear':'$'+o.p,dis:i===outCur||(!ow&&money<o.p),f:()=>{if(!ow){money-=o.p;outOwned.push(i)}outCur=i;applyOut();openSite(s)}})});
  rows.push({t:'Sunglasses',d:shades?(shadesOn?'On':'Off'):'',b:shades?(shadesOn?'Remove':'Wear'):'$60',dis:!shades&&money<60,f:()=>{if(!shades){money-=60;shades=1;shadesOn=1}else shadesOn=1-shadesOn;applyOut();openSite(s)}})}
 if(s.shop==='bike')BIKES.forEach(b=>rows.push({t:b.n,d:'Top speed '+b.sp+' m/s. Parks outside.',b:'$'+b.p,dis:money<b.p,f:()=>{money-=b.p;own.push(b.k);spawnOwn(b.k,own.length-1);closeMenu();toast('Bought a '+b.n+'! It is parked next to you.')}}));
 if(s.shop==='home')rows.push({t:'Sleep until morning',d:'Restores health and skips the night',b:'Sleep',f:()=>{hour=7;hp=100;hunger=Math.max(0,hunger-15);closeMenu();toast('You slept well. Good morning, '+pname+'!')}});
 menu(s.n,rows.length?rows:[{t:'Nothing here',b:'-',dis:1}])}
 /* ---------- interaction ---------- */
const nearSite=()=>SITES.find(s=>Math.hypot(s.x-pl.x,s.z-pl.z)<5);
function nearNpc(r){let b=null,bd=r;for(const n of npcs){const d=Math.hypot(n.g.position.x-pl.x,n.g.position.z-pl.z);if(d<bd){bd=d;b=n}}return b}
function ctx(){const s=nearSite();if(s)return{k:'site',s};for(const v of vendors)if(Math.hypot(v.x-pl.x,v.z-pl.z)<3.5)return{k:'site',s:vendorSite};return null}
function act(){if(!playing||menuOpen)return;const c=ctx();if(!c)return toast('Nothing to use nearby');openSite(c.s)}
const refuse=['Sorry, I have no cash.','Get a job, man!','Not today, buddy.','I am in a hurry.','Ask someone else.','No, sorry.'];
const round=()=>{const n=RI(3,4);return{g:RI(0,n-1),c:0}};let rnd=round();
function ask(){if(!playing||menuOpen)return;const n=nearNpc(3.2);if(!n)return toast('Nobody close enough to ask');
 n.pause=3;if(n.cool>0)return say(n,'I already helped you.');n.cool=45;
 if(rnd.c===rnd.g){const a=RI(5,20);money+=a;say(n,'Here, take $'+a+'.');toast('+$'+a+' from a kind stranger');rnd=round()}
 else{say(n,PK(refuse));rnd.c++}}
let tt;function toast(s){const e=$('toast');e.textContent=s;e.style.opacity=1;clearTimeout(tt);tt=setTimeout(()=>e.style.opacity=0,2400)}
const SITE_URL='',UPI='mus892164@okaxis';
function copyTxt(t,msg){const done=()=>toast(msg),fb=()=>{const a=document.createElement('textarea');a.value=t;a.style.cssText='position:fixed;opacity:0';document.body.appendChild(a);a.select();try{document.execCommand('copy')}catch(e){}a.remove();done()};
 try{navigator.clipboard.writeText(t).then(done,fb)}catch(e){fb()}}
function share(){const url=SITE_URL||location.href.split('#')[0],data={title:'Real Life Simulator',text:'Play Real Life Simulator, a free 3D open-world life game! ',url};
 if(navigator.share)navigator.share(data).catch(()=>{});else copyTxt(data.text+url,'Game link copied. Paste it to share!')}
function donate(){menu('Support the developer',[{t:'UPI ID',d:UPI,b:'Copy',f:()=>copyTxt(UPI,'UPI ID copied')},{t:'Pay with a UPI app',d:'Any amount. Thank you for supporting the game!',b:'Open',f:()=>{location.href='upi://pay?pa='+UPI+'&pn=Real%20Life%20Simulator&cu=INR'}},{t:'Back',b:'Back',f:mainMenu}])}
const TL=['Auto (day and night cycle)','Always day','Always night'];
function mainMenu(){menu('Menu: '+pname,[{t:'Share this game',d:'Send the game link to friends',b:'Share',f:share},{t:'Support the developer',d:'Donate via UPI',b:'Donate',f:donate},
 {t:'Time of day',d:TL[tlock],b:'Change',f:()=>{tlock=(tlock+1)%3;mainMenu()}},
 {t:'Fullscreen',d:'Bigger playing area',b:'Toggle',f:()=>{try{document.fullscreenElement?document.exitFullscreen():document.documentElement.requestFullscreen()}catch(e){}closeMenu()}},
 {t:'Change name',d:pname,b:'Rename',f:()=>{const n=(prompt('Enter your character name',pname)||'').trim().slice(0,14);if(n){setName(n);save()}mainMenu()}},
 {t:'Reset progress',d:'Deletes money, level and items',b:'Reset',f:()=>{if(confirm('Reset all progress?')){nosave=1;try{localStorage.removeItem('rls2')}catch(e){}location.reload()}}}])}
function fight(){if(!playing||menuOpen)return;if(veh)return toast('Get off the vehicle to fight');if(fc>0)return;fc=.55;pt=.3;
 const hx=Math.sin(pl.ry),hz=Math.cos(pl.ry);let b=null,bd=2.4;
 for(const n of npcs){if(n.ko>0)continue;const dx=n.g.position.x-pl.x,dz=n.g.position.z-pl.z,d=Math.hypot(dx,dz);if(d<bd&&dx*hx+dz*hz>-.3*d){bd=d;b=n}}
 if(!b)return;const dx=b.g.position.x-pl.x,dz=b.g.position.z-pl.z,d=Math.hypot(dx,dz)||1;b.hp-=1;
 if(b.agg){b.g.position.x+=dx/d*.7;b.g.position.z+=dz/d*.7}else b.t+=(b.axis?dz:dx)/d*.7;
 if(b.hp<=0){b.ko=9;b.agg=0;b.g.rotation.x=-Math.PI/2;b.g.position.y=gy(b.g.position.x,b.g.position.z)+.18;toast('Knocked out! Someone called the police');startWanted()}
 else{say(b,PK(['Ouch!','Hey! Stop it!','Are you crazy?!']));if(Math.random()<.55)b.agg=1;else b.sp=Math.min(3.6,b.sp+1.5)}}
const dst=g=>Math.hypot(g.position.x-pl.x,g.position.z-pl.z);
function dctx(){if(veh)return veh.rent?null:'exit';for(const v of parked)if(dst(v.g)<3.5)return v;for(const c of cars)if(dst(c.g)<4.2)return c;return null}
function drive(){if(!playing||menuOpen)return;const c=dctx();if(!c)return toast(veh?'Return to your job site to quit the job':'No vehicle nearby');
 if(c==='exit')dismount();else if(parked.includes(c))mount(c);else steal(c)}
function faint(){const f=Math.min(50,Math.floor(money));money-=f;hp=60;clearJob();if(veh)dismount(veh.stolen);endWanted();const h=SITES.find(q=>q.id==='home');pl.x=h.x;pl.z=h.z+2.5;pl.y=.4;toast('You collapsed. Hospital bill $'+f+'. You woke up at home.')}
function mkProp(k,x,z){if(!k)return null;const g=new T.Group(),M=c=>new T.MeshLambertMaterial({color:c}),
 add=(geo,c,px,py,pz,rot)=>{const m=new T.Mesh(geo,M(c));m.position.set(px,py,pz);if(rot)m.rotation.set(R(0,3),R(0,3),R(0,3));m.castShadow=true;g.add(m);return m};
 g.position.set(x,gy(x,z),z);scene.add(g);
 if(k==='trash'){const st=new T.Mesh(new T.CircleGeometry(1.6,20),new T.MeshBasicMaterial({color:0x2b2118,transparent:true,opacity:.7}));st.rotation.x=-Math.PI/2;st.position.y=.05;g.add(st);
  for(let i=0;i<14;i++){const a=R(0,6.28),r=R(0,1.4),px=Math.cos(a)*r,pz=Math.sin(a)*r,q=RI(0,3);
   if(q===0)add(new T.SphereGeometry(R(.1,.18),6,5),0xf0f0f0,px,.14,pz,1);
   else if(q===1)add(new T.CylinderGeometry(.06,.06,.24,8),PK([0x2e7d32,0x8d6e63,0x1976d2]),px,.1,pz,1);
   else if(q===2)add(new T.BoxGeometry(.32,.04,.22),PK([0xf9d71c,0xe53935,0xffffff]),px,.06,pz,0);
   else add(new T.CylinderGeometry(.07,.07,.12,8),0xb0b0b0,px,.1,pz,1)}
  add(new T.SphereGeometry(.55,8,6),0x151515,.3,.45,-.3,0);add(new T.SphereGeometry(.4,8,6),0x1c1c1c,-.6,.32,.4,0)}
 else if(k==='brick'){for(let i=0;i<12;i++)add(new T.BoxGeometry(.4,.2,.2),0xb5533c,(i%4-1.5)*.42,.1+Math.floor(i/4)*.21,0,0);add(new T.BoxGeometry(.7,.3,.45),0x9e9e9e,1.4,.15,.2,0)}
 else if(k==='post'){add(new T.CylinderGeometry(.02,.32,.9,10),0xff7a00,0,.45,0,0);add(new T.BoxGeometry(.5,.35,.05),0x1976d2,0,1.1,0,0)}
 else if(k==='parcel'){add(new T.BoxGeometry(.7,.5,.5),0xc59b64,0,.25,0,0);add(new T.BoxGeometry(.72,.06,.12),0x8d6e63,0,.5,0,0)}
 else if(k==='pax'){const h=human({skin:PK(skins),hair:PK(hairs),shirt:PK([0x1f2a44,0xd94f4f,0x4fa36b,0xeeeeee]),pants:0x333844});h.rotation.y=R(0,6);g.add(h)}
 return g}

/* ---------- input ---------- */
const keys={};addEventListener('keydown',e=>{if(e.target.tagName==='INPUT')return;keys[e.code]=1;if(e.code==='KeyE')act();if(e.code==='KeyF')ask();if(e.code==='KeyQ')fight();if(e.code==='KeyR')drive();if(e.code==='Space'){wantJump=true;e.preventDefault()}});
addEventListener('keyup',e=>keys[e.code]=0);document.oncontextmenu=e=>e.preventDefault();
const joy=$('joy'),knob=$('knob');let jid=null;
function jm(e){const r=joy.getBoundingClientRect();let dx=e.clientX-(r.left+r.width/2),dy=e.clientY-(r.top+r.height/2);const m=r.width*.3,l=Math.hypot(dx,dy);if(l>m){dx*=m/l;dy*=m/l}knob.style.transform='translate('+dx+'px,'+dy+'px)';jx=dx/m;jy=dy/m}
joy.onpointerdown=e=>{jid=e.pointerId;joy.setPointerCapture(jid);jm(e)};
joy.onpointermove=e=>{if(e.pointerId===jid)jm(e)};
joy.onpointerup=joy.onpointercancel=()=>{jid=null;jx=jy=0;knob.style.transform=''};
const B=(id,f)=>$(id).addEventListener('pointerdown',e=>{e.preventDefault();f()});
B('bJ',()=>wantJump=true);B('bR',()=>{sprint=!sprint;$('bR').classList.toggle('on',sprint)});B('bA',act);B('bK',ask);B('bF',fight);B('bD',drive);$('mb').addEventListener('pointerdown',e=>{e.preventDefault();if(playing)menuOpen?closeMenu():mainMenu()});
const drags={};cvs.onpointerdown=e=>{drags[e.pointerId]=[e.clientX,e.clientY];cvs.setPointerCapture(e.pointerId)};
cvs.onpointermove=e=>{const d=drags[e.pointerId];if(!d)return;yaw-=(e.clientX-d[0])*.006;pitch=Math.max(.05,Math.min(1.2,pitch+(e.clientY-d[1])*.004));d[0]=e.clientX;d[1]=e.clientY};
cvs.onpointerup=cvs.onpointercancel=e=>delete drags[e.pointerId];
$('nm').value=hadName?pname:'';$('sh').onclick=share;$('dn').onclick=donate;
$('go').onclick=()=>{setName($('nm').value.trim().slice(0,14)||pname||'Player');save();playing=true;$('start').style.display='none';toast('Welcome, '+pname+'!');
 try{const r=document.documentElement;Promise.resolve(r.requestFullscreen&&r.requestFullscreen()).then(()=>screen.orientation&&screen.orientation.lock('landscape')).catch(()=>{})}catch(e){}};

function collide(p,rad){for(const c of cols){const nx=Math.max(c.x-c.hx,Math.min(p.x,c.x+c.hx)),nz=Math.max(c.z-c.hz,Math.min(p.z,c.z+c.hz));
 let dx=p.x-nx,dz=p.z-nz,d=Math.hypot(dx,dz);if(d<rad){if(d<1e-4){dx=1;dz=0;d=1}p.x+=dx/d*(rad-d);p.z+=dz/d*(rad-d)}}}

/* ---------- day and night ---------- */
const dayC=new T.Color(0x8ecdf5),nightC=new T.Color(0x060a18),duskC=new T.Color(0xff8c5a),sky=new T.Color(),sd=new T.Vector3();
function env(){const a=(hour-6)/12*Math.PI,s=Math.sin(a),k=T.MathUtils.smoothstep(s,-.15,.35),d=Math.max(0,1-Math.abs(s)*3.5);
 sky.copy(nightC).lerp(dayC,k).lerp(duskC,d*.55).lerp(greyC,rain*.55*(.3+.7*k));scene.background=sky;scene.fog.color.copy(sky);scene.fog.near=80-45*rain;scene.fog.far=420-190*rain;
 zen.copy(nightZ).lerp(dayZ,k).lerp(greyC,rain*.5);
 for(let i=0;i<dn;i++){const t=Math.pow(Math.max(0,dpos.getY(i)/600),.55);dcol[i*3]=sky.r+(zen.r-sky.r)*t;dcol[i*3+1]=sky.g+(zen.g-sky.g)*t;dcol[i*3+2]=sky.b+(zen.b-sky.b)*t}
 domeG.attributes.color.needsUpdate=true;dome.position.set(pl.x,0,pl.z);
 sd.set(Math.cos(a),s*.9,.3).normalize();
 sun.intensity=Math.max(0,s)*1.15*(1-.6*rain);sun.color.setRGB(1,.85+.15*(1-d),.7+.3*(1-d));hemi.intensity=.3+.5*k;
 sun.position.set(pl.x+sd.x*90,Math.max(sd.y,.12)*90,pl.z+sd.z*90);sun.target.position.set(pl.x,0,pl.z);
 sunM.position.set(pl.x+sd.x*430,sd.y*430,pl.z+sd.z*430);moonM.position.set(pl.x-sd.x*430,-sd.y*430,pl.z-sd.z*430);
 stars.position.set(pl.x,0,pl.z);stars.material.opacity=1-k;cloudM.opacity=Math.min(1,.15+.5*k+.3*rain);
 wall.forEach(m=>m.emissiveIntensity=(1-k)*.95);lampM.color.setRGB(.3+.7*(1-k),.3+.55*(1-k),.3+.2*(1-k));glowM.opacity=(1-k)*.85}

/* ---------- update ---------- */
function update(dt){
 if(tlock===1)hour=11;else if(tlock===2)hour=0;else hour=(hour+dt*.05)%24;hunger=Math.max(0,hunger-dt*.18);if(playing){if(hunger>30)hp=Math.min(100,hp+dt*.6);if(hp<=0)faint()}if(fc>0)fc-=dt;rainT-=dt;if(rainT<0){rainOn=rainOn?0:(Math.random()<.5?1:0);rainT=R(60,150);if(rainOn)toast('It started raining')}rain+=(rainOn-rain)*Math.min(1,dt*.3);
 let ix=0,iz=0;if(playing&&!menuOpen){ix=jx+((keys.KeyD||keys.ArrowRight)?1:0)-((keys.KeyA||keys.ArrowLeft)?1:0);iz=jy+((keys.KeyS||keys.ArrowDown)?1:0)-((keys.KeyW||keys.ArrowUp)?1:0)}
 const m=Math.min(1,Math.hypot(ix,iz)),cs=Math.cos(yaw),sn=Math.sin(yaw);
 if(playing){
  if(pl.st<=0)pl.tired=true;if(pl.st>25)pl.tired=false;
  const starving=hunger<=0,run=!veh&&(sprint||keys.ShiftLeft)&&!pl.tired&&!starving&&m>.1;
  if(m>.05){const mx=cs*ix+sn*iz,mz=-sn*ix+cs*iz,l=Math.hypot(mx,mz);
   if(veh){pl.ry=turn(pl.ry,Math.atan2(mx,mz),Math.min(1,dt*4));pl.x+=Math.sin(pl.ry)*veh.sp*m*dt;pl.z+=Math.cos(pl.ry)*veh.sp*m*dt}
   else{const sp=(run?8.5:4.2)*(starving?.65:1);pl.x+=mx/l*m*sp*dt;pl.z+=mz/l*m*sp*dt;pl.ry=turn(pl.ry,Math.atan2(mx,mz),Math.min(1,dt*12));pl.ph+=dt*(run?13:9)*m}}
  if(!veh)pl.st=Math.max(0,Math.min(100,pl.st+(run?-20:(starving?4:12))*dt));
  const g0=gy(pl.x,pl.z);
  if(wantJump&&!veh&&pl.y<=g0+.02&&pl.vy===0&&pl.st>8){pl.vy=6.4;pl.st-=8}wantJump=false;
  pl.vy-=20*dt;pl.y+=pl.vy*dt;if(pl.y<=g0){pl.y=g0;pl.vy=0}
  collide(pl,veh&&veh.car?1.3:.55);pl.x=Math.max(-300,Math.min(300,pl.x));pl.z=Math.max(-300,Math.min(300,pl.z));
  if(!veh)swing(me,pl.ph,m>.05?(run?1:.65):0);
 }else yaw+=dt*.12;
 me.rotation.y=pl.ry;
 if(veh){veh.g.position.set(pl.x,pl.y,pl.z);veh.g.rotation.y=pl.ry;me.position.set(pl.x,pl.y-.05,pl.z);
  if(!veh.car){const u=me.u;u.lL.rotation.x=u.lR.rotation.x=-1.15;u.aL.rotation.x=u.aR.rotation.x=-.9}}
 else me.position.set(pl.x,pl.y,pl.z);
 if(pt>0){pt-=dt;me.u.aR.rotation.x=-1.7*Math.sin(Math.PI*Math.max(0,1-pt/.3));me.u.aL.rotation.x=.5}
 if(rain>.05){rainM.visible=true;rainM.position.set(pl.x,0,pl.z);for(let i=0;i<RN;i++){let y=rpz[i*6+1]-dt*32;if(y<0)y+=30;rpz[i*6+1]=y;rpz[i*6+4]=y-1.2}rg.attributes.position.needsUpdate=true}else rainM.visible=false;

 for(const n of npcs){
  const p=n.g.position,pd=Math.hypot(pl.x-p.x,pl.z-p.z),vis=pd<115;n.g.visible=vis;
  if(n.bt>0){n.bt-=dt;if(n.bt<=0&&n.b){n.g.remove(n.b);n.b=null}}if(n.cool>0)n.cool-=dt;
  if(n.ko>0){n.ko-=dt;if(n.ko<=0){n.g.rotation.x=0;n.hp=3;n.agg=0}continue}
  if(n.agg){
   if(pd>30||(veh&&pd>14)){n.agg=0;n.axis=RI(0,1);const sn2=v=>{const k=Math.round(v/P),a=k*P+11,b=k*P-11;return Math.abs(v-a)<Math.abs(v-b)?a:b};if(n.axis){n.t=p.z;n.c=sn2(p.x)}else{n.t=p.x;n.c=sn2(p.z)}}
   else{const dx=pl.x-p.x,dz=pl.z-p.z;n.g.rotation.y=turn(n.g.rotation.y,Math.atan2(dx,dz),.3);
    if(pd>1.5){const q={x:p.x+dx/pd*3.4*dt,z:p.z+dz/pd*3.4*dt};collide(q,.5);p.x=q.x;p.z=q.z;p.y=gy(p.x,p.z);n.ph+=dt*10;if(vis)swing(n.g,n.ph,.8)}
    else{n.fc=(n.fc||0)-dt;if(vis)swing(n.g,0,0);if(n.fc<=0){n.fc=1.1;n.pt=.3;if(!veh){hp-=RI(4,8);toast('You got hit!')}}}
    if(n.pt>0){n.pt-=dt;n.g.u.aR.rotation.x=-1.7*Math.sin(Math.PI*Math.max(0,1-n.pt/.3))}
    continue}}
  if(n.pause>0||pd<1.3){n.pause-=dt;n.g.rotation.y=turn(n.g.rotation.y,Math.atan2(pl.x-p.x,pl.z-p.z),.15);if(vis)swing(n.g,0,0)}
  else{n.t+=n.dir*n.sp*dt;if(Math.abs(n.t)>H+20)n.dir*=-1;n.ph+=dt*n.sp*4.5;
   const x=n.axis?n.c:n.t,z=n.axis?n.t:n.c;n.g.rotation.y=turn(n.g.rotation.y,Math.atan2(n.axis?0:n.dir,n.axis?n.dir:0),.2);
   p.set(x,gy(x,z),z);if(vis)swing(n.g,n.ph,.6)}}

 for(const c of cars){
  const pt=c.axis?pl.z:pl.x,lp=c.axis?pl.x:pl.z,ah=(pt-c.t)*c.dir,lat=Math.abs(lp-c.off),stop=ah>0&&ah<10&&lat<2.4&&pl.y<2;
  c.v+=((stop?0:c.sp)-c.v)*Math.min(1,dt*(stop?3:.8));c.t+=c.dir*c.v*dt;
  if(c.t>H+60)c.t-=2*H+120;else if(c.t<-H-60)c.t+=2*H+120;
  if(c.axis)c.g.position.set(c.off,0,c.t);else c.g.position.set(c.t,0,c.off);
  if(!veh&&Math.abs(ah)<2.8&&lat<1.4&&c.v>1&&pl.y<1.6){const sg=(lp-c.off)>=0?1:-1;if(c.axis)pl.x=c.off+sg*1.5;else pl.z=c.off+sg*1.5;pl.st=Math.max(0,pl.st-25);toast('Watch out for cars!')}}

 if(wanted){let md=1e9;for(const c of cops){const p=c.g.position,dx=pl.x-p.x,dz=pl.z-p.z,d=Math.hypot(dx,dz);md=Math.min(md,d);
   if(d>1.2){const q={x:p.x+dx/d*9*dt,z:p.z+dz/d*9*dt};collide(q,.5);p.x=q.x;p.z=q.z;c.g.rotation.y=Math.atan2(dx,dz);c.ph+=dt*11;swing(c.g,c.ph,.9)}
   p.y=gy(p.x,p.z)}
  if(md<1.6)busted();else if(md>100){far+=dt;if(far>6){endWanted();toast('You escaped! The police lost you.')}}else far=0}

 if(job){const t=job.t[job.i];job.t.forEach((q,k)=>q.m.visible=k===job.i);
  if(Math.hypot(t.x-pl.x,t.z-pl.z)<3.2){t.prog+=dt;
   if(t.k==='trash'&&t.prop)t.prop.scale.setScalar(Math.max(.06,1-t.prog/t.w));
   if(!veh&&t.k&&t.prog>0){me.u.aL.rotation.x=me.u.aR.rotation.x=-1+Math.sin(t.prog*11)*.45}
   if(t.prog>=t.w){if(t.pay){money+=t.pay;toast('+$'+t.pay)}else toast(t.lab.replace('Pick up','Picked up'));
    scene.remove(t.m);if(t.prop)scene.remove(t.prop);job.i++;if(job.i>=job.t.length){const d=job.d;toast('Job complete! +'+d.xp+' XP');addXp(d.xp);clearJob()}}}
  else{t.prog=0;if(t.k==='trash'&&t.prop)t.prop.scale.setScalar(1)}}
 if(hunger<=0&&Math.random()<dt*.02)toast('You are starving! Buy food at the dhaba');

 const c=ctx(),dc=dctx(),nn=nearNpc(3.2),hi=$('hint');
 hi.textContent=!playing||menuOpen?'':c?'Tap Use: talk / shop':dc?(dc==='exit'?'Tap Drive: get off':parked.includes(dc)?'Tap Drive: ride your vehicle':'Tap Drive: take this vehicle (police will chase)'):nn?'Tap Ask: ask for money  |  Fight: punch':'';
 hi.style.opacity=hi.textContent?1:0;

 let tcd=veh?(veh.car?9:7.5):6.5;
 for(let s=0;s<9;s++){const x=pl.x+sn*Math.cos(pitch)*tcd,z=pl.z+cs*Math.cos(pitch)*tcd;let hit=false;
  for(const q of cols)if(q.b&&Math.abs(x-q.x)<q.hx+.6&&Math.abs(z-q.z)<q.hz+.6){hit=true;break}
  if(!hit)break;tcd-=.7;if(tcd<1.5){tcd=1.5;break}}
 cd+=(tcd-cd)*Math.min(1,dt*(tcd<cd?14:3));
 const ty=pl.y+1.5;cam.position.set(pl.x+sn*Math.cos(pitch)*cd,ty+Math.sin(pitch)*cd,pl.z+cs*Math.cos(pitch)*cd);cam.lookAt(pl.x,ty,pl.z);
 clouds.forEach(q=>{q.position.x+=dt*3;if(q.position.x>pl.x+450)q.position.x-=900;if(q.position.x<pl.x-450)q.position.x+=900;if(Math.abs(q.position.z-pl.z)>450)q.position.z=pl.z+R(-440,440)});
 env();hud();
}

/* ---------- HUD and minimap ---------- */
const mc=$('map').getContext('2d');
function hud(){
 $('money').textContent='$'+Math.floor(money).toLocaleString();
 $('lvt').textContent='LV '+level;$('xb').style.width=(xp/need(level)*100)+'%';
 const hh=Math.floor(hour),mm=Math.floor((hour%1)*60);$('clock').textContent=(hour>=6&&hour<18?'☀ ':'☾ ')+(hh%12||12)+':'+String(mm).padStart(2,'0')+(hh<12?' AM':' PM');
 $('pb').style.width=hp+'%';$('sb').style.width=pl.st+'%';$('sb').style.background=pl.tired?'#d9534f':'#f2c230';$('hb').style.width=hunger+'%';$('hb').style.background=hunger<20?'#d9534f':'#ff9f43';
 if(hw!==wanted){hw=wanted;$('wt').style.display=$('siren').style.display=wanted?'block':'none'}
 const cs=Math.cos(yaw),sn=Math.sin(yaw),s=.38,mp=(x,z)=>{const dx=x-pl.x,dz=z-pl.z;return[48+(dx*cs-dz*sn)*s,48+(dx*sn+dz*cs)*s]};
 const c=mc;c.setTransform(2,0,0,2,0,0);c.clearRect(0,0,96,96);c.save();c.beginPath();c.arc(48,48,48,0,7);c.clip();c.fillStyle='#22331f';c.fillRect(0,0,96,96);
 c.strokeStyle='#6a6a6a';c.lineWidth=RW*s;c.beginPath();
 for(let k=-3;k<=3;k++){let a=mp(-H-60,k*P),b=mp(H+60,k*P);c.moveTo(a[0],a[1]);c.lineTo(b[0],b[1]);a=mp(k*P,-H-60);b=mp(k*P,H+60);c.moveTo(a[0],a[1]);c.lineTo(b[0],b[1])}c.stroke();
 SITES.forEach(q=>{const p=mp(q.x,q.z);c.fillStyle=q.col;c.beginPath();c.arc(p[0],p[1],3,0,7);c.fill()});
 cops.forEach(q=>{const p=mp(q.g.position.x,q.g.position.z);c.fillStyle='#4d7cff';c.beginPath();c.arc(p[0],p[1],2.5,0,7);c.fill()});
 if(job){const t=job.t[job.i];let p=mp(t.x,t.z),dx=p[0]-48,dy=p[1]-48,l=Math.hypot(dx,dy);if(l>42){p[0]=48+dx*42/l;p[1]=48+dy*42/l}
  c.fillStyle='#fff';c.strokeStyle='#f2c230';c.lineWidth=2;c.beginPath();c.arc(p[0],p[1],4,0,7);c.fill();c.stroke();
  const d=Math.hypot(t.x-pl.x,t.z-pl.z),dxw=t.x-pl.x,dzw=t.z-pl.z;
  $('job').style.display='block';$('jt').textContent=job.d.n+': '+t.lab+'  '+Math.round(d)+' m'+(t.prog>0?'  '+Math.min(100,Math.round(t.prog/t.w*100))+'%':'');
  $('arr').style.transform='rotate('+Math.atan2(dxw*cs-dzw*sn,-(dxw*sn+dzw*cs))+'rad)'}
 else $('job').style.display='none';
 c.translate(48,48);const hx=Math.sin(pl.ry),hz=Math.cos(pl.ry);c.rotate(Math.atan2(hx*cs-hz*sn,-(hx*sn+hz*cs)));
 c.fillStyle='#fff';c.strokeStyle='#000';c.lineWidth=1;c.beginPath();c.moveTo(0,-7);c.lineTo(5,6);c.lineTo(-5,6);c.closePath();c.fill();c.stroke();c.restore()}

let last=0;
function loop(ts){requestAnimationFrame(loop);const dt=Math.min(.05,(ts-last)/1000||0);last=ts;update(dt);renderer.render(scene,cam)}
requestAnimationFrame(loop);
</script></body></html>
