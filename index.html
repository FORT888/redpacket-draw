<!doctype html>
<html lang="zh-CN">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>三轮红包抽奖（Firebase 同步版 - 国内兼容）</title>

<!-- ✅ 使用国内可访问的 Firebase CDN -->
<script src="https://cdn.jsdelivr.net/npm/firebase@10.13.0/firebase-app-compat.js"></script>
<script src="https://cdn.jsdelivr.net/npm/firebase@10.13.0/firebase-firestore-compat.js"></script>

<style>
:root{--bg:#0b1222;--card:#0f172a;--text:#e5e7eb;--muted:#94a3b8;--primary:#22d3ee;--ok:#34d399;--warn:#fbbf24}
*{box-sizing:border-box}
html,body{margin:0;height:100%;background:linear-gradient(180deg,#0b1222,#0f172a);
  color:var(--text);font-family:ui-sans-serif,system-ui,-apple-system,Segoe UI,Roboto;overflow-x:hidden;}
.wrap{max-width:1000px;margin:36px auto;padding:0 16px}
.title{text-align:center;font-weight:800;font-size:24px}
.card{background:rgba(17,24,39,.88);border:1px solid #1f2937;border-radius:20px;
  padding:18px;box-shadow:0 10px 30px #0006;text-align:center}
.btn{cursor:pointer;border:1px solid #1f2937;color:#0b1222;background:var(--primary);
  border-radius:14px;padding:10px 14px;font-weight:800;margin:6px}
.btn.ok{background:var(--ok);color:#062b1e}
.btn.warn{background:var(--warn);color:#3b2900}
.btn.secondary{background:#1f2937;color:var(--text)}
.btn.ghost{background:transparent;color:var(--muted);border-color:#243041}
.disabled{opacity:.55;pointer-events:none}
.amount{font-size:56px;font-weight:900;letter-spacing:1px}
.hint{font-size:12px;color:var(--muted)}
.firework{position:fixed;left:50%;top:50%;width:6px;height:6px;border-radius:50%;
  background:#fff;pointer-events:none;animation:boom 1s ease-out forwards;}
@keyframes boom{from{opacity:1;transform:scale(1) translate(0,0);}to{opacity:0;transform:scale(2) translate(var(--x),var(--y));}}
</style>
</head>
<body>
<div class="wrap">
  <div class="title">🎁 三轮红包抽奖（云端同步版）</div>
  <div class="card" style="margin-top:20px">
    <div id="roundInfo">当前：—</div>
    <div id="statusInfo" class="hint">状态：未开始</div>
    <button class="btn ok" id="draw">抽红包</button>
    <button class="btn ghost" id="export">导出记录</button>
    <button class="btn secondary" id="reset">重置（需密码）</button>
    <button class="btn warn" id="next">开始下一轮（需密码）</button>
    <div style="margin-top:18px">
      <div class="hint">本次抽中金额</div>
      <div id="amountView" class="amount">—</div>
    </div>
    <div class="hint" id="leftInfo" style="margin-top:8px">剩余：—</div>
  </div>

  <div class="card" style="margin-top:20px;text-align:left">
    <div class="hint">抽奖记录：</div>
    <table style="width:100%;border-collapse:collapse;font-size:14px">
      <thead>
        <tr><th>ID</th><th>轮次</th><th>时间</th><th style="text-align:right">金额</th></tr>
      </thead>
      <tbody id="logBody"></tbody>
    </table>
  </div>
</div>

<script>
/* ---------- Firebase 初始化 ---------- */
try {
  const firebaseConfig = {
    apiKey: "AIzaSyBqiysQdJzUMfn-zwzeEu8hhU0T51OKGGA",
    authDomain: "redpacket-lottery.firebaseapp.com",
    projectId: "redpacket-lottery",
    storageBucket: "redpacket-lottery.appspot.com",
    messagingSenderId: "252110350053",
    appId: "1:252110350053:web:60b394678bb8910ae6560c",
    measurementId: "G-5MXFC392RP"
  };

  firebase.initializeApp(firebaseConfig);
  var db = firebase.firestore();
  console.log("✅ Firebase 已初始化");
} catch (e) {
  alert("❌ Firebase 连接失败，请检查网络或防火墙设置。");
  console.error(e);
}

/* ---------- 配置 ---------- */
const ADMIN_PASSWORD = "happy888999";
const PRESETS = [
  { name: "第 1 轮", total: 500, count: 3, min: 8.8 },
  { name: "第 2 轮", total: 1000, count: 70, min: 8.8 },
  { name: "第 3 轮", total: 1000, count: 100, min: 8.8 }
];
const CURRENCY = "¥";

let roundIndex = 0, pool = [], history = [], userDraws = {}, roundLocked = false;

const els = {
  draw: document.querySelector("#draw"),
  export: document.querySelector("#export"),
  reset: document.querySelector("#reset"),
  next: document.querySelector("#next"),
  amountView: document.querySelector("#amountView"),
  logBody: document.querySelector("#logBody"),
  roundInfo: document.querySelector("#roundInfo"),
  statusInfo: document.querySelector("#statusInfo"),
  leftInfo: document.querySelector("#leftInfo")
};

function randomRedPackets(total, count, min){
  const result=[];let remain=total;
  for(let i=0;i<count-1;i++){
    const max=(remain-min*(count-i-1))*2/(count-i);
    const money=Math.random()*(Math.max(min,max)-min)+min;
    const v=Math.floor(money*100)/100;result.push(v);remain-=v;
  }
  result.push(Number(remain.toFixed(2)));return result;
}
function shuffle(a){const arr=[...a];for(let i=arr.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[arr[i],arr[j]]=[arr[j],arr[i]];}return arr;}

/* ---------- Firestore 同步 ---------- */
async function syncLoad(){
  try {
    const ref = db.collection("lottery").doc("roundState");
    const snap = await ref.get();
    if(!snap.exists){
      await ref.set({roundIndex:0, pool: shuffle(randomRedPackets(PRESETS[0].total, PRESETS[0].count, PRESETS[0].min)), 
        history:[], userDraws:{}, roundLocked:false});
      return syncLoad();
    }
    const d = snap.data();
    roundIndex = d.roundIndex; pool = d.pool; history = d.history; userDraws = d.userDraws; roundLocked = d.roundLocked;
    render();
  } catch (err) {
    alert("⚠️ 无法加载数据库，请检查网络或 Firestore 规则。");
    console.error(err);
  }
}

async function syncSave(){
  try {
    await db.collection("lottery").doc("roundState").set({roundIndex,pool,history,userDraws,roundLocked});
  } catch (e) {
    alert("⚠️ 数据保存失败，请重试。");
    console.error(e);
  }
}

/* ---------- 抽奖 ---------- */
async function drawOne(){
  if(roundLocked){alert("当前轮未开放，请等待管理员开启下一轮。");return;}
  if(pool.length===0){alert("本轮红包已抽完！");return;}
  const id=prompt("请输入您的6位数字ID：");
  if(!/^[0-9]{6}$/.test(id)){alert("请输入正确的6位数字ID！");return;}
  if(!userDraws[id])userDraws[id]={0:false,1:false,2:false};
  if(userDraws[id][roundIndex]){alert("您本轮已经抽过红包，下一轮再来吧！");return;}
  const v=pool.shift();
  userDraws[id][roundIndex]=true;
  const record={id,round:PRESETS[roundIndex].name,t:new Date().toLocaleString(),v};
  history.unshift(record);
  showFireworks();
  alert(`🎉 恭喜获得 ${CURRENCY}${v.toFixed(2)}！`);
  els.amountView.textContent=`${CURRENCY}${v.toFixed(2)}`;
  if(pool.length===0){roundLocked=true;alert(`${PRESETS[roundIndex].name} 已抽完，请管理员开启下一轮。`);}
  await syncSave();render();
}

/* ---------- 下一轮 ---------- */
async function nextRound(){
  const pwd=prompt("请输入管理员密码以开启下一轮：");
  if(pwd!==ADMIN_PASSWORD){alert("❌ 密码错误，无法开启下一轮。");return;}
  if(roundIndex>=PRESETS.length-1){alert("已经是最后一轮！");return;}
  roundIndex+=1;
  const p=PRESETS[roundIndex];
  pool=shuffle(randomRedPackets(p.total,p.count,p.min));
  roundLocked=false;
  await syncSave();
  alert(`${PRESETS[roundIndex].name} 已开始！`);
  render();
}

/* ---------- 重置 ---------- */
async function resetAll(){
  const pwd=prompt("请输入管理员密码以确认重置：");
  if(pwd!==ADMIN_PASSWORD){alert("❌ 密码错误，无法重置。");return;}
  if(!confirm("确定要重置所有记录吗？"))return;
  roundIndex=0;history=[];userDraws={};roundLocked=false;
  pool=shuffle(randomRedPackets(PRESETS[0].total,PRESETS[0].count,PRESETS[0].min));
  await syncSave();
  alert("✅ 已重置，回到第一轮。");render();
}

/* ---------- 导出 ---------- */
function exportCSV(){
  if(history.length===0){alert("暂无记录");return;}
  const rows=[["ID","轮次","时间","金额"],...history.map(h=>[h.id,h.round,h.t,`${CURRENCY}${h.v.toFixed(2)}`])];
  const csv=rows.map(r=>r.map(c=>`"${String(c).replace(/"/g,'""')}"`).join(",")).join("\n");
  const blob=new Blob(['\ufeff'+csv],{type:'text/csv;charset=utf-8;'});
  const a=document.createElement("a");a.href=URL.createObjectURL(blob);a.download=`records_${Date.now()}.csv`;a.click();
}

/* ---------- 其他 ---------- */
function showFireworks(){for(let i=0;i<25;i++){const f=document.createElement('div');f.className='firework';
f.style.setProperty('--x',`${(Math.random()-0.5)*400}px`);f.style.setProperty('--y',`${(Math.random()-0.5)*400}px`);
f.style.background=`hsl(${Math.random()*360},100%,70%)`;document.body.appendChild(f);setTimeout(()=>f.remove(),1000);}}
function render(){
  const p=PRESETS[roundIndex];
  els.roundInfo.textContent=`当前：${p.name}（总额 ${CURRENCY}${p.total} / ${p.count} 包）`;
  els.leftInfo.textContent=`剩余：${pool.length} 包`;
  els.statusInfo.textContent=`状态：${roundLocked?"未开放":"进行中"}`;
  els.logBody.innerHTML=history.map(h=>`<tr><td>${h.id}</td><td>${h.round}</td><td>${h.t}</td><td style='text-align:right'>${CURRENCY}${h.v.toFixed(2)}</td></tr>`).join("");
}

/* ---------- 初始化 ---------- */
function bindClick(el, handler){
  el.addEventListener("click", handler);
  el.addEventListener("touchstart", handler);
}
bindClick(els.draw, drawOne);
bindClick(els.next, nextRound);
bindClick(els.reset, resetAll);
bindClick(els.export, exportCSV);
syncLoad();
</script>
</body>
</html>
