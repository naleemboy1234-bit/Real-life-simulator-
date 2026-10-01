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

#start{background:rgba(14,16,20,.35)}
#vg{background:radial-gradient(ellipse at center,transparent 62%,rgba(0,0,0,.4))}
#bX{right:calc(var(--d)*2.4);bottom:calc(var(--d)*2.4);border-color:#ff9f43}
#mp{position:fixed;z-index:3;top:var(--top);right:calc(152px + env(safe-area-inset-right,0px));width:36px;height:36px;border-radius:8px;background:rgba(14,16,20,.55);border:1px solid rgba(255,255,255,.4);display:flex;align-items:center;justify-content:center;font-size:11px;font-weight:700}
#wpt{top:calc(var(--top) + 90px);left:50%;transform:translateX(-50%);font-size:13px;background:rgba(160,20,30,.75);padding:5px 12px;border-radius:6px;display:none;white-space:nowrap}
#bm{position:fixed;inset:0;z-index:11;background:rgba(8,10,14,.95);display:none;flex-direction:row;align-items:center;justify-content:center;gap:14px;padding:8px}
#bmc{border-radius:10px;border:2px solid #3a3f4a}
#bmp{display:flex;flex-direction:column;gap:8px;width:170px;font-size:13px}
#bmp button{font:inherit;font-weight:700;padding:9px 12px;border:0;border-radius:6px;background:#f2c230;color:#15171b}
#bmp #bmx{background:#2b303a;color:#fff}
</style></head><body>
<canvas id="g"></canvas>
<div id="vg" class="h" style="inset:0;pointer-events:none"></div>
<div id="siren" class="h"></div>
<div id="money" class="h">$0</div>
<div id="lv" class="h">LV 1<div class="bar"><i id="xb" style="width:0%"></i></div></div>
<div id="clock" class="h">08:00 AM</div>
<div id="bars" class="h">
  ENERGY <div class="bar" style="margin-bottom:4px"><i id="sb" style="width:100%"></i></div>
  HEALTH <div class="bar"><i id="hb" style="width:100%"></i></div>
</div>
<div id="wt" class="h">WANTED ★</div>
<canvas id="map" class="h"></canvas>
<div id="job" class="h"><span id="arr">➔</span> <span id="jt">GO TO WORK</span></div>
<div id="wpt" class="h">🎯 WAYPOINT</div>
<div id="hint" class="h">Press ACTION to interact</div>
<div id="toast" class="h"></div>

<div id="mb">⚙</div>
<div id="mp">🗺️</div>

<div id="joy"><div id="knob"></div></div>
<div id="pad">
  <div id="bJ" class="b">JUMP</div>
  <div id="bR" class="b">RUN</div>
  <div id="bA" class="b">ACT</div>
  <div id="bK" class="b">ATTACK</div>
  <div id="bF" class="b">FIRE</div>
  <div id="bD" class="b">DRIVE</div>
  <div id="bX" class="b">PHONE</div>
</div>

<div id="bm">
  <canvas id="bmc"></canvas>
  <div id="bmp">
    <div><strong>Big Map</strong></div>
    <div style="color:#aaa;font-size:11px">Tap anywhere on map to set waypoint</div>
    <button id="bmw">Clear Waypoint</button>
    <button id="bmx">Close Map</button>
  </div>
</div>

<div id="menu">
  <h2>Game Menu</h2>
  <div class="row">
    <div><strong>Player Stats</strong><small id="mst">Cash: $0 | Level: 1</small></div>
  </div>
  <div class="row">
    <div><strong>Sleep & Rest</strong><small>Restore full energy (+100)</small></div>
    <button id="msleep">Sleep</button>
  </div>
  <div class="row">
    <div><strong>Buy Health Care</strong><small>Restore full health ($50)</small></div>
    <button id="mheal">Heal</button>
  </div>
  <div class="row">
    <div><strong>Clear Wanted Level</strong><small>Bribe authorities ($200)</small></div>
    <button id="mbribe">Bribe</button>
  </div>
  <button class="close" id="mclose">Close Menu</button>
</div>

