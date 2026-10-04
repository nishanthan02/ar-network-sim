<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no">
<title>AR OSI Packet Simulator</title>
<script src="https://aframe.io/releases/1.4.0/aframe.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/AR-js-org/AR.js@3.4.5/aframe/build/aframe-ar.js"></script>
<style>
*{box-sizing:border-box}
body{margin:0;overflow:hidden;font-family:system-ui,sans-serif;user-select:none}
.hud{position:fixed;z-index:1000;background:rgba(15,23,42,.9);color:#e2e8f0}
#st{top:12px;left:50%;transform:translateX(-50%);width:88%;max-width:430px;padding:9px 14px;border-radius:16px;font:bold 12px monospace;color:#38bdf8;text-align:center;pointer-events:none}
#lay{top:84px;left:8px;padding:6px;border-radius:10px;font-size:10px;width:116px}
#lay div{padding:3px 5px;margin:2px 0;border-radius:4px;border-left:3px solid;opacity:.35;transition:.2s}
#lay div.on{opacity:1;background:#1e293b}
#ctl{bottom:14px;left:50%;transform:translateX(-50%);display:flex;flex-wrap:wrap;gap:6px;justify-content:center;padding:8px 10px;border-radius:22px;width:94%;max-width:540px}
button{padding:8px 12px;border:0;border-radius:16px;font:700 12px system-ui;cursor:pointer;background:#334155;color:#fff}
button:active{transform:scale(.95)}
#run{background:#38bdf8;color:#0f172a}
#nx{background:#facc15;color:#0f172a;display:none}
#mod{display:none;top:50%;left:50%;transform:translate(-50%,-50%);width:90%;max-width:380px;max-height:84vh;overflow:auto;border-radius:14px;padding:14px;border:1px solid #334155;z-index:2000}
#mod .r{background:#1e293b;border-left:3px solid;padding:5px 8px;margin:5px 0;border-radius:5px;font-size:11px}
#mod .r b{display:block;font-size:9px;color:#94a3b8;text-transform:uppercase}
#mod .r span{font-family:monospace}
.q button{display:block;width:100%;margin:4px 0;text-align:left;font-weight:500}
</style>
</head>
<body>
<div id="st" class="hud">Tap “Run Session”. Tap the packet any time to inspect it.</div>
<div id="lay" class="hud"></div>
<div id="ctl" class="hud">
  <button id="run" onclick="run()">▶ Run Session</button>
  <button id="sc" onclick="nextScen()">Scenario: Normal</button>
  <button id="sm" onclick="toggleStep()">Mode: Auto</button>
  <button id="nx" onclick="nextRes&&nextRes()">Next ▶</button>
  <button onclick="location.href=AR?'?3d':location.pathname">View: <span id="vm"></span></button>
</div>
<div id="mod" class="hud"></div>

<script>
const AR = !location.search.includes('3d');
$vm = document.getElementById('vm'); $vm.textContent = AR ? 'AR' : '3D';

/* ---------- Data ---------- */
/* compact snake layout: row 1 left→right (Client, Switch, Router), row 2 right→left (Firewall, Server) */
const X = [-.62, 0, .62, .62, 0], Z = [-.32, -.32, -.32, .32, .32];
const LN = {7:'Application',6:'Presentation',5:'Session',4:'Transport',3:'Network',2:'Data Link',1:'Physical'};
const LC = {7:'#a855f7',6:'#c084fc',5:'#818cf8',4:'#38bdf8',3:'#22c55e',2:'#f59e0b',1:'#e2e8f0'};
const ACT = {7:'HTTP data created',6:'encode / encrypt',5:'session tracked',4:'TCP header added (ports)',3:'IP header added (addresses)',2:'frame header added (MACs)',1:'converted to bits / signal'};
const M = {c:'3C:5A:B4:72:01:A1', rA:'00:1B:44:11:3A:B7', rB:'00:1B:44:11:3A:B8', s:'B2:11:F9:64:0C:5E'};
const SEQ = [
  {n:'SYN',    dir: 1, fl:'SYN',      q:'seq=100',         pl:'(no data) connection request', c:'#facc15'},
  {n:'SYN-ACK',dir:-1, fl:'SYN, ACK', q:'seq=300 ack=101', pl:'(no data) server accepts',     c:'#fb923c'},
  {n:'ACK',    dir: 1, fl:'ACK',      q:'seq=101 ack=301', pl:'(no data) handshake done',     c:'#facc15'},
  {n:'HTTP GET',dir:1, fl:'PSH, ACK', q:'seq=101 ack=301', pl:'GET /index.html HTTP/1.1',     c:'#e879f9'},
  {n:'HTTP 200',dir:-1,fl:'PSH, ACK', q:'seq=301 ack=125', pl:'HTTP/1.1 200 OK + index.html', c:'#4ade80'}
];
const N = [
  {name:'Client PC', ip:'192.168.1.50', L:7, v:`<a-box width=".3" height=".05" depth=".2" position="0 .025 .04" color="#38bdf8"></a-box><a-box width=".3" height=".2" depth=".02" position="0 .15 -.06" color="#0ea5e9"></a-box>`},
  {name:'Switch', ip:'MAC-based', L:2, v:`<a-box width=".32" height=".07" depth=".2" position="0 .035 0" color="#f59e0b"></a-box><a-sphere radius=".012" position="-.09 .075 .08" color="#fde047"></a-sphere><a-sphere radius=".012" position="-.05 .075 .08" color="#fde047"></a-sphere>`},
  {name:'Router', ip:'192.168.1.1', L:3, v:`<a-box width=".3" height=".07" depth=".2" position="0 .035 0" color="#14b8a6"></a-box><a-cylinder radius=".008" height=".18" position="-.1 .15 0" color="#0f766e"></a-cylinder><a-cylinder radius=".008" height=".18" position=".1 .15 0" color="#0f766e"></a-cylinder>`},
  {name:'Firewall', ip:'ALLOW', L:4, v:`<a-box id="fw" width=".06" height=".3" depth=".34" position="0 .15 0" color="#22c55e"></a-box>`},
  {name:'Web Server', ip:'10.0.0.8', L:7, v:`<a-box width=".24" height=".09" depth=".2" position="0 .045 0" color="#a855f7"></a-box><a-box width=".24" height=".09" depth=".2" position="0 .14 0" color="#9333ea"></a-box><a-box width=".24" height=".09" depth=".2" position="0 .23 0" color="#7e22ce"></a-box><a-sphere radius=".012" position=".09 .14 .101" color="#4ade80"></a-sphere>`}
];
const QZ = [
  {q:'Which layer adds the port number?', o:['L2 Data Link','L4 Transport','L7 Application'], a:1},
  {q:'What changes at every router hop?', o:['Source IP','MAC addresses','TCP port'], a:1},
  {q:'A switch forwards using…', o:['IP address','MAC address','Port number'], a:1},
  {q:'What does a router decrement?', o:['Sequence number','TTL','Port'], a:1},
  {q:'Which TCP flag starts a connection?', o:['ACK','FIN','SYN'], a:2},
  {q:'On the receiver, headers are removed starting from…', o:['L7','L1','Random order'], a:1}
];
const SC = ['Scenario: Normal','Scenario: Firewall blocks :80','Scenario: TTL = 1','Scenario: Packet loss'];
const RT = {
  7:()=>`Payload: ${H.pl}`,
  6:()=>`Encoding: UTF-8 · TLS: off (demo)`,
  5:()=>`Session ID: 0x2A41 · open`,
  4:()=>`TCP ${H.sp} → ${H.dp} | [${H.fl}] | ${H.q}`,
  3:()=>`IP ${H.src} → ${H.dst} | TTL=${H.ttl}`,
  2:()=>`MAC ${H.ms} → ${H.md}`,
  1:()=>`Bits: 0101 1100 1010 … (Ethernet signal)`
};

/* ---------- Build scene ---------- */
const rot = '-90 0 0';
let w = '';
for (let i=0;i<4;i++) w += `<a-entity line="start:${X[i]} .02 ${Z[i]};end:${X[i+1]} .02 ${Z[i+1]};color:#94a3b8"></a-entity>`;
N.forEach((n,i)=>{
  const lo = Z[i]<0 ? -.27 : .27;  /* label sits outside the layout, away from neighbours */
  w += `<a-entity id="n${i}" position="${X[i]} 0 ${Z[i]}">${n.v}<a-plane width=".58" height=".2" color="#0f172a" opacity=".75" transparent="true" rotation="${rot}" position="0 .03 ${lo}"></a-plane><a-text id="t${i}" value="${n.name}\n${n.ip}\n[L${n.L}]" position="0 .035 ${lo}" rotation="${rot}" align="center" width=".56" wrap-count="13" color="#fff" side="double"></a-text></a-entity>`;
});
let sh = '';
for (let l = 7; l >= 1; l--) {
  const s = .08 + .035 * (7 - l);
  sh += `<a-box id="s${l}" width="${s}" height="${s}" depth="${s}" color="${LC[l]}" material="opacity:${l==7?1:.3};transparent:true;wireframe:${l==1}" visible="false"></a-box>`;
}
w += `<a-entity id="pkt" class="clickable" position="${X[0]} .3 ${Z[0]}" visible="false">${sh}</a-entity>`;

const sc = document.createElement('a-scene');
sc.setAttribute('vr-mode-ui', 'enabled:false');
sc.setAttribute('cursor', 'rayOrigin:mouse');
sc.setAttribute('raycaster', 'objects:.clickable');
if (AR) {
  sc.setAttribute('embedded', '');
  sc.setAttribute('arjs', 'sourceType:webcam;debugUIEnabled:false');
  sc.innerHTML = `<a-marker preset="hiro" smooth="true" smoothCount="10">${w}</a-marker><a-entity camera></a-entity>`;
} else {
  sc.innerHTML = `<a-sky color="#0b1220"></a-sky><a-plane rotation="-90 0 0" width="8" height="8" color="#111c33"></a-plane>${w}<a-entity camera look-controls position="0 1.9 1.5" rotation="-55 0 0"></a-entity>`;
}
document.body.appendChild(sc);

const $ = id => document.getElementById(id);
const P = $('pkt'), S = l => $('s' + l);
$('lay').innerHTML = [7,6,5,4,3,2,1].map(l=>`<div id="h${l}" style="border-color:${LC[l]}">L${l} ${LN[l]}</div>`).join('');

/* ---------- State & helpers ---------- */
let paused=false, stepMode=false, running=false, scen=0, lossDone=false, nextRes=null;
let H={}, seg='A', loc='', px=X[0], z=Z[0], cur=null;
const on = new Set();
const sleep = ms => new Promise(r => setTimeout(r, ms));
const say = (t, c='#38bdf8') => { const e=$('st'); e.textContent=t; e.style.color=c; };
const hud = a => { for (let l=1;l<=7;l++) $('h'+l).classList.toggle('on', a.includes(l)); };
const show = (l,v) => { S(l).object3D.visible=v; v ? on.add(l) : on.delete(l); };
const clearP = () => { for (let l=1;l<=7;l++) show(l,false); };
async function wait(ms){ for(let e=0;e<ms;e+=30){ while(paused) await sleep(50); await sleep(30); } }
function pulse(i){ const e=$('n'+i); e.removeAttribute('animation__p'); e.setAttribute('animation__p','property:scale;from:1 1 1;to:1.18 1.18 1.18;dur:250;dir:alternate;loop:1;easing:easeInOutQuad'); }
function macs(d){ [H.ms,H.md] = seg==='A' ? (d>0?[M.c,M.rA]:[M.rA,M.c]) : (d>0?[M.rB,M.s]:[M.s,M.rB]); }
async function stepWait(){ if(!stepMode) return; $('nx').style.display='inline-block'; await new Promise(r=>nextRes=r); nextRes=null; $('nx').style.display='none'; }
async function fail(m){ S(7).setAttribute('color','#ef4444'); say(m,'#ef4444'); await wait(2200); }

/* delta-time movement: same speed on 60Hz and 120Hz screens, no setAttribute per frame */
const move = (tx,tz) => new Promise(res => {
  let t = performance.now();
  (function f(n){
    if(!paused){
      const d=Math.min((n-t)/1000,.1), dx=tx-px, dz=tz-z, L=Math.hypot(dx,dz);
      if(L<1e-4) return res();
      const s=Math.min(.5*d,L)/L; px+=dx*s; z+=dz*s;
      P.object3D.position.set(px,.3,z);
      if(Math.hypot(tx-px,tz-z)<1e-4) return res();
    }
    t=n; requestAnimationFrame(f);
  })(t);
});

/* ---------- OSI algorithm ---------- */
async function encap(a){
  for (const l of [7,6,5,4,3,2,1]) {
    show(l,true); hud([l]);
    if (l===2) macs(H.d);
    say(`${N[a].name} · L${l} ${LN[l]}: ${ACT[l]}`, LC[l]);
    await wait(450);
  }
  hud([]); await stepWait();
}
async function decap(b){
  for (const l of [1,2,3,4,5,6,7]) {
    hud([l]);
    say(l<7 ? `${N[b].name} · L${l} ${LN[l]}: header removed` : `${N[b].name} · L7: data delivered → "${H.pl}"`, LC[l]);
    await wait(450);
    if (l<7) show(l,false);
  }
  await stepWait();
}
async function atNode(i,p){
  const n=N[i]; pulse(i); loc=n.name; hud([1,2,3,4,5,6,7].filter(l=>l<=n.L));
  if (i===1) { say('Switch · L2 only: reads destination MAC, forwards. IP & ports untouched.', LC[2]); await wait(1000); }
  if (i===2) {
    say('Router · L3: strips old frame, routes by destination IP', LC[3]); await wait(900);
    seg = seg==='A'?'B':'A'; H.ttl--; macs(p.dir);
    say(`Router: TTL → ${H.ttl}, NEW frame MACs, IP unchanged`, LC[3]); await wait(1200);
    if (H.ttl<=0) { await fail('TTL = 0 → dropped. Router sends ICMP “Time Exceeded”'); return 'drop'; }
  }
  if (i===3) {
    say('Firewall · L3/L4: checking IPs and ports…', LC[4]); await wait(900);
    if (scen===1 && (H.dp===80 || H.sp===80)) { await fail('Firewall rule DENY port 80 → packet dropped'); return 'drop'; }
    say('Rule matched: ALLOW', '#22c55e'); await wait(500);
  }
  await stepWait(); return null;
}
async function deliver(p){
  const d=p.dir, a=d>0?0:4, b=d>0?4:0; cur=p; seg=d>0?'A':'B';
  H = {d, pl:p.pl, fl:p.fl, q:p.q, sp:d>0?49152:80, dp:d>0?80:49152,
       src:d>0?'192.168.1.50':'10.0.0.8', dst:d>0?'10.0.0.8':'192.168.1.50', ttl:(scen===2&&d>0)?1:64};
  px=X[a]; z=Z[a]; P.object3D.position.set(px,.3,z); clearP(); S(7).setAttribute('color',p.c);
  P.object3D.visible=true; loc=N[a].name; pulse(a);
  say(`Sending ${p.n}…`, p.c); await wait(500);
  await encap(a);
  for (let i=a+d;;i+=d) {
    if (scen===3 && !lossDone && p.n==='SYN' && i===3) {
      await move((X[2]+X[3])/2,(Z[2]+Z[3])/2); lossDone=true;
      await fail('Packet lost in transit! No reply will come'); P.object3D.visible=false; return 'lost';
    }
    await move(X[i],Z[i]); if (i===b) break;
    const r = await atNode(i,p);
    if (r) { P.object3D.visible=false; return r; }
  }
  loc=N[b].name; pulse(b); await decap(b); P.object3D.visible=false; return 'ok';
}
async function run(){
  if (running) return; running=true; lossDone=false; $('run').disabled=true; let ok=true;
  for (const p of SEQ) {
    let r = await deliver(p);
    if (r==='lost') { say(`Timeout → TCP retransmits ${p.n}`, '#facc15'); await wait(1300); r = await deliver(p); }
    if (r!=='ok') { say(`Session failed at ${p.n} — client would time out`, '#ef4444'); ok=false; break; }
    await wait(500);
  }
  if (ok) say('✔ TCP handshake + HTTP request/response complete', '#22c55e');
  hud([]); running=false; $('run').disabled=false;
}

/* ---------- Controls ---------- */
function nextScen(){
  if (running) return; scen=(scen+1)%4; $('sc').textContent=SC[scen];
  const b=scen===1; $('fw').setAttribute('color', b?'#ef4444':'#22c55e');
  $('t3').setAttribute('value', `Firewall\n${b?'DENY :80':'ALLOW'}\n[L4]`);
}
function toggleStep(){ stepMode=!stepMode; $('sm').textContent='Mode: '+(stepMode?'Step':'Auto'); if(!stepMode&&nextRes) nextRes(); }

/* ---------- Inspector + quiz ---------- */
P.addEventListener('click', () => {
  if (!running || paused) return;
  paused=true; render(); $('mod').style.display='block';
});
function render(){
  let h = `<div style="font-weight:700;color:#38bdf8;margin-bottom:6px">🔍 ${cur.n} packet — at ${loc}</div>`;
  for (let l=1;l<=7;l++) if (on.has(l)) h += `<div class="r" style="border-color:${LC[l]}"><b>L${l} ${LN[l]}</b><span>${RT[l]()}</span></div>`;
  h += `<div style="font-size:10px;color:#94a3b8">Outermost layer (L1) first, as on the wire</div><div id="qz"></div><div style="margin-top:8px"><button onclick="quiz()">🎯 Quiz me</button> <button onclick="resume()" style="background:#38bdf8;color:#0f172a">Resume</button></div>`;
  $('mod').innerHTML = h;
}
function quiz(){
  const q = QZ[Math.random()*QZ.length|0];
  $('qz').innerHTML = `<div class="q"><p style="font-size:12px;margin:8px 0 2px">${q.q}</p>${q.o.map((o,i)=>`<button onclick="ans(this,${i},${q.a})">${o}</button>`).join('')}</div>`;
}
function ans(b,i,a){ const bs=b.parentNode.querySelectorAll('button'); bs[a].style.background='#16a34a'; if(i!==a) b.style.background='#dc2626'; }
function resume(){ $('mod').style.display='none'; paused=false; }
</script>
</body>
</html>
