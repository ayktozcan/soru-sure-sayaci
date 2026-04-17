# soru-sure-sayaci
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="default">
  <meta name="apple-mobile-web-app-title" content="Soru Sayaç">
  <title>Soru Sayaç</title>
  <style>
    :root{
      --bg:#f5f7fb;--card:#ffffff;--text:#101828;--muted:#667085;--line:#d0d5dd;--blue:#2563eb;--green:#16a34a;--red:#dc2626;
    }
    *{box-sizing:border-box;-webkit-tap-highlight-color:transparent;touch-action:manipulation}
    body{margin:0;font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Arial,sans-serif;background:var(--bg);color:var(--text)}
    .wrap{max-width:800px;margin:0 auto;padding:calc(12px + env(safe-area-inset-top)) 12px calc(18px + env(safe-area-inset-bottom))}
    .card{background:var(--card);border:1px solid var(--line);border-radius:18px;padding:14px;margin-bottom:12px;box-shadow:0 4px 14px rgba(16,24,40,.06)}
    h1,h2,p{margin:0} h1{font-size:1.3rem} h2{font-size:1.05rem;margin-bottom:10px}
    .sub{margin-top:6px;color:var(--muted);font-size:.95rem;line-height:1.4}
    .grid{display:grid;grid-template-columns:1fr 1fr;gap:10px}
    .grid3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px}
    @media(max-width:640px){.grid,.grid3{grid-template-columns:1fr}}
    label{display:block;font-size:.9rem;color:var(--muted);margin-bottom:6px}
    input{width:100%;padding:12px 14px;border-radius:12px;border:1px solid var(--line);font-size:1rem;background:#fff;color:var(--text)}
    .row{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:10px}
    button{border:none;border-radius:14px;padding:14px 16px;font-size:1rem;font-weight:700}
    .primary{background:var(--blue);color:#fff}
    .soft{background:#eef2ff;color:#1d4ed8}
    .danger{background:#fef2f2;color:var(--red);border:1px solid #fecaca}
    .tap{margin-top:12px;border-radius:22px;border:2px dashed #93c5fd;background:#eff6ff;min-height:230px;display:flex;align-items:center;justify-content:center;text-align:center;padding:20px}
    .tap.disabled{opacity:.55}
    .timer{font-size:3rem;font-weight:800;letter-spacing:.02em}
    .q{margin-top:10px;font-size:1.05rem;color:#1d4ed8;font-weight:700}
    .hint{margin-top:8px;color:var(--muted)}
    .stats{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:12px}
    .stat{background:#fafafa;border:1px solid var(--line);border-radius:14px;padding:12px}
    .k{font-size:.85rem;color:var(--muted)} .v{margin-top:8px;font-size:1.1rem;font-weight:800}
    .pill{display:inline-block;padding:7px 10px;border-radius:999px;background:#eef2ff;color:#1d4ed8;font-size:.88rem;margin-top:10px}
    .list{display:grid;gap:8px;margin-top:10px}
    .item{display:flex;justify-content:space-between;gap:8px;padding:10px 12px;border:1px solid var(--line);border-radius:12px;background:#fafafa}
    .muted{color:var(--muted)}
    .history{display:grid;gap:10px;margin-top:10px}
    .hitem{border:1px solid var(--line);border-radius:14px;padding:12px;background:#fafafa}
    canvas{width:100%;height:260px;border:1px solid var(--line);border-radius:16px;background:#fff;display:block}
    .note{font-size:.84rem;color:var(--muted);line-height:1.5}
  </style>
</head>
<body>
<div class="wrap">
  <div class="card">
    <h1>Soru Sayaç</h1>
    <p class="sub">iPhone Safari için sadeleştirilmiş sürüm. Her soru bitince büyük alana dokun.</p>
  </div>

  <div class="card">
    <h2>Yeni çalışma</h2>
    <div class="grid3">
      <div>
        <label for="name">Çalışma adı</label>
        <input id="name" type="text" placeholder="Örn: Türkçe Deneme">
      </div>
      <div>
        <label for="count">Soru sayısı</label>
        <input id="count" type="number" min="1" max="300" value="25" inputmode="numeric">
      </div>
      <div>
        <label for="warn">Uyarı eşiği (sn)</label>
        <input id="warn" type="number" min="10" max="600" value="120" inputmode="numeric">
      </div>
    </div>

    <div class="row">
      <button id="startBtn" class="primary">Başlat</button>
      <button id="pauseBtn" class="soft">Duraklat</button>
      <button id="undoBtn" class="soft">Son soruyu geri al</button>
      <button id="resetBtn" class="danger">Sıfırla</button>
    </div>

    <div class="pill" id="status">Hazır</div>

    <div class="tap disabled" id="tapBox" role="button" tabindex="0">
      <div>
        <div class="timer" id="timer">00:00.0</div>
        <div class="q" id="qLabel">Başlatınca soru süresi burada akar</div>
        <div class="hint" id="hint">Her soru bitince bu alana dokun</div>
      </div>
    </div>

    <div class="stats">
      <div class="stat"><div class="k">Mevcut soru</div><div class="v" id="cur">-</div></div>
      <div class="stat"><div class="k">Toplam süre</div><div class="v" id="tot">00:00</div></div>
      <div class="stat"><div class="k">Ortalama</div><div class="v" id="avg">00:00</div></div>
      <div class="stat"><div class="k">En uzun soru</div><div class="v" id="max">00:00</div></div>
    </div>

    <div class="list" id="times"></div>
  </div>

  <div class="card">
    <h2>Performans grafiği</h2>
    <canvas id="chart" width="780" height="260"></canvas>
    <p class="note" style="margin-top:10px">Grafik kayıtlı çalışmaların ortalama soru sürelerini gösterir.</p>
  </div>

  <div class="card">
    <h2>Geçmiş çalışmalar</h2>
    <div class="row">
      <button id="exportBtn" class="soft">Verileri dışa aktar</button>
      <button id="importBtn" class="soft">Veri içe al</button>
      <button id="clearBtn" class="danger">Geçmişi sil</button>
      <button id="installNoteBtn" class="soft">Ana ekrana ekleme notu</button>
    </div>
    <input id="fileInput" type="file" accept="application/json" style="display:none">
    <div class="history" id="history"></div>
  </div>
</div>

<script>
(function(){
  var STORAGE_KEY = 'soru-sayac-history-v3';
  var el = {
    name: document.getElementById('name'), count: document.getElementById('count'), warn: document.getElementById('warn'),
    startBtn: document.getElementById('startBtn'), pauseBtn: document.getElementById('pauseBtn'), undoBtn: document.getElementById('undoBtn'), resetBtn: document.getElementById('resetBtn'),
    status: document.getElementById('status'), tapBox: document.getElementById('tapBox'), timer: document.getElementById('timer'), qLabel: document.getElementById('qLabel'), hint: document.getElementById('hint'),
    cur: document.getElementById('cur'), tot: document.getElementById('tot'), avg: document.getElementById('avg'), max: document.getElementById('max'), times: document.getElementById('times'),
    history: document.getElementById('history'), chart: document.getElementById('chart'), exportBtn: document.getElementById('exportBtn'), importBtn: document.getElementById('importBtn'), clearBtn: document.getElementById('clearBtn'), fileInput: document.getElementById('fileInput'), installNoteBtn: document.getElementById('installNoteBtn')
  };

  var state = resetState();
  var timerId = null;
  var tapLock = false;

  function resetState(){
    return {running:false, paused:false, totalQuestions:25, currentQuestion:1, sessionName:'', warnSec:120, sessionStart:0, questionStart:0, pauseStart:0, pausedAccum:0, pausedAccumQuestion:0, perQuestion:[]};
  }

  function now(){ return Date.now(); }
  function pad(n){ return String(n).padStart(2,'0'); }
  function fmt(ms, tenth){
    ms = Math.max(0, ms || 0);
    var totalSec = ms / 1000;
    var m = Math.floor(totalSec / 60);
    var s = Math.floor(totalSec % 60);
    var t = Math.floor((ms % 1000) / 100);
    return tenth ? (pad(m)+':'+pad(s)+'.'+t) : (pad(m)+':'+pad(s));
  }
  function loadHistory(){ try { return JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]'); } catch(e){ return []; } }
  function saveHistory(arr){ try { localStorage.setItem(STORAGE_KEY, JSON.stringify(arr)); } catch(e){} }
  function safeName(s){ return String(s||'').replace(/[&<>\"]/g, function(ch){ return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[ch]; }); }
  function setStatus(s){ el.status.textContent = s; }

  function totalElapsed(){
    if(!state.sessionStart) return 0;
    var activeNow = state.paused ? state.pauseStart : now();
    return activeNow - state.sessionStart - state.pausedAccum;
  }
  function questionElapsed(){
    if(!state.questionStart) return 0;
    var activeNow = state.paused ? state.pauseStart : now();
    return activeNow - state.questionStart - state.pausedAccumQuestion;
  }

  function updateStats(){
    var sum = 0, max = 0, i;
    for(i=0;i<state.perQuestion.length;i++){ sum += state.perQuestion[i]; if(state.perQuestion[i] > max) max = state.perQuestion[i]; }
    var avg = state.perQuestion.length ? (sum / state.perQuestion.length) : 0;
    el.cur.textContent = state.running ? (Math.min(state.currentQuestion, state.totalQuestions) + '/' + state.totalQuestions) : '-';
    el.tot.textContent = fmt(totalElapsed(), false);
    el.avg.textContent = fmt(avg, false);
    el.max.textContent = fmt(max, false);
    renderTimes();
  }

  function renderTimes(){
    if(!state.perQuestion.length){ el.times.innerHTML = ''; return; }
    var warnMs = (parseInt(state.warnSec,10) || 120) * 1000;
    var html = '';
    for(var i=0;i<state.perQuestion.length;i++){
      var val = state.perQuestion[i];
      var extra = '';
      if(val >= warnMs * 1.5) extra = ' style="border-color:#fecaca;background:#fef2f2"';
      else if(val >= warnMs) extra = ' style="border-color:#fde68a;background:#fffbeb"';
      html += '<div class="item"'+extra+'><span>Soru '+(i+1)+'</span><strong>'+fmt(val,true)+'</strong></div>';
    }
    el.times.innerHTML = html;
  }

  function startSession(){
    var q = parseInt(el.count.value,10);
    var w = parseInt(el.warn.value,10);
    if(!q || q < 1){ alert('Geçerli bir soru sayısı gir.'); return; }
    state = resetState();
    state.running = true;
    state.totalQuestions = q;
    state.warnSec = w || 120;
    state.sessionName = (el.name.value || '').trim();
    state.sessionStart = now();
    state.questionStart = now();
    el.tapBox.classList.remove('disabled');
    el.qLabel.textContent = 'Soru 1 / ' + state.totalQuestions;
    el.hint.textContent = 'Soruyu bitirince bu alana dokun';
    el.pauseBtn.textContent = 'Duraklat';
    setStatus('Çalışma başladı');
    updateStats();
    if(timerId) clearInterval(timerId);
    timerId = setInterval(tick, 100);
    tick();
  }

  function togglePause(){
    if(!state.running) return;
    if(!state.paused){
      state.paused = true;
      state.pauseStart = now();
      el.pauseBtn.textContent = 'Devam et';
      setStatus('Duraklatıldı');
    } else {
      var diff = now() - state.pauseStart;
      state.pausedAccum += diff;
      state.pausedAccumQuestion += diff;
      state.pauseStart = 0;
      state.paused = false;
      el.pauseBtn.textContent = 'Duraklat';
      setStatus('Devam ediyor');
      tick();
    }
  }

  function nextQuestion(){
    if(!state.running || state.paused || tapLock) return;
    tapLock = true;
    setTimeout(function(){ tapLock = false; }, 250);

    var spent = questionElapsed();
    state.perQuestion.push(spent);

    if(state.currentQuestion >= state.totalQuestions){
      finishSession();
      return;
    }

    state.currentQuestion += 1;
    state.questionStart = now();
    state.pausedAccumQuestion = 0;
    el.qLabel.textContent = 'Soru ' + state.currentQuestion + ' / ' + state.totalQuestions;
    setStatus('Soru ' + (state.currentQuestion - 1) + ' kaydedildi');
    updateStats();
    tick();
  }

  function undoLast(){
    if(!state.running || !state.perQuestion.length) return;
    state.perQuestion.pop();
    state.currentQuestion = Math.max(1, state.currentQuestion - 1);
    state.questionStart = now();
    state.pausedAccumQuestion = 0;
    el.qLabel.textContent = 'Soru ' + state.currentQuestion + ' / ' + state.totalQuestions;
    setStatus('Son soru geri alındı');
    updateStats();
    tick();
  }

  function hardReset(){
    state = resetState();
    if(timerId){ clearInterval(timerId); timerId = null; }
    el.tapBox.classList.add('disabled');
    el.timer.textContent = '00:00.0';
    el.qLabel.textContent = 'Başlatınca soru süresi burada akar';
    el.hint.textContent = 'Her soru bitince bu alana dokun';
    el.pauseBtn.textContent = 'Duraklat';
    setStatus('Hazır');
    updateStats();
    el.times.innerHTML = '';
  }

  function finishSession(){
    var totalMs = totalElapsed();
    var sum = 0, max = 0;
    for(var i=0;i<state.perQuestion.length;i++){ sum += state.perQuestion[i]; if(state.perQuestion[i] > max) max = state.perQuestion[i]; }
    var avg = state.perQuestion.length ? sum / state.perQuestion.length : 0;
    var history = loadHistory();
    history.unshift({
      id: 'id-' + now() + '-' + Math.floor(Math.random()*1000000),
      name: state.sessionName || ('Çalışma ' + new Date().toLocaleDateString('tr-TR')),
      questionCount: state.totalQuestions,
      warnSec: state.warnSec,
      perQuestion: state.perQuestion.slice(),
      totalMs: totalMs,
      avgMs: avg,
      maxMs: max,
      createdAt: new Date().toISOString()
    });
    saveHistory(history);
    renderHistory();
    drawChart();
    alert('Bitti.\nToplam süre: ' + fmt(totalMs,false) + '\nOrtalama soru süresi: ' + fmt(avg,false));
    hardReset();
  }

  function renderHistory(){
    var history = loadHistory();
    if(!history.length){ el.history.innerHTML = '<div class="muted">Henüz kayıt yok.</div>'; return; }
    var html = '';
    for(var i=0;i<history.length;i++){
      var item = history[i];
      html += '<div class="hitem"><strong>' + safeName(item.name) + '</strong><div class="note" style="margin-top:6px">' +
        new Date(item.createdAt).toLocaleString('tr-TR') + '</div><div class="note" style="margin-top:6px">' +
        item.questionCount + ' soru • Toplam: ' + fmt(item.totalMs,false) + ' • Ortalama: ' + fmt(item.avgMs,false) + ' • En uzun: ' + fmt(item.maxMs,false) + '</div></div>';
    }
    el.history.innerHTML = html;
  }

  function drawChart(){
    var canvas = el.chart;
    var ctx = canvas.getContext('2d');
    var w = canvas.width, h = canvas.height;
    ctx.clearRect(0,0,w,h);
    ctx.fillStyle = '#ffffff'; ctx.fillRect(0,0,w,h);
    var history = loadHistory().slice(0,20).reverse();
    if(!history.length){ ctx.fillStyle = '#667085'; ctx.font = '16px sans-serif'; ctx.fillText('Grafik için kayıt gerekli.', 20, 34); return; }
    var pad = {l:46,r:14,t:16,b:34};
    var plotW = w - pad.l - pad.r, plotH = h - pad.t - pad.b;
    var vals = [], i, max = 60;
    for(i=0;i<history.length;i++){ vals.push(Math.round(history[i].avgMs/1000)); if(vals[i] > max) max = vals[i]; }
    ctx.strokeStyle = '#e4e7ec'; ctx.lineWidth = 1; ctx.font = '12px sans-serif'; ctx.fillStyle = '#667085';
    for(i=0;i<=4;i++){
      var y = pad.t + plotH*(i/4);
      ctx.beginPath(); ctx.moveTo(pad.l,y); ctx.lineTo(w-pad.r,y); ctx.stroke();
      var label = Math.round(max - (max*i/4)); ctx.fillText(String(label), 8, y+4);
    }
    ctx.strokeStyle = '#98a2b3';
    ctx.beginPath(); ctx.moveTo(pad.l,pad.t); ctx.lineTo(pad.l,h-pad.b); ctx.lineTo(w-pad.r,h-pad.b); ctx.stroke();
    var stepX = history.length===1 ? plotW/2 : plotW/(history.length-1);
    ctx.strokeStyle = '#2563eb'; ctx.lineWidth = 2; ctx.beginPath();
    for(i=0;i<history.length;i++){
      var x = pad.l + (history.length===1 ? plotW/2 : stepX*i);
      var y2 = pad.t + plotH*(1 - (vals[i]/max));
      if(i===0) ctx.moveTo(x,y2); else ctx.lineTo(x,y2);
    }
    ctx.stroke();
    for(i=0;i<history.length;i++){
      var x2 = pad.l + (history.length===1 ? plotW/2 : stepX*i);
      var y3 = pad.t + plotH*(1 - (vals[i]/max));
      ctx.beginPath(); ctx.arc(x2,y3,4,0,Math.PI*2); ctx.fillStyle = '#16a34a'; ctx.fill();
      ctx.fillStyle = '#667085'; ctx.fillText(String(i+1), x2-4, h-12);
    }
  }

  function exportHistory(){
    var data = JSON.stringify(loadHistory(), null, 2);
    var blob = new Blob([data], {type:'application/json'});
    var url = URL.createObjectURL(blob);
    var a = document.createElement('a');
    a.href = url; a.download = 'soru-sayac-gecmis.json';
    document.body.appendChild(a); a.click(); document.body.removeChild(a);
    setTimeout(function(){ URL.revokeObjectURL(url); }, 500);
  }

  function importHistory(file){
    var reader = new FileReader();
    reader.onload = function(){
      try{
        var arr = JSON.parse(reader.result);
        if(!Array.isArray(arr)) throw new Error('bad');
        saveHistory(arr.concat(loadHistory()));
        renderHistory(); drawChart();
        alert('Veriler içe aktarıldı.');
      } catch(e){ alert('Geçerli bir JSON dosyası seç.'); }
    };
    reader.readAsText(file);
  }

  function tick(){
    if(!state.running) return;
    el.timer.textContent = fmt(questionElapsed(), true);
    el.tot.textContent = fmt(totalElapsed(), false);
  }

  function handleTap(ev){
    if(ev){ ev.preventDefault(); ev.stopPropagation(); }
    nextQuestion();
  }

  el.startBtn.addEventListener('click', startSession);
  el.pauseBtn.addEventListener('click', togglePause);
  el.undoBtn.addEventListener('click', undoLast);
  el.resetBtn.addEventListener('click', hardReset);
  el.exportBtn.addEventListener('click', exportHistory);
  el.importBtn.addEventListener('click', function(){ el.fileInput.click(); });
  el.fileInput.addEventListener('change', function(e){ var f = e.target.files && e.target.files[0]; if(f) importHistory(f); e.target.value=''; });
  el.clearBtn.addEventListener('click', function(){ if(confirm('Tüm kayıtlı çalışmaları silmek istiyor musun?')){ saveHistory([]); renderHistory(); drawChart(); } });
  el.installNoteBtn.addEventListener('click', function(){ alert('Safari ile aç. Paylaş menüsünden “Ana Ekrana Ekle” seç. WhatsApp veya Dosyalar içindeki önizlemede değil, doğrudan Safari içinde kullan.'); });

  el.tapBox.addEventListener('click', handleTap, false);
  el.tapBox.addEventListener('pointerup', handleTap, false);
  el.tapBox.addEventListener('touchend', handleTap, {passive:false});
  el.tapBox.addEventListener('keyup', function(e){ if(e.key === 'Enter' || e.key === ' '){ handleTap(e); } });

  hardReset();
  renderHistory();
  drawChart();
})();
</script>
</body>
</html>
