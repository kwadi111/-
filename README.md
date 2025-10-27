<html lang="ru">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover,maximum-scale=1,user-scalable=no">
<meta name="theme-color" content="#ffffff">
<title>Удостоверение личности</title>
<style>
  :root{
    --blue:#007BFF; --blue-700:#0062d1;
    --ink:#1a1a1a; --muted:#6b6b6b;
    --bg:#ffffff; --chip:#f2f3f5; --border:#e5e7eb;
    --radius:12px;
    --safe-top: env(safe-area-inset-top);
    --safe-bottom: env(safe-area-inset-bottom);
  }
  *{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
  html,body{height:100%}
  body{
    margin:0;background:var(--bg);color:var(--ink);
    font:16px/1.45 -apple-system,BlinkMacSystemFont,Segoe UI,Roboto,Inter,Arial,sans-serif;
  }

  /* Контейнер страницы (мобильный) */
  .screen{
    min-height:100dvh; display:flex; flex-direction:column;
    padding-bottom: calc(16px + var(--safe-bottom));
  }

  /* AppBar */
  .appbar{
    padding: calc(6px + var(--safe-top)) 12px 8px;
    display:grid; grid-template-columns:40px 1fr 40px; align-items:center;
    border-bottom:1px solid var(--border); background:#fff;
  }
  .back{display:grid;place-items:center}
  .back svg{width:24px;height:24px}
  .title{font-weight:700;text-align:center}

  /* Вкладки-сегменты */
  .tabs{
    margin:12px; background:var(--chip); border-radius:10px; display:flex; overflow:hidden;
  }
  .tab{flex:1;padding:10px 0;text-align:center;font-weight:700;color:var(--muted)}
  .tab.active{background:#fff;color:var(--ink);box-shadow:0 0 0 1px var(--border) inset}

  /* Контент */
  .content{padding:12px 12px 0}
  .card{
    border:1px solid var(--border); border-radius:16px; padding:10px; background:#fff;
  }
  .frame{
    width:100%; aspect-ratio: 16/10; border-radius:12px; border:1px solid var(--border);
    background:linear-gradient(180deg,#f6f8fb,#e9eef6);
    position:relative; overflow:hidden;
  }
  .frame img{position:absolute; inset:0; width:100%; height:100%; object-fit:cover; display:none}
  .placeholder{position:absolute; inset:0; display:flex; align-items:center; justify-content:center; color:var(--muted); font-weight:600}
  .hint{position:absolute; bottom:8px; left:0; right:0; text-align:center; font-size:12px; color:var(--muted)}
  .hidden{position:absolute; inset:0; opacity:0}

  /* Нижняя панель */
  .bottom{
    margin-top:auto; padding:12px 12px calc(16px + var(--safe-bottom));
    background:#fff; box-shadow:0 -2px 8px rgba(0,0,0,.04);
  }
  .btn{
    width:100%; padding:16px; border-radius:12px; font-weight:700; display:flex; align-items:center; justify-content:center; gap:8px;
    border:0; background:var(--blue); color:#fff; font-size:16px;
  }
  .btn:active{background:var(--blue-700)}
  .btn-outline{
    margin-top:10px; background:#fff; color:var(--ink); border:1.5px solid var(--border);
  }
  .btn svg{width:20px;height:20px;stroke:currentColor}

  /* Тост */
  .toast{
    position:fixed; left:12px; right:12px; bottom:calc(96px + var(--safe-bottom));
    background:#0b3e16; color:#c7ffd5; padding:10px 12px; border-radius:10px; font-size:14px; display:none;
  }
  .toast.err{background:#5b1212;color:#ffd6d6}

  /* Адаптация под большие экраны (центрируем «телефон») */
  @media (min-width:480px){
    .screen{max-width:420px;margin:0 auto;border-left:1px solid var(--border);border-right:1px solid var(--border)}
  }
</style>
</head>
<body>
  <div class="screen">
    <!-- Верх -->
    <div class="appbar">
      <div class="back" onclick="history.back()" aria-label="Назад">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 18l-6-6 6-6"/></svg>
      </div>
      <div class="title">Удостоверение личности</div>
      <div></div>
    </div>

    <!-- Вкладки -->
    <div class="tabs">
      <div class="tab active">Документ</div>
      <div class="tab">Реквизиты</div>
    </div>

    <!-- Контент -->
    <div class="content">
      <div class="card">
        <label class="frame" id="docFrame">
          <input id="fileInput" class="hidden" type="file" accept="image/jpeg,image/png,image/heic,image/heif">
          <img id="preview" alt="Фото удостоверения">
          <div class="placeholder">Нажмите, чтобы выбрать фото</div>
          <div class="hint">Поддержка JPG/PNG/HEIC, до 10 МБ</div>
        </label>
      </div>
    </div>

    <!-- Низ -->
    <div class="bottom">
      <button class="btn" id="presentBtn">
        <svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9"/><path d="M12 8v8M8 12h8" stroke-width="2"/></svg>
        Предъявить документ
      </button>
      <button class="btn btn-outline" id="sendBtn">
        <svg viewBox="0 0 24 24" fill="none"><path d="M22 2L11 13"/><path d="M22 2L15 22L11 13L2 9L22 2Z"/></svg>
        Отправить документ
      </button>
    </div>

    <div class="toast" id="toast" role="status" aria-live="polite"></div>
  </div>

<script>
  const MAX = 10 * 1024 * 1024;
  const ACCEPT = ['image/jpeg','image/png','image/heic','image/heif'];
  const $ = id => document.getElementById(id);
  const toast = (m, ok=true)=>{
    const t=$('toast'); t.textContent=m; t.classList.toggle('err',!ok);
    t.style.display='block'; clearTimeout(t._to); t._to=setTimeout(()=>t.style.display='none',2200);
  };

  // Загрузка и предпросмотр
  const input = $('fileInput'), preview=$('preview'), frame=$('docFrame');
  input.addEventListener('change', async ()=>{
    const f = input.files[0]; if(!f) return;
    if(!ACCEPT.includes(f.type)) return toast('Формат: JPG/PNG/HEIC', false);
    if(f.size>MAX) return toast('Файл больше 10 МБ', false);
    const url = URL.createObjectURL(f);
    preview.src = url; preview.style.display='block';
    frame.querySelector('.placeholder').style.display='none';
    frame.querySelector('.hint').style.display='none';
  });

  $('presentBtn').addEventListener('click', ()=>{
    if(!input.files.length) return toast('Загрузите фото документа', false);
    toast('Документ готов к предъявлению ✅', true);
  });

  $('sendBtn').addEventListener('click', (e)=>{
    e.preventDefault();
    if(!input.files.length) return toast('Загрузите фото документа', false);
    const fd = new FormData(); fd.append('document', input.files[0]);
    // Замените на адрес вашего бэкенда:
    const UPLOAD_URL = '/api/upload-id';
    console.log('Готово к отправке на', UPLOAD_URL, [...fd.entries()]);
    toast('Данные готовы к отправке (демо). Подключите сервер.', true);

    // Для реальной отправки:
    // fetch(UPLOAD_URL, {method:'POST', body:fd})
    //   .then(r => r.ok ? toast('Отправлено. Спасибо!', true) : Promise.reject())
    //   .catch(()=>toast('Ошибка отправки', false));
  });
</script>
</body>
</html>
