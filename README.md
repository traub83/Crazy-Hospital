<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Anne-Lise's World</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;}
body{background:#060610;font-family:'Courier New',monospace;overflow:hidden;height:100vh;display:flex;flex-direction:column;align-items:center;user-select:none;-webkit-user-select:none;}
#titleScreen{position:fixed;inset:0;background:#060610;display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:200;}
#titleScreen h1{font-size:clamp(2rem,6vw,4rem);color:#e8d5f0;text-shadow:0 0 24px #c084fc,0 0 50px #7c3aed;letter-spacing:.1em;text-align:center;line-height:1.15;margin-bottom:.25em;}
.sub{color:#7a5a9a;font-size:.75rem;letter-spacing:.35em;text-transform:uppercase;margin-bottom:1.4em;}
.instruct{color:#4a3a6a;font-size:.62rem;letter-spacing:.08em;text-align:center;line-height:2.1;margin-bottom:2.5em;max-width:300px;}
.instruct b{color:#c084fc;}
.start-btn{background:none;border:2px solid #c084fc;color:#e8d5f0;font-family:'Courier New',monospace;font-size:1rem;letter-spacing:.2em;padding:.75em 2.5em;cursor:pointer;text-transform:uppercase;transition:all .2s;box-shadow:0 0 18px #7c3aed44;}
.start-btn:hover{background:#7c3aed33;box-shadow:0 0 30px #c084fc88;}
.flicker{animation:flicker 5s infinite;}
@keyframes flicker{0%,94%,98%,100%{opacity:1;}96%{opacity:.3;}}
#gameWrap{width:100%;height:100vh;display:flex;flex-direction:column;align-items:center;}
#topBar{width:100%;max-width:700px;display:flex;align-items:center;justify-content:space-between;padding:4px 10px;background:#e8e8e0;border-bottom:2px solid #cc0000;flex-shrink:0;}
.gtitle{color:#cc0000;font-size:.58rem;letter-spacing:.2em;text-transform:uppercase;font-weight:bold;}
#timerBox{font-size:.95rem;font-weight:bold;letter-spacing:.1em;color:#007700;transition:color .4s,text-shadow .4s;}
#timerBox.warn{color:#cc6600;}
#timerBox.crit{color:#cc0000;animation:blink .5s infinite alternate;}
@keyframes blink{from{opacity:1;}to{opacity:.4;}}
#scoreBox{color:#444;font-size:.6rem;letter-spacing:.08em;}
#canvasWrap{position:relative;width:100%;max-width:700px;flex:1 1 0;overflow:hidden;background:#000;min-height:0;}
#gc{width:100%;height:100%;display:block;image-rendering:pixelated;image-rendering:crisp-edges;}
#roomLbl{position:absolute;top:6px;left:50%;transform:translateX(-50%);background:rgba(255,255,255,.85);color:#cc0000;font-size:.55rem;letter-spacing:.15em;text-transform:uppercase;padding:3px 10px;border-radius:3px;pointer-events:none;white-space:nowrap;border:1px solid #cc0000;font-weight:bold;}
#msgBox{position:absolute;bottom:6px;left:50%;transform:translateX(-50%);background:rgba(255,255,255,.92);border:2px solid #cc0000;color:#222;font-size:.68rem;padding:5px 14px;border-radius:4px;pointer-events:none;opacity:0;transition:opacity .3s;text-align:center;max-width:94%;white-space:pre-line;line-height:1.5;}
#dangerStrip{position:absolute;top:26px;left:0;right:0;display:flex;gap:3px;justify-content:center;flex-wrap:wrap;padding:0 4px;pointer-events:none;}
.dbadge{background:rgba(200,0,0,.9);border:1px solid #880000;color:#fff;font-size:.48rem;padding:2px 6px;border-radius:3px;animation:dbp .9s infinite alternate;font-weight:bold;}
@keyframes dbp{from{opacity:.8;}to{opacity:1;}}
#reaperVig{position:absolute;inset:0;background:radial-gradient(ellipse at center,transparent 35%,#2a000099 100%);pointer-events:none;opacity:0;transition:opacity .25s;}
#minimap{position:absolute;bottom:4px;right:4px;border:1px solid #2a1a3a;border-radius:3px;background:#00000099;}
#pillTray{width:100%;max-width:700px;background:#f0f0e8;border-top:2px solid #cc0000;display:flex;align-items:center;padding:5px 8px;gap:6px;flex-shrink:0;min-height:44px;}
.tlbl{color:#cc0000;font-size:.55rem;letter-spacing:.1em;white-space:nowrap;font-weight:bold;}
#pillsRow{display:flex;gap:6px;flex-wrap:wrap;flex:1;align-items:center;}
.tpill-wrap{display:flex;flex-direction:column;align-items:center;gap:2px;}
.tpill{width:30px;height:13px;border-radius:7px;border:2px solid rgba(0,0,0,.25);cursor:pointer;transition:transform .15s,box-shadow .15s,opacity .3s;}
.tpill:hover{transform:scale(1.15);}
.tpill.sel{border-color:#000;box-shadow:0 0 10px rgba(0,0,0,.5);transform:scale(1.2) translateY(-2px);}
.tpill.used{opacity:.15;pointer-events:none;}
.tpill-lbl{font-size:.4rem;color:#444;text-align:center;}
#controls{width:100%;max-width:700px;background:#e0e0d8;border-top:1px solid #c0c0b8;padding:6px 12px 8px;flex-shrink:0;}
.ctrl-row{display:flex;align-items:center;justify-content:space-between;}
.dpad{display:grid;grid-template-columns:48px 48px 48px;grid-template-rows:48px 48px 48px;gap:3px;}
.btn{background:#c8c8c0;border:1px solid #a0a098;border-radius:7px;color:#444;font-size:1.2rem;cursor:pointer;display:flex;align-items:center;justify-content:center;-webkit-tap-highlight-color:transparent;touch-action:manipulation;transition:background .1s,color .1s;-webkit-touch-callout:none;}
.btn:active,.btn.pr{background:#cc0000;color:#fff;border-color:#880000;}
.bf{grid-column:2;grid-row:1;}.bba{grid-column:2;grid-row:3;}.bl{grid-column:1;grid-row:2;}.br{grid-column:3;grid-row:2;}
.bc{grid-column:2;grid-row:2;font-size:.45rem;background:#b8b8b0;color:#888;}
.turn-col{display:flex;flex-direction:column;align-items:center;gap:4px;}
.turn-lbl{color:#888;font-size:.48rem;letter-spacing:.1em;text-transform:uppercase;}
.turn-row{display:flex;gap:6px;}
.bturn{width:56px;height:48px;font-size:1.1rem;}
.act-col{display:flex;flex-direction:column;align-items:center;gap:3px;}
.bgive{width:68px;height:68px;border-radius:50%;background:#cc0000;border:3px solid #880000;color:#fff;font-size:.55rem;letter-spacing:.07em;text-transform:uppercase;cursor:pointer;-webkit-tap-highlight-color:transparent;touch-action:manipulation;box-shadow:0 0 14px #cc000066;transition:all .15s;text-align:center;line-height:1.4;display:flex;align-items:center;justify-content:center;flex-direction:column;font-weight:bold;}
.bgive:active{background:#990000;box-shadow:0 0 24px #cc000099;transform:scale(.94);}
.endScr{display:none;position:fixed;inset:0;flex-direction:column;align-items:center;justify-content:center;z-index:300;text-align:center;gap:.8em;padding:20px;}
#winScr{background:#060610ee;}
#winScr h2{font-size:2rem;color:#44dd88;text-shadow:0 0 24px #44dd8844;}
#loseScr{background:#060005ee;}
#loseScr h2{font-size:2rem;color:#dd3333;text-shadow:0 0 24px #dd333388;animation:blink .9s infinite alternate;}
.skull{font-size:4rem;animation:flt 2s ease-in-out infinite;}
@keyframes flt{0%,100%{transform:translateY(0);}50%{transform:translateY(-14px);}}
.endScr p{color:#9a7abc;font-size:.82rem;}
.endScr .dlst{color:#cc4444;font-size:.72rem;margin-top:.3em;}
.endScr button{margin-top:.8em;background:none;font-family:'Courier New',monospace;font-size:.9rem;letter-spacing:.2em;padding:.6em 2em;cursor:pointer;text-transform:uppercase;}
#winScr button{color:#44dd88;border:2px solid #44dd88;}
#loseScr button{color:#dd3333;border:2px solid #dd3333;}
</style>
</head>
<body>
<div id="titleScreen">
  <h1 class="flicker">Anne-Lise's<br>World</h1>
  <div class="sub">Nurse on Duty — Psych Ward B</div>
  <div class="instruct">
    Walk up to patients &amp; press <b>GIVE MED</b><br>
    Match pill to their <b>hair color</b><br>
    Each patient has <b>90 seconds</b><br>
    <b>☠ Reaper</b> hunts after <b>15 sec</b><br>
    Patients <b>wander</b> — check minimap!
  </div>
  <button class="start-btn" onclick="startGame()">Begin Shift</button>
</div>
<div id="gameWrap" style="display:none">
  <div id="topBar">
    <div class="gtitle">Anne-Lise's World</div>
    <div id="timerBox">1:00</div>
    <div id="scoreBox">Saved: 0 / 6</div>
  </div>
  <div id="canvasWrap">
    <canvas id="gc"></canvas>
    <div id="roomLbl">Hallway</div>
    <div id="dangerStrip"></div>
    <div id="reaperVig"></div>
    <div id="msgBox"></div>
    <canvas id="minimap"></canvas>
  </div>
  <div id="pillTray">
    <div class="tlbl">PILLS</div>
    <div id="pillsRow"></div>
  </div>
  <div id="controls">
    <div class="ctrl-row">
      <div class="dpad">
        <div class="btn bf"  id="bf">▲</div>
        <div class="btn bba" id="bb">▼</div>
        <div class="btn bl"  id="bl">◄</div>
        <div class="btn br"  id="br">►</div>
        <div class="btn bc">MOVE</div>
      </div>
      <div class="act-col">
        <div class="bgive" id="bgive" onclick="giveAction()">GIVE<br>MED</div>
      </div>
      <div class="turn-col">
        <div class="turn-lbl">Turn</div>
        <div class="turn-row">
          <div class="btn bturn" id="btl">↺</div>
          <div class="btn bturn" id="btr">↻</div>
        </div>
      </div>
    </div>
  </div>
</div>
<div class="endScr" id="winScr">
  <h2>☀ Shift Complete!</h2><p>All patients are safe.</p>
  <p id="winTime"></p><button onclick="location.reload()">New Shift</button>
</div>
<div class="endScr" id="loseScr">
  <div class="skull">☠</div><h2>The Reaper Won</h2>
  <p id="loseMsg"></p><div class="dlst" id="deadLst"></div>
  <button onclick="location.reload()">Try Again</button>
</div>

<script>
// ═══════════════════════════════════════════════════════════════
//  MAP  60×60  — large ward with multiple wings, long corridors,
//  side rooms, bathrooms, common area, chapel, storage rooms
// ═══════════════════════════════════════════════════════════════
const MW=60,MH=60;
const M=new Uint8Array(MW*MH);
M.fill(1); // start as all walls

function ms(x,y,v){if(x>=0&&x<MW&&y>=0&&y<MH)M[y*MW+x]=v;}
function mg(x,y){if(x<0||x>=MW||y<0||y>=MH)return 1;return M[y*MW+x];}
function isW(fx,fy){return mg(Math.floor(fx),Math.floor(fy))===1;}
function carve(x0,y0,x1,y1){for(let y=y0;y<=y1;y++)for(let x=x0;x<=x1;x++)ms(x,y,0);}

// ── MAIN HORIZONTAL CORRIDOR  rows 26-29, cols 1-58
carve(1,26,58,29);

// ── NORTH VERTICAL CORRIDOR  cols 28-31, rows 1-26
carve(28,1,31,26);

// ── SOUTH VERTICAL CORRIDOR  cols 28-31, rows 29-56
carve(28,29,31,56);

// ── EAST VERTICAL CORRIDOR  cols 50-53, rows 10-50
carve(50,10,53,50);

// ── WEST VERTICAL CORRIDOR  cols 6-9, rows 10-50
carve(6,10,9,50);

// ── NURSE STATION  (NW)  cols 1-22, rows 1-20
carve(1,1,22,20);
// connect to N corridor
carve(22,10,28,13);
// connect to main hall
carve(10,20,13,26);

// ── ROOM A (NE common room)  cols 34-58, rows 1-20
carve(34,1,58,20);
// connect to N corridor
carve(31,8,34,11);
// connect to E corridor
carve(50,14,54,17);

// ── ROOM B  cols 1-22, rows 32-50
carve(1,32,22,50);
carve(6,29,9,32);   // door from W corridor / main hall
carve(10,50,13,56); // south passage

// ── ROOM C  cols 34-46, rows 32-50
carve(34,32,46,50);
carve(37,29,40,32);

// ── ROOM D  cols 34-58, rows 54-58  (long south room)
carve(34,53,58,58);
carve(46,50,49,54);

// ── ROOM E  cols 1-22, rows 53-58  (south wing)
carve(1,53,22,58);
carve(10,50,13,54);

// ── CHAPEL  cols 14-26, rows 32-50
carve(14,32,26,50);
carve(18,29,21,32);
carve(14,45,18,50); // chapel apse

// ── STORAGE rooms along east corridor
carve(54,12,58,20);
carve(54,24,58,32);
carve(54,36,58,44);
carve(54,47,58,55);
// doorways into storage
carve(53,14,54,16);
carve(53,26,54,28);
carve(53,38,54,40);
carve(53,49,54,51);

// ── BATHROOMS along west corridor
carve(1,32,5,38);
carve(1,41,5,47);
carve(6,34,6,36);
carve(6,43,6,45);

// ── SMALL ALCOVES along main corridor
carve(16,22,20,25);
carve(38,22,42,25);
carve(16,30,20,33);
carve(38,30,42,33);

// ── CONNECTING PASSAGES
carve(22,40,28,43);  // B to chapel
carve(46,40,50,43);  // C to E corridor
carve(28,53,31,58);  // S corridor to south wing

// Re-enforce outer border
for(let x=0;x<MW;x++){ms(x,0,1);ms(x,MH-1,1);}
for(let y=0;y<MH;y++){ms(0,y,1);ms(MW-1,y,1);}

// ── ROOM NAME LOOKUP
function getRoomName(px,py){
  const x=Math.floor(px),y=Math.floor(py);
  if(x>=1&&x<=22&&y>=1&&y<=20) return'Nurse Station';
  if(x>=1&&x<=58&&y>=26&&y<=29) return'Main Corridor';
  if(x>=28&&x<=31&&y>=1&&y<=58) return'North-South Hall';
  if(x>=6&&x<=9&&y>=10&&y<=50) return'West Corridor';
  if(x>=50&&x<=53&&y>=10&&y<=50) return'East Corridor';
  if(x>=34&&x<=58&&y>=1&&y<=20) return'Common Room';
  if(x>=1&&x<=22&&y>=32&&y<=50) return'Room B — Ward';
  if(x>=34&&x<=46&&y>=32&&y<=50) return'Room C — Ward';
  if(x>=14&&x<=26&&y>=32&&y<=50) return'Chapel';
  if(x>=1&&x<=22&&y>=53&&y<=58) return'Room E — South Wing';
  if(x>=34&&x<=58&&y>=53&&y<=58) return'Room D — South Wing';
  if(x>=54&&x<=58) return'Storage';
  if(x>=1&&x<=5) return'Bathroom';
  return'Corridor';
}

// ═══════════════════════════════════════════════════════════════
//  PATHFINDING  — simple BFS for reaper wall avoidance
// ═══════════════════════════════════════════════════════════════
function bfsPath(sx,sy,ex,ey){
  const gx=Math.floor(sx),gy=Math.floor(sy);
  const tx=Math.floor(ex),ty=Math.floor(ey);
  if(gx===tx&&gy===ty)return null;
  const visited=new Uint8Array(MW*MH);
  const parent=new Int16Array(MW*MH).fill(-1);
  const queue=[];
  const start=gy*MW+gx;
  visited[start]=1;queue.push(start);
  const dirs=[[1,0],[-1,0],[0,1],[0,-1],[1,1],[1,-1],[-1,1],[-1,-1]];
  let head=0;
  while(head<queue.length){
    const cur=queue[head++];
    const cx=cur%MW,cy=Math.floor(cur/MW);
    if(cx===tx&&cy===ty){
      // trace back 1-2 steps to get next move dir
      let p=cur;
      let prev=p;
      for(let i=0;i<2;i++){if(parent[p]!==-1){prev=p;p=parent[p];}else break;}
      const nx=prev%MW,ny=Math.floor(prev/MW);
      return{x:nx+.5,y:ny+.5};
    }
    for(const [dx,dy] of dirs){
      const nx=cx+dx,ny=cy+dy;
      if(nx<0||nx>=MW||ny<0||ny>=MH)continue;
      // for diagonal, check both cardinal neighbors are open
      if(dx!==0&&dy!==0){if(mg(cx+dx,cy)===1||mg(cx,cy+dy)===1)continue;}
      if(mg(nx,ny)===1)continue;
      const ni=ny*MW+nx;
      if(visited[ni])continue;
      visited[ni]=1;parent[ni]=cur;queue.push(ni);
    }
  }
  return null; // no path
}

// ═══════════════════════════════════════════════════════════════
//  PATIENTS
// ═══════════════════════════════════════════════════════════════
const HAIRS=[
  {name:'Blonde',hex:'#f0c830'},
  {name:'Red',   hex:'#cc2211'},
  {name:'Blue',  hex:'#2277dd'},
  {name:'Green', hex:'#22aa44'},
  {name:'Purple',hex:'#9933bb'},
  {name:'White', hex:'#d8d8ee'},
];
const PNAMES=['Margot','Elise','Theodore','Ruth','Damien','Simone'];
// Start positions spread across the ward
const PSTARTS=[
  {x:10,y:5},   // nurse station area
  {x:45,y:10},  // common room
  {x:8,y:40},   // room B
  {x:20,y:42},  // chapel
  {x:39,y:40},  // room C
  {x:42,y:56},  // room D south
];

const PTIMER=90,RHUNT=15;
let patients=[],savedCt=0,deadCt=0,gameOver=false,gameStartT=0;

function initPatients(){
  patients=PSTARTS.map((s,i)=>({
    x:s.x+.5,y:s.y+.5,
    name:PNAMES[i],hair:HAIRS[i],
    medGiven:false,dead:false,
    timeLeft:PTIMER,
    // wander state
    wAngle:Math.random()*Math.PI*2,
    wTimer:0,wSpeed:.018,
    wPause:0,
  }));
}

const WANDER_SPEED=0.022;
const WANDER_CHANGE=2.5; // seconds between direction changes

function updatePatients(dt){
  patients.forEach(p=>{
    if(p.dead||p.medGiven)return;
    // wander
    p.wPause-=dt;
    if(p.wPause<=0){
      p.wTimer-=dt;
      if(p.wTimer<=0){
        // pick new random direction
        p.wAngle=Math.random()*Math.PI*2;
        p.wTimer=1+Math.random()*WANDER_CHANGE;
        p.wPause=0;
      }
      const nx=p.x+Math.cos(p.wAngle)*WANDER_SPEED;
      const ny=p.y+Math.sin(p.wAngle)*WANDER_SPEED;
      // collide with walls — if blocked pick new angle
      if(!isW(nx,p.y)&&!isW(nx,ny)){p.x=nx;}
      else{p.wAngle=Math.random()*Math.PI*2;p.wTimer=.5;}
      if(!isW(p.x,ny)&&!isW(nx,ny)){p.y=ny;}
      else{p.wAngle=Math.random()*Math.PI*2;p.wTimer=.5;}
    }
  });
}

// ═══════════════════════════════════════════════════════════════
//  GRIM REAPER  — uses BFS pathfinding, can't walk through walls
// ═══════════════════════════════════════════════════════════════
let reaper={
  x:30,y:27.5,  // starts centre of main corridor
  bobT:0,
  target:null,
  speed:0.025,
  pathTarget:null, // BFS next waypoint
  pathTimer:0,     // recompute every N seconds
};

function updateReaper(dt){
  reaper.bobT+=dt*2.5;
  reaper.pathTimer-=dt;

  // find most-urgent hunted patient
  let best=null,bestT=9999;
  patients.forEach(p=>{
    if(p.dead||p.medGiven)return;
    const elapsed=PTIMER-p.timeLeft;
    if(elapsed>=RHUNT&&p.timeLeft<bestT){bestT=p.timeLeft;best=p;}
  });
  reaper.target=best;

  if(best){
    // recompute BFS path periodically or when we reach waypoint
    const wpDist=reaper.pathTarget?
      Math.hypot(reaper.x-reaper.pathTarget.x,reaper.y-reaper.pathTarget.y):999;

    if(reaper.pathTimer<=0||wpDist<0.6||!reaper.pathTarget){
      const next=bfsPath(reaper.x,reaper.y,best.x,best.y);
      reaper.pathTarget=next||{x:best.x,y:best.y};
      reaper.pathTimer=0.4; // recompute every 0.4s
    }

    const tx=reaper.pathTarget.x,ty=reaper.pathTarget.y;
    const dx=tx-reaper.x,dy=ty-reaper.y;
    const d=Math.hypot(dx,dy);
    const spd=reaper.speed*(1+(PTIMER-best.timeLeft)/PTIMER*1.8);
    if(d>0.1){
      const nx=reaper.x+dx/d*spd;
      const ny=reaper.y+dy/d*spd;
      if(!isW(nx,reaper.y))reaper.x=nx;
      if(!isW(reaper.x,ny))reaper.y=ny;
    }

    // kill if touching patient
    const killDist=Math.hypot(reaper.x-best.x,reaper.y-best.y);
    if(killDist<0.6&&!best.dead){
      best.dead=true;deadCt++;
      reaper.pathTarget=null;reaper.pathTimer=0;
      showMsg(`☠  The Reaper claimed ${best.name}!`);
      updScore();if(savedCt+deadCt===6)endGame();
    }

    // red vignette near player
    const rd=Math.hypot(reaper.x-player.x,reaper.y-player.y);
    document.getElementById('reaperVig').style.opacity=Math.max(0,Math.min(.75,(5-rd)/5)).toFixed(2);
  } else {
    // no target — drift back to main corridor centre
    reaper.pathTimer-=dt;
    if(reaper.pathTimer<=0){
      const next=bfsPath(reaper.x,reaper.y,30,27.5);
      reaper.pathTarget=next||{x:30,y:27.5};
      reaper.pathTimer=0.6;
    }
    if(reaper.pathTarget){
      const dx=reaper.pathTarget.x-reaper.x,dy=reaper.pathTarget.y-reaper.y;
      const d=Math.hypot(dx,dy);
      if(d>0.3){
        const nx=reaper.x+dx/d*.012;const ny=reaper.y+dy/d*.012;
        if(!isW(nx,reaper.y))reaper.x=nx;
        if(!isW(reaper.x,ny))reaper.y=ny;
      }
    }
    document.getElementById('reaperVig').style.opacity=0;
  }
}

// ═══════════════════════════════════════════════════════════════
//  RAYCASTER  — DDA
// ═══════════════════════════════════════════════════════════════
const gc=document.getElementById('gc');
const gctx=gc.getContext('2d');
const RW=480,RH=270;
gc.width=RW;gc.height=RH;
let zBuf=new Float32Array(RW);
const FOV=Math.PI/2.2;

// Red-cross wall decoration positions (map tile coords where cross appears)
// Each entry: {x, y} of wall tile that shows a cross on its face
const CROSS_WALLS=[
  {x:1,y:10},{x:1,y:5},{x:22,y:8},   // nurse station walls
  {x:8,y:26},{x:20,y:26},             // south side of main corridor
  {x:38,y:26},{x:50,y:26},
  {x:28,y:15},{x:28,y:40},            // N-S corridor walls
];
const CROSS_SET=new Set(CROSS_WALLS.map(c=>c.y*100+c.x));

function wallRgb(mx,my,side,rayHitU){
  // All walls are bright clinical white — slightly warmer on E/W faces
  const sh = side===0 ? 0.78 : 1.0;
  const base = 245;
  return [base*sh|0, base*sh|0, (base-5)*sh|0];
}

// Check if a ray hit lands on a red-cross wall tile
function isCrossWall(mx,my){
  return CROSS_SET.has(my*100+mx);
}

// Draw a red cross stripe at a given screen column, given wall slice params
function drawCrossOnCol(col, top, wh, perp, hitU){
  // hitU = fractional position along wall face (0..1)
  // Cross occupies middle third of wall height, centered horizontally
  const crossW=0.25, crossH=0.45; // fraction of wall face
  // Is this column within the cross horizontal band?
  const inHoriz = hitU>=(0.5-crossW/2) && hitU<=(0.5+crossW/2);
  const midTop  = top + wh*(0.5-crossH/2)|0;
  const midH    = wh*crossH|0;
  const armTop  = top + wh*(0.5-crossW/2)|0;
  const armH    = wh*crossW|0;
  const shade   = Math.max(.2,Math.min(1,1.1-perp/26));

  if(inHoriz){
    // vertical bar of cross
    gctx.fillStyle=`rgba(210,${20*shade|0},${20*shade|0},${shade*.85})`;
    gctx.fillRect(col, midTop, 1, midH);
  } else {
    // horizontal bar of cross (only in the arm row)
    if(hitU>=(0.5-crossH/2) && hitU<=(0.5+crossH/2)){
      gctx.fillStyle=`rgba(210,${20*shade|0},${20*shade|0},${shade*.85})`;
      gctx.fillRect(col, armTop, 1, armH);
    }
  }
}

function castRay(px,py,ang){
  const rdx=Math.cos(ang),rdy=Math.sin(ang);
  let mx=Math.floor(px),my=Math.floor(py);
  const sx=rdx>=0?1:-1,sy=rdy>=0?1:-1;
  const ddx=Math.abs(1/rdx),ddy=Math.abs(1/rdy);
  let sdx=rdx>=0?(mx+1-px)*ddx:(px-mx)*ddx;
  let sdy=rdy>=0?(my+1-py)*ddy:(py-my)*ddy;
  let side=0;
  for(let i=0;i<120;i++){
    if(sdx<sdy){sdx+=ddx;mx+=sx;side=0;}
    else{sdy+=ddy;my+=sy;side=1;}
    if(mg(mx,my)===1)break;
  }
  const dist=side===0?(mx-px+(1-sx)/2)/rdx:(my-py+(1-sy)/2)/rdy;
  // compute fractional hit position along wall face (for cross pattern)
  let hitU;
  if(side===0){ hitU=(py+dist*rdy)%1; if(hitU<0)hitU+=1; }
  else         { hitU=(px+dist*rdx)%1; if(hitU<0)hitU+=1; }
  return{dist:Math.max(.05,dist),mx,my,side,hitU};
}

// ── DESK sprite positions in nurse station ──
const DESKS=[
  {x:5.5,y:8.5},{x:8.5,y:5.5},{x:14.5,y:5.5},
];

function drawScene(px,py,ang){
  // Ceiling — bright hospital white/off-white
  const cg=gctx.createLinearGradient(0,0,0,RH/2);
  cg.addColorStop(0,'#e8e8e0');cg.addColorStop(1,'#d0d0c8');
  gctx.fillStyle=cg;gctx.fillRect(0,0,RW,RH/2);
  // Floor — bright clinical yellow
  const fg=gctx.createLinearGradient(0,RH/2,0,RH);
  fg.addColorStop(0,'#d4b800');fg.addColorStop(1,'#a89000');
  gctx.fillStyle=fg;gctx.fillRect(0,RH/2,RW,RH/2);

  // Wall columns
  for(let col=0;col<RW;col++){
    const ray=ang-FOV/2+(col/RW)*FOV;
    const h=castRay(px,py,ray);
    const perp=Math.max(.05,h.dist*Math.cos(ray-ang));
    const wh=Math.min(RH*2.5,RH/perp|0);
    const top=(RH-wh)/2|0;
    const shade=Math.max(.12,Math.min(1,1.15-perp/26));
    const [wr,wg,wb]=wallRgb(h.mx,h.my,h.side,h.hitU);
    gctx.fillStyle=`rgb(${wr*shade|0},${wg*shade|0},${wb*shade|0})`;
    gctx.fillRect(col,top,1,wh);
    // Red cross overlay on designated wall tiles
    if(isCrossWall(h.mx,h.my)){
      drawCrossOnCol(col,top,wh,perp,h.hitU);
    }
    zBuf[col]=perp;
  }

  // collect & sort sprites back-to-front
  const sprites=[];
  patients.forEach(p=>{
    if(p.dead)return;
    const dx=p.x-px,dy=p.y-py;
    sprites.push({t:'p',p,dx,dy,d2:dx*dx+dy*dy});
  });
  // Desks as sprites
  DESKS.forEach(d=>{
    const dx=d.x-px,dy=d.y-py;
    sprites.push({t:'desk',dx,dy,d2:dx*dx+dy*dy,ox:d.x,oy:d.y});
  });
  const rx=reaper.x,ry=reaper.y+Math.sin(reaper.bobT)*.25;
  const rdx=rx-px,rdy=ry-py;
  sprites.push({t:'r',dx:rdx,dy:rdy,d2:rdx*rdx+rdy*rdy,rx,ry});
  sprites.sort((a,b)=>b.d2-a.d2);

  sprites.forEach(s=>{
    const dist=Math.sqrt(s.d2);
    if(dist<.2||dist>30)return;
    let rel=Math.atan2(s.dy,s.dx)-ang;
    while(rel>Math.PI)rel-=Math.PI*2;
    while(rel<-Math.PI)rel+=Math.PI*2;
    if(Math.abs(rel)>FOV*.72)return;
    const sx=(rel/(FOV/2)+1)*RW/2|0;
    if(s.t==='p')drawPatSprite(s.p,dist,sx);
    else if(s.t==='desk')drawDeskSprite(dist,sx);
    else drawReaperSprite(dist,sx);
  });
}

function drawDeskSprite(dist,cx){
  if(dist>10)return;
  const shade=Math.max(.15,Math.min(1,1.1-dist/10));
  // Desk is low and wide — sits on the floor
  const h=Math.min(RH,RH/dist*.7|0);
  const w=Math.max(6,h*1.6|0);
  // Position desk bottom at floor line
  const floorY=RH/2+h/2|0;
  const top=floorY-h;
  const lft=cx-(w/2|0);

  for(let c=lft;c<lft+w;c++){
    if(c<0||c>=RW||dist>zBuf[c]+.05)continue;
    const t=(c-lft)/w;
    // Desk surface (top strip) — light brown
    gctx.fillStyle=`rgb(${155*shade|0},${115*shade|0},${75*shade|0})`;
    gctx.fillRect(c,top,1,Math.max(2,h*.18|0));
    // Desk body — darker brown
    gctx.fillStyle=`rgb(${120*shade|0},${88*shade|0},${55*shade|0})`;
    gctx.fillRect(c,top+Math.max(2,h*.18|0),1,h*.82|0);
    // Computer monitor on top-center
    if(t>.25&&t<.75){
      const monTop=top-Math.max(3,h*.35|0);
      const monH=Math.max(3,h*.35|0);
      // monitor body (grey)
      gctx.fillStyle=`rgb(${60*shade|0},${65*shade|0},${70*shade|0})`;
      gctx.fillRect(c,monTop,1,monH);
      // screen (blue-ish glow)
      if(t>.3&&t<.7){
        gctx.fillStyle=`rgb(${20*shade|0},${80*shade|0},${160*shade|0})`;
        gctx.fillRect(c,monTop+1,1,monH-2);
      }
    }
    // Legs
    if(t<.08||t>.92){
      gctx.fillStyle=`rgb(${80*shade|0},${60*shade|0},${40*shade|0})`;
      gctx.fillRect(c,top+h*.82|0,1,h*.18|0);
    }
    zBuf[c]=dist;
  }
  // "NURSE STATION" label above desk when close
  if(dist<5){
    gctx.save();
    const fs=Math.max(6,Math.min(11,10/dist*1.5|0));
    gctx.font=`bold ${fs}px Courier New`;
    gctx.fillStyle=`rgba(200,30,30,${Math.min(1,(5-dist)/3)})`;
    gctx.textAlign='center';
    gctx.fillText('NURSE STATION',cx,top-6);
    gctx.restore();
  }
}

function drawPatSprite(p,dist,cx){
  const shade=Math.max(.1,Math.min(1,1.1-dist/22));
  const h=Math.min(RH*2,RH/dist*1.8|0);
  const w=Math.max(4,h*.55|0);
  const top=(RH-h)/2|0;
  const lft=cx-(w/2|0);
  const urg=p.medGiven?0:Math.max(0,1-p.timeLeft/PTIMER);

  // gown
  const gr=(215-urg*90)*shade|0,gg=(210-urg*70)*shade|0,gb=225*shade|0;
  for(let c=lft;c<lft+w;c++){
    if(c<0||c>=RW||dist>zBuf[c]+.05)continue;
    gctx.fillStyle=`rgb(${gr},${gg},${gb})`;
    gctx.fillRect(c,top+h*.3|0,1,h*.7|0);
    if(c===lft||c===lft+w-1){gctx.fillStyle='rgba(0,0,0,.3)';gctx.fillRect(c,top+h*.3|0,1,h*.7|0);}
    zBuf[c]=dist;
  }
  // head skin
  const hh=Math.max(3,h*.22|0),hw=Math.max(3,w*.68|0);
  const hl=cx-(hw/2|0),ht=top+(h*.06|0);
  const sr=200*shade|0,sg=164*shade|0,sb=128*shade|0;
  for(let c=hl;c<hl+hw;c++){
    if(c<0||c>=RW||dist>zBuf[c]+.05)continue;
    gctx.fillStyle=`rgb(${sr},${sg},${sb})`;
    gctx.fillRect(c,ht,1,hh);
  }
  // hair — colorful and tall
  const hr=hexC(p.hair.hex,0),hg2=hexC(p.hair.hex,1),hb=hexC(p.hair.hex,2);
  const hairH=Math.max(3,hh*.95|0);
  for(let c=hl-1;c<hl+hw+1;c++){
    if(c<0||c>=RW)continue;
    gctx.fillStyle=`rgb(${hr*shade|0},${hg2*shade|0},${hb*shade|0})`;
    gctx.fillRect(c,ht-hairH,1,hairH+1);
  }
  // eyes
  if(dist<9){
    const ey=ht+(hh*.45|0),eo=Math.max(1,hw*.2|0);
    gctx.fillStyle=`rgba(20,15,30,${shade})`;
    gctx.fillRect(cx-eo-1,ey,2,2);gctx.fillRect(cx+eo-1,ey,2,2);
  }
  // timer ring + name
  if(!p.medGiven){
    const ratio=p.timeLeft/PTIMER;
    const rad=Math.max(5,12/dist*1.6|0);
    const ry2=ht-hairH-rad-2;
    gctx.save();
    gctx.strokeStyle='#2a2a3a';gctx.lineWidth=2.5;
    gctx.beginPath();gctx.arc(cx,ry2,rad,0,Math.PI*2);gctx.stroke();
    gctx.strokeStyle=ratio>.5?'#44dd88':ratio>.25?'#ffaa22':'#ff3333';
    gctx.lineWidth=2.5;
    gctx.beginPath();gctx.arc(cx,ry2,rad,-Math.PI/2,-Math.PI/2+ratio*Math.PI*2);gctx.stroke();
    gctx.restore();
    if(dist<12){
      gctx.save();
      const fs=Math.max(7,Math.min(13,18/dist*1.4|0));
      gctx.font=`bold ${fs}px Courier New`;gctx.textAlign='center';
      gctx.fillStyle=`rgba(255,238,255,${Math.min(1,(12-dist)/8)})`;
      gctx.fillText(p.name,cx,ry2-rad-2);
      gctx.restore();
    }
  } else {
    gctx.save();
    gctx.font=`bold ${Math.max(8,16/dist*1.4|0)}px Courier New`;
    gctx.fillStyle=`rgba(68,221,136,${Math.min(1,1.3-dist/8)})`;
    gctx.textAlign='center';gctx.fillText('✓',cx,top-2);
    gctx.restore();
  }
}

function drawReaperSprite(dist,cx){
  const shade=Math.max(.1,Math.min(.88,1-dist/24));
  const h=Math.min(RH*2.2,RH/dist*2.1|0);
  const w=Math.max(3,h*.48|0);
  const top=(RH-h)/2|0-Math.floor(h*.06);
  const lft=cx-(w/2|0);

  for(let c=lft;c<lft+w;c++){
    if(c<0||c>=RW||dist>zBuf[c]+.05)continue;
    const t=(c-lft)/w,tap=Math.sin(t*Math.PI);
    if(tap<.07)continue;
    gctx.fillStyle=`rgba(5,2,12,${(shade*tap+.08).toFixed(2)})`;
    gctx.fillRect(c,top+h*.2|0,1,h*.8|0);
    zBuf[c]=dist;
  }
  const hw2=w*.58|0,hl2=cx-(hw2/2|0);
  for(let c=hl2;c<hl2+hw2;c++){
    if(c<0||c>=RW)continue;
    gctx.fillStyle=`rgba(4,1,10,${shade*.9})`;
    gctx.fillRect(c,top,1,h*.3|0);
  }
  // glowing eyes
  const ey=top+(h*.12|0),eo=Math.max(1,w*.14|0);
  const gl=.5+.5*Math.sin(reaper.bobT*2.8);
  gctx.save();gctx.shadowColor='#ff1111';gctx.shadowBlur=10*shade;
  gctx.fillStyle=`rgba(255,${40+30*gl|0},${40+30*gl|0},${Math.min(1,shade*gl+.25)})`;
  gctx.fillRect(cx-eo-2,ey,3,3);gctx.fillRect(cx+eo-1,ey,3,3);
  gctx.restore();
  // scythe
  if(dist<12){
    const sc=lft+w;
    if(sc>=0&&sc<RW){
      gctx.fillStyle=`rgba(145,138,158,${shade*.65})`;
      gctx.fillRect(sc,top+(h*.04|0),2,h*.88|0);
      const bladeW=w*.38|0;
      for(let i=0;i<bladeW;i++){
        const bc=sc-i;if(bc<0||bc>=RW)continue;
        gctx.fillStyle=`rgba(198,192,212,${(1-i/bladeW)*shade*.75})`;
        gctx.fillRect(bc,top+(h*.04|0),1,h*.12|0);
      }
    }
  }
  if(dist<8){
    gctx.save();
    gctx.font=`bold ${Math.max(7,14/dist|0)}px Courier New`;
    gctx.fillStyle=`rgba(210,50,50,${Math.min(1,(8-dist)/5)})`;
    gctx.textAlign='center';gctx.fillText('☠',cx,top-3);
    gctx.restore();
  }
}

function hexC(h,ch){
  return parseInt(h.slice(1+ch*2,3+ch*2),16);
}

// ═══════════════════════════════════════════════════════════════
//  MINIMAP
// ═══════════════════════════════════════════════════════════════
const mm=document.getElementById('minimap');
const mmx=mm.getContext('2d');
const MMS=110; // minimap canvas size
mm.width=MMS;mm.height=MMS;
const CS=MMS/MW; // cell size

function drawMinimap(px,py,ang){
  mmx.fillStyle='#2a2000';mmx.fillRect(0,0,MMS,MMS);
  for(let y=0;y<MH;y++)for(let x=0;x<MW;x++){
    if(mg(x,y)===0){mmx.fillStyle='#c8a800';mmx.fillRect(x*CS,y*CS,CS,CS);}
  }
  // draw desk markers
  DESKS.forEach(d=>{
    mmx.fillStyle='#884400';
    mmx.fillRect(d.x*CS-2,d.y*CS-1,4,2);
  });
  // cross wall markers
  CROSS_WALLS.forEach(c=>{
    mmx.fillStyle='#cc0000';
    mmx.fillRect(c.x*CS,c.y*CS,CS,CS);
  });
  patients.forEach(p=>{
    if(p.dead)return;
    mmx.fillStyle=p.medGiven?'#44dd88':p.hair.hex;
    mmx.beginPath();mmx.arc(p.x*CS,p.y*CS,2.8,0,Math.PI*2);mmx.fill();
  });
  // reaper
  mmx.fillStyle='#ff2222';
  mmx.beginPath();mmx.arc(reaper.x*CS,reaper.y*CS,3.2,0,Math.PI*2);mmx.fill();
  // player dot + dir
  mmx.fillStyle='#c084fc';
  mmx.beginPath();mmx.arc(px*CS,py*CS,3,0,Math.PI*2);mmx.fill();
  mmx.strokeStyle='#c084fc';mmx.lineWidth=1.5;
  mmx.beginPath();mmx.moveTo(px*CS,py*CS);
  mmx.lineTo((px+Math.cos(ang)*3.5)*CS,(py+Math.sin(ang)*3.5)*CS);mmx.stroke();
}

// ═══════════════════════════════════════════════════════════════
//  PLAYER
// ═══════════════════════════════════════════════════════════════
const player={x:5,y:27.5,ang:0};
const SPD=.07,TSPD=.056;
const keys={};

function movePlayer(){
  const ca=Math.cos(player.ang),sa=Math.sin(player.ang);
  let nx=player.x,ny=player.y;
  if(keys.f){nx+=ca*SPD;ny+=sa*SPD;}
  if(keys.b){nx-=ca*SPD;ny-=sa*SPD;}
  if(keys.l){nx+=sa*SPD;ny-=ca*SPD;}
  if(keys.r){nx-=sa*SPD;ny+=ca*SPD;}
  if(keys.tl)player.ang-=TSPD;
  if(keys.tr)player.ang+=TSPD;
  const buf=.32;
  if(!isW(nx+Math.sign(nx-player.x+.001)*buf,player.y))player.x=nx;
  if(!isW(player.x,ny+Math.sign(ny-player.y+.001)*buf))player.y=ny;
  const room=getRoomName(player.x,player.y);
  document.getElementById('roomLbl').textContent=room;
}

// ═══════════════════════════════════════════════════════════════
//  TIMERS & HUD
// ═══════════════════════════════════════════════════════════════
let selectedPill=null;

function updateTimers(dt){
  let minT=9999;
  const strip=document.getElementById('dangerStrip');
  strip.innerHTML='';
  patients.forEach(p=>{
    if(p.dead||p.medGiven)return;
    p.timeLeft=Math.max(0,p.timeLeft-dt);
    if(p.timeLeft<minT)minT=p.timeLeft;
    const elapsed=PTIMER-p.timeLeft;
    if(elapsed>=RHUNT){
      const el=document.createElement('div');
      el.className='dbadge';
      el.textContent=`☠ ${p.name} ${Math.ceil(p.timeLeft)}s`;
      strip.appendChild(el);
    }
    if(p.timeLeft===0&&!p.dead){
      p.dead=true;deadCt++;
      showMsg(`☠  ${p.name} didn't make it...`);
      updScore();if(savedCt+deadCt===6)endGame();
    }
  });
  const tel=document.getElementById('timerBox');
  const living=patients.filter(p=>!p.dead&&!p.medGiven);
  if(living.length>0){
    const s=Math.ceil(minT);
    tel.textContent=`${Math.floor(s/60)}:${(s%60).toString().padStart(2,'0')}`;
    tel.className=s>45?'':s>20?'warn':'crit';
  } else {tel.textContent='--';tel.className='';}
}

function updScore(){
  document.getElementById('scoreBox').textContent=`Saved: ${savedCt} / 6`;
}

// ═══════════════════════════════════════════════════════════════
//  GIVE MED
// ═══════════════════════════════════════════════════════════════
function giveAction(){
  if(gameOver)return;
  if(selectedPill===null){showMsg('Select a pill from the tray first!');return;}
  let near=null,nearD=9999;
  patients.forEach(p=>{
    if(p.dead||p.medGiven)return;
    const d=Math.hypot(p.x-player.x,p.y-player.y);
    if(d<nearD){nearD=d;near=p;}
  });
  if(!near||nearD>3.2){showMsg('Get closer to a patient!\n(walk up until you can see them clearly)');return;}
  const pill=HAIRS[selectedPill];
  if(pill.name===near.hair.name){
    near.medGiven=true;savedCt++;
    showMsg(`✓  ${near.name} received their ${pill.name} pill!`);
    document.querySelectorAll('.tpill')[selectedPill].classList.add('used');
    selectedPill=null;
    document.querySelectorAll('.tpill').forEach(p=>p.classList.remove('sel'));
    updScore();
    if(savedCt+deadCt===6)endGame();
  } else {
    showMsg(`✗  Wrong pill for ${near.name}!\nCheck their hair color carefully.`);
  }
}

function endGame(){
  if(gameOver)return;
  gameOver=true;
  setTimeout(()=>{
    if(deadCt===0){
      const s=Math.floor((Date.now()-gameStartT)/1000);
      document.getElementById('winTime').textContent=`Shift time: ${Math.floor(s/60)}m ${s%60}s`;
      document.getElementById('winScr').style.display='flex';
    } else {
      document.getElementById('loseMsg').textContent=savedCt>0?`You saved ${savedCt} but lost ${deadCt}.`:'Nobody survived.';
      document.getElementById('deadLst').textContent='Lost: '+patients.filter(p=>p.dead).map(p=>p.name).join(', ');
      document.getElementById('loseScr').style.display='flex';
    }
  },900);
}

// ═══════════════════════════════════════════════════════════════
//  PILL TRAY
// ═══════════════════════════════════════════════════════════════
function buildTray(){
  const row=document.getElementById('pillsRow');row.innerHTML='';
  HAIRS.forEach((h,i)=>{
    const wrap=document.createElement('div');wrap.className='tpill-wrap';
    const p=document.createElement('div');p.className='tpill';p.style.background=h.hex;p.title=h.name;
    p.addEventListener('click',()=>pickPill(i));
    const lbl=document.createElement('div');lbl.className='tpill-lbl';lbl.textContent=h.name;
    wrap.appendChild(p);wrap.appendChild(lbl);row.appendChild(wrap);
  });
}
function pickPill(i){
  document.querySelectorAll('.tpill').forEach(p=>p.classList.remove('sel'));
  if(selectedPill===i){selectedPill=null;return;}
  selectedPill=i;document.querySelectorAll('.tpill')[i].classList.add('sel');
  showMsg(`Selected: ${HAIRS[i].name} pill`);
}

// ═══════════════════════════════════════════════════════════════
//  MESSAGES
// ═══════════════════════════════════════════════════════════════
let msgT;
function showMsg(txt){
  const el=document.getElementById('msgBox');el.textContent=txt;el.style.opacity=1;
  clearTimeout(msgT);msgT=setTimeout(()=>el.style.opacity=0,3200);
}

// ═══════════════════════════════════════════════════════════════
//  MAIN LOOP
// ═══════════════════════════════════════════════════════════════
let last=0;
function loop(ts){
  const dt=Math.min((ts-last)/1000,.1);last=ts;
  if(!gameOver){
    movePlayer();
    updateTimers(dt);
    updatePatients(dt);
    updateReaper(dt);
  }
  drawScene(player.x,player.y,player.ang);
  drawMinimap(player.x,player.y,player.ang);
  requestAnimationFrame(loop);
}

// ═══════════════════════════════════════════════════════════════
//  STARTUP & CONTROLS
// ═══════════════════════════════════════════════════════════════
function startGame(){
  document.getElementById('titleScreen').style.display='none';
  document.getElementById('gameWrap').style.display='flex';
  initPatients();buildTray();
  gameStartT=Date.now();
  resizeC();
  requestAnimationFrame(ts=>{last=ts;requestAnimationFrame(loop);});
}
function resizeC(){
  const w=document.getElementById('canvasWrap');
  gc.style.width=w.clientWidth+'px';gc.style.height=w.clientHeight+'px';
}
window.addEventListener('resize',resizeC);

function bindBtn(id,key){
  const el=document.getElementById(id);if(!el)return;
  const dn=e=>{e.preventDefault();keys[key]=true;el.classList.add('pr');};
  const up=e=>{e.preventDefault();keys[key]=false;el.classList.remove('pr');};
  el.addEventListener('touchstart',dn,{passive:false});
  el.addEventListener('touchend',up,{passive:false});
  el.addEventListener('touchcancel',up,{passive:false});
  el.addEventListener('mousedown',dn);el.addEventListener('mouseup',up);el.addEventListener('mouseleave',up);
}
document.addEventListener('DOMContentLoaded',()=>{
  bindBtn('bf','f');bindBtn('bb','b');bindBtn('bl','l');bindBtn('br','r');
  bindBtn('btl','tl');bindBtn('btr','tr');
});
document.addEventListener('keydown',e=>{
  if(e.key==='ArrowUp'||e.key==='w')keys.f=true;
  if(e.key==='ArrowDown'||e.key==='s')keys.b=true;
  if(e.key==='ArrowLeft')keys.tl=true;
  if(e.key==='ArrowRight')keys.tr=true;
  if(e.key==='a')keys.l=true;if(e.key==='d')keys.r=true;
  if(e.key===' '||e.key==='Enter')giveAction();
});
document.addEventListener('keyup',e=>{
  if(e.key==='ArrowUp'||e.key==='w')keys.f=false;
  if(e.key==='ArrowDown'||e.key==='s')keys.b=false;
  if(e.key==='ArrowLeft')keys.tl=false;if(e.key==='ArrowRight')keys.tr=false;
  if(e.key==='a')keys.l=false;if(e.key==='d')keys.r=false;
});
</script>
</body>
</html>
