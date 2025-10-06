<!doctype html>
<html lang="zh-CN">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>随机红包抽奖（3套配置）</title>
  <style>
    :root{--bg:#0b1222;--card:#0f172a;--text:#e5e7eb;--muted:#94a3b8;--primary:#22d3ee;--accent:#a78bfa;}
    *{box-sizing:border-box}html,body{margin:0;height:100%;background:linear-gradient(180deg,#0b1222,#0f172a);color:var(--text);font-family:ui-sans-serif,system-ui,-apple-system,Segoe UI,Roboto}
    .wrap{max-width:980px;margin:40px auto;padding:0 16px}
    .title{display:flex;align-items:center;gap:10px;font-weight:800;font-size:24px}
    .card{background:rgba(17,24,39,.85);border:1px solid #1f2937;border-radius:20px;padding:18px;box-shadow:0 10px 30px #0006}
    .grid{display:grid;gap:16px}
    @media(min-width:900px){.grid{grid-template-columns:1.1fr .9fr}}
    .row{display:flex;gap:10px;flex-wrap:wrap}
    .label{font-size:12px;color:var(--muted);margin-bottom:6px}
    input,select,button{font:inherit}
    input,select{width:100%;padding:10px 12px;background:#0b1222;color:var(--text);border:1px solid #1f2937;border-radius:12px;outline:none}
    input:focus,select:focus{border-color:var(--primary);box-shadow:0 0 0 3px #22d3ee22}
    button{cursor:pointer;border:1px solid #1f2937;color:#0b1222;background:var(--primary);border-radius:14px;padding:10px 14px;font-weight:800;transition:transform .05s ease}
    button.secondary{background:#1f2937;color:var(--text)}
    button.ghost{background:transparent;color:var(--muted);border-color:#243041}
    button:active{transform:translateY(1px)}
    .pill{display:inline-flex;align-items:center;gap:8px;padding:6px 10px;border-radius:999px;background:#0b1222;border:1px solid #243041;color:var(--muted);font-size:12px}
    .result{text-align:center;padding:28px;border-radius:20px;background:#0b1222;border:1px dashed #243041}
    .amount{font-variant-numeric:tabular-nums;font-size:56px;font-weight:900;letter-spacing:1px}
    .list{max-height:320px;overflow:auto;border:1px solid #1f2937;border-radius:14px}
    table{width:100%;border-collapse:collapse;font-size:14px}
    th,td{padding:10px;border-bottom:1px solid #1f2937}
    th{position:sticky;top:0;background:#0b1222;color:var(--muted);text-align:left}
    .right{text-align:right}
    .hint{font-size:12px;color:var(--muted)}
  </style>
</head>
<body>
  <div class="wrap">
    <div class="title">🎁 随机红包抽奖 <span class="pill">3 套配置 · 最低 8.8 元</span></div>

    <div class="grid" style="margin-top:16px">
      <div class="card">
        <div class="row">
          <div style="flex:1 1 260px">
            <div class="label">选择红包配置</div>
            <select id="preset">
              <option value="500-50">500 元 / 50 包</option>
              <option value="1000-70">1000 元 / 70 包</option>
              <option value="1000-100">1000 元 / 100 包</option>
            </select>
          </div>
          <div style="width:160px">
            <div class="label">最低金额</div>
            <input id="min" type="number" step="0.01" value="8.8" />
          </div>
          <div style="width:140px">
            <div class="label">币种</div>
            <input id="currency" value="¥" />
          </div>
        </div>

        <div class="row" style="margin-top:10px">
          <button id="gen">生成红包</button>
          <button id="draw" class="secondary">开始抽一个</button>
          <button id="reset" class="ghost">重置</button>
          <button id="export" class="ghost">导出记录 CSV</button>
          <span class="pill" id="stats">未生成</span>
        </div>

        <div class="result" style="margin-top:14px">
          <div class="label">本次抽中金额</div>
          <div id="amountView" class="amount">—</div>
        </div>
      </div>

      <div class="card">
        <div class="label">抽奖记录</div>
        <div class="list">
          <table>
            <thead>
              <tr><th>#</th><th>时间</th><th class="right">金额</th></tr>
            </thead>
            <tbody id="logBody"></tbody>
          </table>
        </div>
      </div>
    </div>
  </div>

  <script>
    // —— 随机红包算法（保证 >= min，总额准确） ——
    function randomRedPackets(total, count, min){
      const result = [];
      let remain = total;
      for(let i=0;i<count-1;i++){
        const max = (remain - min*(count-i-1))*2/(count-i); // 动态上限（二倍均值法）
        const money = Math.random() * (Math.max(min, max) - min) + min;
        const v = Math.floor(money*100)/100; // 保留两位，向下取
        result.push(v);
        remain -= v;
      }
      result.push(Number(remain.toFixed(2)));
      return result;
    }

    const els = {
      preset: document.querySelector('#preset'),
      min: document.querySelector('#min'),
      currency: document.querySelector('#currency'),
      gen: document.querySelector('#gen'),
      draw: document.querySelector('#draw'),
      reset: document.querySelector('#reset'),
      export: document.querySelector('#export'),
      stats: document.querySelector('#stats'),
      amountView: document.querySelector('#amountView'),
      logBody: document.querySelector('#logBody')
    };

    let pool = [];    // 剩余可抽的红包金额数组
    let history = []; // 抽中记录 {t,v}

    function parsePreset(v){
      const [total,count] = v.split('-').map(Number);
      return {total, count};
    }

    function shuffle(a){
      const arr = [...a];
      for(let i=arr.length-1;i>0;i--){
        const j = Math.floor(Math.random()*(i+1));
        [arr[i],arr[j]] = [arr[j],arr[i]];
      }
      return arr;
    }

    function generate(){
      const {total,count} = parsePreset(els.preset.value);
      const min = Number(els.min.value) || 8.8;
      if(min*count > total){
        alert(`最低金额过高：需要 min*count <= total，当前 ${min}*${count} = ${(min*count).toFixed(2)} > ${total}`);
        return;
      }
      pool = randomRedPackets(total, count, min);
      pool = shuffle(pool); // 打乱，抽取更随机
      history = [];
      save();
      render();
    }

    function drawOne(){
      if(pool.length===0){ alert('请先生成红包'); return; }
      const idx = Math.floor(Math.random()*pool.length);
      const v = pool.splice(idx,1)[0]; // 抽中后从池里移除（不重复）
      const rec = { t:new Date().toISOString(), v };
      history.unshift(rec);
      animateAmount(v);
      save();
      render();
    }

    function animateAmount(target){
      const cur = els.currency.value || '¥';
      const start = performance.now();
      const dur = 800;
      (function step(now){
        const p = Math.min(1,(now-start)/dur);
        const ease = 1 - Math.pow(1-p,3);
        const fake = p<1 ? target*(0.6+Math.random()*0.4) : target;
        els.amountView.textContent = `${cur}${fake.toFixed(2)}`;
        if(p<1) requestAnimationFrame(step); else flash();
      })(start);
      function flash(){
        els.amountView.style.transform = 'scale(1.06)';
        setTimeout(()=>{ els.amountView.style.transform = 'scale(1)'; }, 180);
      }
    }

    function resetAll(){
      pool = [];
      history = [];
      save();
      render();
      els.amountView.textContent = '—';
    }

    function exportCSV(){
      if(history.length===0){ alert('暂无记录可导出'); return; }
      const cur = els.currency.value || '¥';
      const rows = [['Index','Time','Amount'], ...history.map((h,i)=>[i+1,new Date(h.t).toISOString(), `${cur}${h.v.toFixed(2)}`])];
      const csv = rows.map(r=>r.map(c=>`"${String(c).replace(/"/g,'""')}"`).join(',')).join('\n');
      const blob = new Blob(['\ufeff'+csv], {type:'text/csv;charset=utf-8;'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href=url; a.download=`packets_${Date.now()}.csv`; a.click();
      URL.revokeObjectURL(url);
    }

    function render(){
      const cur = els.currency.value || '¥';
      const left = pool.length;
      const sumLeft = pool.reduce((s,x)=>s+x,0);
      const sumSent = history.reduce((s,x)=>s+x.v,0);
      const sumTotal = sumLeft + sumSent;
      els.stats.textContent = left>0 ? `剩余 ${left} 包 · 已发 ${history.length} 包 · 总额 ${cur}${sumTotal.toFixed(2)}` : '未生成';
      els.logBody.innerHTML = history.map((h,i)=>{
        return `<tr><td>${i+1}</td><td>${new Date(h.t).toLocaleString()}</td><td class="right">${cur}${h.v.toFixed(2)}</td></tr>`
      }).join('');
    }

    function save(){
      const data = { preset:els.preset.value, min:els.min.value, currency:els.currency.value, pool, history };
      localStorage.setItem('packet-raffle-3presets', JSON.stringify(data));
    }
    function restore(){
      try{
        const raw = localStorage.getItem('packet-raffle-3presets');
        if(!raw) return;
        const d = JSON.parse(raw);
        if(d.preset) els.preset.value = d.preset;
        if(d.min) els.min.value = d.min;
        if(d.currency) els.currency.value = d.currency;
        pool = Array.isArray(d.pool)? d.pool : [];
        history = Array.isArray(d.history)? d.history : [];
        render();
      }catch{}
    }

    // 事件绑定
    els.gen.addEventListener('click', generate);
    els.draw.addEventListener('click', drawOne);
    els.reset.addEventListener('click', resetAll);
    els.export.addEventListener('click', exportCSV);
    ['preset','min','currency'].forEach(id=>{
      els[id].addEventListener('change', save);
      els[id].addEventListener('keyup', save);
    });

    restore();
  </script>
</body>
</html>
