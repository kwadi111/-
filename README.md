<html lang="ru">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>Удостоверение личности</title>
<style>
  :root{
    --blue:#1e9ff2; --blue-700:#138ad6;
    --ink:#111418; --muted:#6b7280;
    --bg:#f2f4f7; --card:#ffffff; --border:#e6e9ee;
    --tabs-bg:#e8ebee;
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{margin:0;background:#000;font:15px/1.45 -apple-system,BlinkMacSystemFont,Segoe UI,Roboto,Inter,Arial,sans-serif;color:var(--ink)}
  .phone{width:390px;height:844px;margin:0 auto;background:var(--bg);display:flex;flex-direction:column;position:relative}

  /* статусбар */
  .statusbar{height:44px;padding:0 12px;display:flex;align-items:center;justify-content:space-between;color:#000;font-weight:600;font-size:13px}
  .sb-right{display:flex;gap:6px}.dot{width:7px;height:7px;border-radius:50%;background:#000;opacity:.9}

  /* верхняя панель */
  .appbar{position:relative;background:var(--card);border-bottom:1px solid var(--border);height:56px;display:flex;align-items:center;justify-content:center}
  .back{position:absolute;left:12px;top:50%;transform:translateY(-50%);width:28px;height:28px;display:grid;place-items:center;cursor:pointer}
  .back svg{width:18px;height:18px}
  .title{font-weight:700;font-size:17px;white-space:nowrap}

  /* область вкладок */
  .tabs-area{background:var(--tabs-bg);padding:10px 0;border-bottom:1px solid var(--border)}
  .tabs{display:flex;justify-content:center;background:var(--tabs-bg);border-radius:12px;gap:4px;padding:4px;max-width:320px;margin:0 auto}
  .tab{flex:1;padding:9px 0;border-radius:10px;font-weight:700;color:var(--ink);opacity:.65;background:transparent;border:none;cursor:pointer}
  .tab.active{background:#fff;opacity:1;box-shadow:0 1px 3px rgba(0,0,0,.08)}

  /* контент */
  .content{padding:12px}
  /* Документ */
  .id-card{background:var(--card);border:1px solid var(--border);border-radius:18px;box-shadow:0 2px 8px rgba(17,20,24,.06);overflow:hidden;margin-bottom:14px}
  .frame{aspect-ratio:5/3;width:100%;background:linear-gradient(180deg,#f7fafc,#eef2f7);display:flex;align-items:center;justify-content:center;position:relative;cursor:pointer}
  .hint{position:absolute;inset:auto 0 10px 0;text-align:center;color:var(--muted);font-size:12px}
  .frame img{max-width:96%;max-height:96%;border-radius:12px;display:none}
  .hidden-input{position:absolute;inset:0;opacity:0;cursor:pointer}

  /* Реквизиты */
  .list{background:#fff;border:1px solid var(--border);border-radius:16px;padding:6px 8px}
  .row{display:grid;grid-template-columns:1fr auto;gap:8px;padding:10px 8px;border-bottom:1px solid #f1f3f6}
  .row:last-child{border-bottom:none}
  .label{font-size:12px;color:var(--muted);margin-bottom:2px}
  .value{display:flex;align-items:center;gap:8px}
  .value input{width:100%;border:none;background:transparent;padding:0;margin:0;font:15px/1.45 inherit;color:inherit}
  .value input:focus{outline:none}
  .copy{border:1px solid var(--border);background:#fff;border-radius:9px;width:34px;height:34px;display:grid;place-items:center;cursor:pointer}
  .copy svg{width:18px;height:18px;stroke:#657080}

  /* нижние кнопки */
  .footer{position:fixed;left:0;right:0;bottom:0;background:#fff;border-top:1px solid var(--border);padding:12px 12px 24px;margin:0 auto;max-width:390px}
  .btn{width:100%;border-radius:12px;padding:16px 18px;font-weight:700;font-size:16px;display:flex;align-items:center;justify-content:center;gap:8px;border:0}
  .btn svg{width:20px;height:20px}
  .btn-primary{background:var(--blue);color:#fff}
  .btn-primary:active{background:var(--blue-700)}
  .btn-outline{background:#fff;color:var(--ink);border:1.5px solid var(--border)}
  .link{display:block;text-align:center;margin-top:10px;color:var(--blue);text-decoration:none;font-weight:600}

  /* тост */
  .toast{position:fixed;left:12px;right:12px;bottom:96px;background:#0b3e16;color:#c5ffd1;border-radius:10px;padding:10px 12px;font-size:13px;display:none;margin:0 auto;max-width:390px}
  .toast.err{background:#5b1212;color:#ffd6d6}
</style>
</head>
<body>
  <div class="phone">
    <div class="statusbar">
      <div>11:53</div>
      <div class="sb-right"><div class="dot"></div><div class="dot"></div><div class="dot"></div></div>
    </div>

    <div class="appbar">
      <div class="back" onclick="history.back()">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M15 18l-6-6 6-6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </div>
      <div class="title">Удостоверение личности</div>
    </div>

    <!-- Вкладки -->
    <div class="tabs-area">
      <div class="tabs">
        <button class="tab active" id="tab-doc" type="button">Документ</button>
        <button class="tab" id="tab-req" type="button">Реквизиты</button>
      </div>
    </div>

    <!-- Контент -->
    <div class="content">
      <!-- Документ -->
      <section id="panel-doc">
        <div class="id-card">
          <div class="frame" id="frontFrame">
            <input class="hidden-input" id="frontInput" type="file" accept="image/jpeg,image/png">
            <img id="frontPreview" alt="Лицевая сторона"/>
            <div class="hint">Лицевая сторона — нажмите для выбора фото</div>
          </div>
        </div>
        <div class="id-card">
          <div class="frame" id="backFrame">
            <input class="hidden-input" id="backInput" type="file" accept="image/jpeg,image/png">
            <img id="backPreview" alt="Оборотная сторона"/>
            <div class="hint">Оборотная сторона — нажмите для выбора фото</div>
          </div>
        </div>
      </section>

      <!-- Реквизиты -->
      <section id="panel-req" hidden>
        <div class="list">
          <div class="row">
            <div>
              <div class="label">ФИО</div>
              <div class="value"><input id="fio" placeholder="Иванов Иван Иванович"></div>
            </div>
            <button class="copy" data-copy="fio" title="Копировать">
              <svg viewBox="0 0 24 24" fill="none"><rect x="9" y="9" width="11" height="11" rx="2" stroke-width="2"/><rect x="4" y="4" width="11" height="11" rx="2" stroke-width="2"/></svg>
            </button>
          </div>

          <div class="row">
            <div>
              <div class="label">ИИН</div>
              <div class="value"><input id="iin" inputmode="numeric" pattern="[0-9]*" placeholder="000000000000"></div>
            </div>
            <button class="copy" data-copy="iin" title="Копировать">
              <svg viewBox="0 0 24 24" fill="none"><rect x="9" y="9" width="11" height="11" rx="2" stroke-width="2"/><rect x="4" y="4" width="11" height="11" rx="2" stroke-width="2"/></svg>
            </button>
          </div>

          <div class="row">
            <div>
              <div class="label">Дата рождения</div>
              <div class="value"><input id="dob" type="date"></div>
            </div>
            <button class="copy" data-copy="dob" title="Копировать">
              <svg viewBox="0 0 24 24" fill="none"><rect x="9" y="9" width="11" height="11" rx="2" stroke-width="2"/><rect x="4" y="4" width="11" height="11" rx="2" stroke-width="2"/></svg>
            </button>
          </div>

          <div class="row">
            <div>
              <div class="label">Номер документа</div>
              <div class="value"><input id="docnum" placeholder="XXXXXXXXX"></div>
            </div>
            <button class="copy" data-copy="docnum" title="Копировать">
              <svg viewBox="0 0 24 24" fill="none"><rect x="9" y="9" width="11" height="11" rx="2" stroke-width="2"/><rect x="4" y="4" width="11" height="11" rx="2" stroke-width="2"/></svg>
            </button>
          </div>

          <div class="row">
            <div>
              <div class="label">Дата выдачи</div>
              <div class="value"><input id="issued" type="date"></div>
            </div>
            <button class="copy" data-copy="issued" title="Копировать">
              <svg viewBox="0 0 24 24" fill="none"><rect x="9" y="9" width="11" height="11" rx="2" stroke-width="2"/><rect x="4" y="4" width="11" height="11" rx="2" stroke-width="2"/></svg>
            </button>
          </div>

          <div class="row">
            <div>
              <div class="label">Срок действия</div>
              <div class="value"><input id="expiry" type="date"></div>
            </div>
            <button class="copy" data-copy="expiry" title="Копировать">
              <svg viewBox="0 0 24 24" fill="none"><rect x="9" y="9" width="11" height="11" rx="2" stroke-width="2"/><rect x="4" y="4" width="11" height="11" rx="2" stroke-width="2"/></svg>
            </button>
          </div>
        </div>
      </section>
    </div>

    <!-- нижние кнопки -->
    <div class="footer">
      <button class="btn btn-primary" id="action-doc">
        <!-- QR иконка -->
        <svg viewBox="0 0 24 24" fill="none">
          <path d="M3 3h7v7H3V3zm2 2v3h3V5H5z" fill="#fff"/>
          <path d="M14 3h7v7h-7V3zm2 2v3h3V5h-3z" fill="#fff"/>
          <path d="M3 14h7v7H3v-7zm2 2v3h3v-3H5z" fill="#fff"/>
          <path d="M14 14h3v3h-3zM17 17h4v4h-4zM21 14h-2v-2h2zM14 21h-2v-2h2z" fill="#fff"/>
        </svg>
        Предъявить документ
      </button>
      <button class="btn btn-outline" id="action-req" hidden>
        <!-- иконка «поделиться» -->
        <svg viewBox="0 0 24 24" fill="none" stroke="#111418" stroke-width="2" stroke-linejoin="round">
          <path d="M22 2L11 13"/><path d="M22 2L15 22L11 13L2 9L22 2Z"/>
        </svg>
        Отправить реквизиты
      </button>
    </div>

    <div class="toast" id="toast" role="status" aria-live="polite"></div>
  </div>

<script>
  const MAX_SIZE = 10 * 1024 * 1024;
  const ACCEPT = ['image/jpeg','image/png'];
  const $ = id => document.getElementById(id);
  const toast = (m, ok=true)=>{
    const t=$('toast'); t.textContent=m; t.classList.toggle('err',!ok);
    t.style.display='block'; clearTimeout(t._to); t._to=setTimeout(()=>t.style.display='none',2200);
  };

  // переключение вкладок
  const tabDoc = $('tab-doc'), tabReq = $('tab-req');
  const panelDoc = $('panel-doc'), panelReq = $('panel-req');
  const actionDoc = $('action-doc'), actionReq = $('action-req');

  tabDoc.addEventListener('click', ()=>{
    tabDoc.classList.add('active'); tabReq.classList.remove('active');
    panelDoc.hidden = false; panelReq.hidden = true;
    actionDoc.hidden = false; actionReq.hidden = true;
  });
  tabReq.addEventListener('click', ()=>{
    tabReq.classList.add('active'); tabDoc.classList.remove('active');
    panelDoc.hidden = true; panelReq.hidden = false;
    actionDoc.hidden = true; actionReq.hidden = false;
  });

  // загрузка изображений
  function bindUpload(frameId, inputId, previewId){
    const frame = $(frameId), input = $(inputId), preview = $(previewId);
    const validate = (file)=>{
      if(!file) return 'Файл не выбран';
      if(!ACCEPT.includes(file.type)) return 'Допустимы JPG/PNG';
      if(file.size>MAX_SIZE) return 'Файл больше 10 МБ';
      return null;
    };
    frame.addEventListener('click', ()=>input.click());
    input.addEventListener('change', ()=>{
      const f = input.files[0]; const err = validate(f);
      if(err) return toast(err,false);
      const url = URL.createObjectURL(f); preview.src = url; preview.style.display='block';
      frame.querySelector('.hint').style.display='none';
    });
  }
  bindUpload('frontFrame','frontInput','frontPreview');
  bindUpload('backFrame','backInput','backPreview');

  // действие по кнопке Документ
  actionDoc.addEventListener('click', ()=>{
    const ok = $('frontInput').files.length && $('backInput').files.length;
    toast(ok ? 'Документ готов к предъявлению ✅' : 'Загрузите обе стороны документа', ok);
  });

  // копирование в реквизитах
  document.querySelectorAll('.copy').forEach(btn=>{
    btn.addEventListener('click', async ()=>{
      const id = btn.getAttribute('data-copy');
      const el = $(id);
      try{
        await navigator.clipboard.writeText(el.value || '');
        toast('Скопировано', true);
      }catch{ toast('Не удалось скопировать', false); }
    });
  });

  // отправка реквизитов (демо)
  actionReq.addEventListener('click', ()=>{
    const data = {
      fio: $('fio').value.trim(),
      iin: $('iin').value.trim(),
      dob: $('dob').value,
      docnum: $('docnum').value.trim(),
      issued: $('issued').value,
      expiry: $('expiry').value
    };
    console.log('Реквизиты к отправке:', data);
    toast('Реквизиты подготовлены к отправке (демо). Подключите сервер.', true);

    // Пример реальной отправки:
    // fetch('/api/send-requisites',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify(data)})
    //   .then(r=>r.ok?toast('Отправлено. Спасибо!',true):Promise.reject())
    //   .catch(()=>toast('Ошибка отправки',false));
  });
</script>
</body>
</html>