<div id="start">
  <div>
    <h1>REAL LIFE</h1>
    <p>Live, earn, drive, fight, and build your story in the city.</p>
  </div>
  <input type="text" id="nm" placeholder="Enter Character Name" maxlength="12" value="Player">
  <div class="sr">
    <button id="go">PLAY GAME</button>
  </div>
  <p class="s">Use touch controls on mobile or WASD / Arrow keys + Space on desktop.</p>
</div>

<div id="rot">Please rotate your device to landscape mode to play.</div>

<script>
(function(){
  const cv = document.getElementById('g');
  const ctx = cv.getContext('2d');
  const mcv = document.getElementById('map');
  const mctx = mcv.getContext('2d');
  const bmcv = document.getElementById('bmc');
  const bmctx = bmcv.getContext('2d');

  let W=0, H=0;
  function resize(){
    W = cv.width = window.innerWidth;
    H = cv.height = window.innerHeight;
    mcv.width = 96; mcv.height = 96;
  }
  window.addEventListener('resize', resize);
  resize();

  // State
  let gameRunning = false;
  let name = "Player";
  let money = 250;
  let xp = 0, level = 1, nextXp = 100;
  let stm = 100, maxStm = 100;
  let hp = 100, maxHp = 100;
  let wanted = 0;
  let hour = 8, min = 0, sec = 0;

  // World Config
  const WORLD_SIZE = 2400;
  let wayX = -1, wayY = -1;

  // Controls
  let stick = {x:0, y:0, active:false, id:null, sx:0, sy:0};
  let keys = {};
  let btnState = {run:false, jump:false, act:false, atk:false, fire:false, drive:false, phone:false};

  // Player
  const p = {
    x: 1200, y: 1200, r: 14, speed: 3.2,
    vx: 0, vy: 0,
    angle: 0, inVehicle: null,
    isDriving: false
  };

  // Entities
  let npcs = [];
  let vehicles = [];
  let buildings = [];

  // Generate World
  function initWorld(){
    buildings = [
      {x: 1000, y: 1100, w: 160, h: 120, name: "Home", color: "#4a69bd"},
      {x: 1300, y: 1100, w: 200, h: 140, name: "Job Office", color: "#6ab04c"},
      {x: 1000, y: 1350, w: 180, h: 130, name: "Bank", color: "#e1b12c"},
      {x: 1300, y: 1350, w: 220, h: 150, name: "Car Shop", color: "#e84118"},
      {x: 1600, y: 1200, w: 200, h: 200, name: "Hospital", color: "#c23616"}
    ];

    for(let i=0; i<30; i++){
      npcs.push({
        x: Math.random()*(WORLD_SIZE-200)+100,
        y: Math.random()*(WORLD_SIZE-200)+100,
        r: 12,
        vx: (Math.random()-0.5)*1.5,
        vy: (Math.random()-0.5)*1.5,
        hp: 40,
        type: Math.random()>0.25 ? 'civilian' : 'cop'
      });
    }

    for(let i=0; i<12; i++){
      vehicles.push({
        x: Math.random()*(WORLD_SIZE-300)+150,
        y: Math.random()*(WORLD_SIZE-300)+150,
        w: 48, h: 26, angle: Math.random()*Math.PI*2,
        speed: 0, maxSpeed: 7,
        color: ['#e74c3c','#3498db','#f1c40f','#2ecc71','#9b59b6'][i%5]
      });
    }
  }

  // UI Updates
  function updateUI(){
    document.getElementById('money').innerText = '$' + money;
    document.getElementById('lv').childNodes[0].nodeValue = 'LV ' + level;
    document.getElementById('xb').style.width = (xp/nextXp*100) + '%';
    document.getElementById('sb').style.width = (stm/maxStm*100) + '%';
    document.getElementById('hb').style.width = (hp/maxHp*100) + '%';
    document.getElementById('wt').style.display = wanted > 0 ? 'block' : 'none';
    document.getElementById('siren').style.display = wanted > 2 ? 'block' : 'none';
    
    let hStr = hour < 10 ? '0'+hour : hour;
    let mStr = min < 10 ? '0'+min : min;
    document.getElementById('clock').innerText = `${hStr}:${mStr} ${hour>=12?'PM':'AM'}`;

    document.getElementById('wpt').style.display = wayX >= 0 ? 'block' : 'none';
  }

  // Game Loop
  function tick(){
    if(!gameRunning) return;

    // Time cycle
    sec++;
    if(sec >= 60){ sec=0; min++; }
    if(min >= 60){ min=0; hour=(hour+1)%24; }

    // Controls Handling
    let dx = stick.x;
    let dy = stick.y;

    if(keys['KeyW'] || keys['ArrowUp']) dy = -1;
    if(keys['KeyS'] || keys['ArrowDown']) dy = 1;
    if(keys['KeyA'] || keys['ArrowLeft']) dx = -1;
    if(keys['KeyD'] || keys['ArrowRight']) dx = 1;

    let len = Math.hypot(dx, dy);
    if(len > 1){ dx /= len; dy /= len; }

    let moveSpeed = p.speed * (btnState.run && stm > 5 ? 1.6 : 1.0);
    if(btnState.run && len > 0.1 && stm > 0) stm = Math.max(0, stm - 0.15);
    else if(!btnState.run) stm = Math.min(maxStm, stm + 0.1);

    if(!p.isDriving){
      p.x += dx * moveSpeed;
      p.y += dy * moveSpeed;
      if(len > 0.1) p.angle = Math.atan2(dy, dx);
    } else if(p.inVehicle){
      let v = p.inVehicle;
      if(dy < 0) v.speed = Math.min(v.maxSpeed, v.speed + 0.2);
      else if(dy > 0) v.speed = Math.max(-v.maxSpeed*0.4, v.speed - 0.15);
      else v.speed *= 0.96;

      if(Math.abs(v.speed) > 0.2){
        if(dx < 0) v.angle -= 0.05 * (v.speed/v.maxSpeed);
        if(dx > 0) v.angle += 0.05 * (v.speed/v.maxSpeed);
      }

      v.x += Math.cos(v.angle) * v.speed;
      v.y += Math.sin(v.angle) * v.speed;
      p.x = v.x;
      p.y = v.y;
      p.angle = v.angle;
    }

    // World Bounds
    p.x = Math.max(20, Math.min(WORLD_SIZE-20, p.x));
    p.y = Math.max(20, Math.min(WORLD_SIZE-20, p.y));

    // Update NPCs
    npcs.forEach(n => {
      n.x += n.vx;
      n.y += n.vy;
      if(n.x < 50 || n.x > WORLD_SIZE-50) n.vx *= -1;
      if(n.y < 50 || n.y > WORLD_SIZE-50) n.vy *= -1;
    });

    render();
    renderMinimap();
    updateUI();

    requestAnimationFrame(tick);
  }

  // Drawing
  function render(){
    ctx.clearRect(0,0,W,H);
    ctx.save();
    ctx.translate(W/2 - p.x, H/2 - p.y);

    // Grid / Ground
    ctx.fillStyle = "#1e232d";
    ctx.fillRect(0,0,WORLD_SIZE,WORLD_SIZE);

    // Grid Lines
    ctx.strokeStyle = "#2a303d";
    ctx.lineWidth = 2;
    for(let x=0; x<WORLD_SIZE; x+=200){
      ctx.beginPath(); ctx.moveTo(x,0); ctx.lineTo(x,WORLD_SIZE); ctx.stroke();
    }
    for(let y=0; y<WORLD_SIZE; y+=200){
      ctx.beginPath(); ctx.moveTo(0,y); ctx.lineTo(WORLD_SIZE,y); ctx.stroke();
    }

    // Waypoint Line
    if(wayX >= 0){
      ctx.strokeStyle = "#ff4757";
      ctx.lineWidth = 3;
      ctx.setLineDash([8, 8]);
      ctx.beginPath();
      ctx.moveTo(p.x, p.y);
      ctx.lineTo(wayX, wayY);
      ctx.stroke();
      ctx.setLineDash([]);

      ctx.fillStyle = "#ff4757";
      ctx.beginPath();
      ctx.arc(wayX, wayY, 10, 0, Math.PI*2);
      ctx.fill();
    }

    // Buildings
    buildings.forEach(b => {
      ctx.fillStyle = b.color;
      ctx.fillRect(b.x, b.y, b.w, b.h);
      ctx.fillStyle = "#ffffff";
      ctx.font = "bold 14px sans-serif";
      ctx.textAlign = "center";
      ctx.fillText(b.name, b.x + b.w/2, b.y + b.h/2);
    });

    // Vehicles
    vehicles.forEach(v => {
      ctx.save();
      ctx.translate(v.x, v.y);
      ctx.rotate(v.angle);
      ctx.fillStyle = v.color;
      ctx.fillRect(-v.w/2, -v.h/2, v.w, v.h);
      ctx.fillStyle = "#111";
      ctx.fillRect(-v.w/2+4, -v.h/2-2, 8, 4);
      ctx.fillRect(v.w/2-12, -v.h/2-2, 8, 4);
      ctx.fillRect(-v.w/2+4, v.h/2-2, 8, 4);
      ctx.fillRect(v.w/2-12, v.h/2-2, 8, 4);
      ctx.restore();
    });

    // NPCs
    npcs.forEach(n => {
      ctx.fillStyle = n.type === 'cop' ? '#3498db' : '#e67e22';
      ctx.beginPath();
      ctx.arc(n.x, n.y, n.r, 0, Math.PI*2);
      ctx.fill();
    });

    // Player
    if(!p.isDriving){
      ctx.save();
      ctx.translate(p.x, p.y);
      ctx.rotate(p.angle);
      ctx.fillStyle = "#f2c230";
      ctx.beginPath();
      ctx.arc(0, 0, p.r, 0, Math.PI*2);
      ctx.fill();
      // Direction Pointer
      ctx.fillStyle = "#fff";
      ctx.fillRect(0, -3, p.r + 4, 6);
      ctx.restore();
    }

    ctx.restore();
  }

  function renderMinimap(){
    mctx.clearRect(0,0,96,96);
    mctx.save();
    mctx.beginPath();
    mctx.arc(48,48,46,0,Math.PI*2);
    mctx.clip();

    mctx.fillStyle = "#11141a";
    mctx.fillRect(0,0,96,96);

    let scale = 96 / 600;
    mctx.translate(48 - p.x*scale, 48 - p.y*scale);

    buildings.forEach(b => {
      mctx.fillStyle = b.color;
      mctx.fillRect(b.x*scale, b.y*scale, b.w*scale, b.h*scale);
    });

    if(wayX >= 0){
      mctx.fillStyle = "#ff4757";
      mctx.beginPath();
      mctx.arc(wayX*scale, wayY*scale, 4, 0, Math.PI*2);
      mctx.fill();
    }

    mctx.fillStyle = "#00ffcc";
    mctx.beginPath();
    mctx.arc(p.x*scale, p.y*scale, 3, 0, Math.PI*2);
    mctx.fill();

    mctx.restore();
  }

  // Input Listeners
  window.addEventListener('keydown', e => keys[e.code] = true);
  window.addEventListener('keyup', e => keys[e.code] = false);

  // Touch Joystick Setup
  const joy = document.getElementById('joy');
  const knob = document.getElementById('knob');

  joy.addEventListener('touchstart', e => {
    let t = e.changedTouches[0];
    stick.active = true;
    stick.id = t.identifier;
    let rect = joy.getBoundingClientRect();
    stick.sx = rect.left + rect.width/2;
    stick.sy = rect.top + rect.height/2;
    updateKnob(t.clientX, t.clientY);
  });

  joy.addEventListener('touchmove', e => {
    if(!stick.active) return;
    for(let t of e.changedTouches){
      if(t.identifier === stick.id){
        updateKnob(t.clientX, t.clientY);
      }
    }
  });

  const resetStick = () => {
    stick.active = false;
    stick.x = 0; stick.y = 0;
    knob.style.transform = 'translate(0px, 0px)';
  };

  joy.addEventListener('touchend', resetStick);
  joy.addEventListener('touchcancel', resetStick);

  function updateKnob(cx, cy){
    let dx = cx - stick.sx;
    let dy = cy - stick.sy;
    let maxR = 45;
    let dist = Math.hypot(dx, dy);
    if(dist > maxR){
      dx = (dx/dist)*maxR;
      dy = (dy/dist)*maxR;
    }
    knob.style.transform = `translate(${dx}px, ${dy}px)`;
    stick.x = dx / maxR;
    stick.y = dy / maxR;
  }

  // Action Buttons
  function bindBtn(id, key){
    let el = document.getElementById(id);
    if(!el) return;
    el.addEventListener('touchstart', e => { e.preventDefault(); btnState[key] = true; });
    el.addEventListener('touchend', e => { e.preventDefault(); btnState[key] = false; });
  }

  bindBtn('bR', 'run');
  bindBtn('bJ', 'jump');
  bindBtn('bA', 'act');
  bindBtn('bK', 'atk');
  bindBtn('bF', 'fire');

  document.getElementById('bD').addEventListener('click', () => {
    if(!p.isDriving){
      let v = vehicles.find(veh => Math.hypot(veh.x - p.x, veh.y - p.y) < 50);
      if(v){
        p.isDriving = true;
        p.inVehicle = v;
      }
    } else {
      p.isDriving = false;
      p.x += 35;
      p.inVehicle = null;
    }
  });

  // Menu Handling
  document.getElementById('mb').addEventListener('click', () => {
    document.getElementById('menu').style.display = 'block';
    document.getElementById('mst').innerText = `Cash: $${money} | Level: ${level}`;
  });

  document.getElementById('mclose').addEventListener('click', () => {
    document.getElementById('menu').style.display = 'none';
  });

  document.getElementById('msleep').addEventListener('click', () => {
    stm = maxStm;
    hour = (hour + 8) % 24;
    document.getElementById('menu').style.display = 'none';
  });

  document.getElementById('mheal').addEventListener('click', () => {
    if(money >= 50){
      money -= 50;
      hp = maxHp;
      document.getElementById('menu').style.display = 'none';
    }
  });

  document.getElementById('mbribe').addEventListener('click', () => {
    if(money >= 200 && wanted > 0){
      money -= 200;
      wanted = 0;
      document.getElementById('menu').style.display = 'none';
    }
  });

  // Big Map Handling
  document.getElementById('mp').addEventListener('click', () => {
    let bm = document.getElementById('bm');
    bm.style.display = 'flex';
    bmcv.width = Math.min(window.innerWidth - 180, 400);
    bmcv.height = bmcv.width;
    drawBigMap();
  });

  document.getElementById('bmx').addEventListener('click', () => {
    document.getElementById('bm').style.display = 'none';
  });

  document.getElementById('bmw').addEventListener('click', () => {
    wayX = -1; wayY = -1;
    drawBigMap();
  });

  bmcv.addEventListener('click', e => {
    let rect = bmcv.getBoundingClientRect();
    let cx = e.clientX - rect.left;
    let cy = e.clientY - rect.top;
    let scale = WORLD_SIZE / bmcv.width;
    wayX = cx * scale;
    wayY = cy * scale;
    drawBigMap();
  });

  function drawBigMap(){
    let bw = bmcv.width;
    bmctx.fillStyle = "#1e232d";
    bmctx.fillRect(0,0,bw,bw);

    let scale = bw / WORLD_SIZE;

    buildings.forEach(b => {
      bmctx.fillStyle = b.color;
      bmctx.fillRect(b.x*scale, b.y*scale, b.w*scale, b.h*scale);
    });

    if(wayX >= 0){
      bmctx.fillStyle = "#ff4757";
      bmctx.beginPath();
      bmctx.arc(wayX*scale, wayY*scale, 6, 0, Math.PI*2);
      bmctx.fill();
    }

    bmctx.fillStyle = "#00ffcc";
    bmctx.beginPath();
    bmctx.arc(p.x*scale, p.y*scale, 5, 0, Math.PI*2);
    bmctx.fill();
  }

  // Start Game
  document.getElementById('go').addEventListener('click', () => {
    name = document.getElementById('nm').value || "Player";
    document.getElementById('start').style.display = 'none';
    gameRunning = true;
    initWorld();
    requestAnimationFrame(tick);
  });

})();
</script>
</body>
</html>
