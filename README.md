<html lang="ru">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>Удостоверение личности</title>
<style>
  :root{
    --kaspi-blue:#1e9ff2;      /* основной синий (похож на Kaspi) */
    --kaspi-blue-700:#138ad6;
    --ink:#111418;
    --muted:#6b7280;
    --bg:#f2f4f7;
    --card:#ffffff;
    --border:#e6e9ee;
    --radius:16px;
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    margin:0;background:#000; /* как в видео – тёмные поля по бокам */
    font:15px/1.45 -apple-system,BlinkMacSystemFont,Segoe UI,Roboto,Inter,Arial,sans-serif;
    color:var(--ink);
  }

  /* Мобильная «рамка» */
  .phone{
    width:390px;               /* ширина iPhone 12/13/14 */
    height:844px;              /* примерная высота */
    margin:0 auto;
    background:var(--bg);
    display:flex;flex-direction:column;
    position:relative;
  }

  /* Статус-бар (в верхней части) */
  .statusbar{
    height:44px;padding:0 12px;
    display:flex;align-items:center;justify-content:space-between;
    color:#000; font-weight:600; font-size:13px;
  }
  .sb-left,.sb-right{display:flex;align-items:center;gap:6px}
  .dot{width:7px;height:7px;border-radius:50%;background:#000;opacity:.9}

  /* App Bar */
  .appbar{
    background:var(--card);border-bottom:1px solid var(--border);
    height:56px;display:flex;align-items:center;gap:8px;padding:0 12px;
  }
  .back{width:28px;height:28px;border-radius:999px;display:grid;place-items:center;cursor:pointer}
  .back svg{width:18px;height:18px}
  .title{font-weight:700;font-size:17px;margin-left:4px}

  /* Tabs */
  .tabs{display:flex;gap:8px;padding:10px 12px 0}
  .tab{
    flex:0 0 auto;padding:8px 14px;border-radius:999px;font-weight:600;
    border:1px solid var(--border);background:var(--card);color:var(--ink);cursor:pointer;
  }
  .tab.active{background:var(--kaspi-blue);border-color:var(--kaspi-blue);color:#fff}

  /* Контент */
  .content{padding:12px}
  .id-card{
    background:var(--card);border:1px solid var(--border);border-radius:18px;
    box-shadow:0 2px 8px rgba(17,20,24,.06);
    overflow:hidden; margin-bottom:14px; position:relative;
  }
  .id-card .frame{
    aspect-ratio: 5 / 3;       /* вытянутый прямоугольник как у удостоверения */
    width:100%; background:linear-gradient(180deg,#f7fafc,#eef2f7);
    display:flex;align-items:center;justify-content:center;
    position:relative; cursor:pointer;
  }
  .hint{
    position:absolute;inset:auto 0 10px 0;text-align:center;color:var(--muted);font-size:12px;
  }
  .frame img{max-width:96%;max-height:96%;border-radius:12px;display:none}
  .frame.drag{outline:3px dashed var(--kaspi-blue);background:#fff}

  .hidden-input{position:absolute;inset:0;opacity:0;cursor:pointer}

  /* Нижняя панель с кнопками */
  .footer{
    margin-top:auto; padding:14px 12px 28px;
    background:transparent;
  }
  .primary{
    width:100%;border:0;border-radius:12px;padding:16px 18px;
    font-weight:700;font-size:16px;color:#fff;background:var(--kaspi-blue);cursor:pointer;
  }
  .primary:active{background:var(--kaspi-blue-700)}
  .link{display:block;text-align:center;margin-top:10px;color:var(--kaspi-blue);text-decoration:none;font-weight:600}

  .toast{position:absolute;left:12px;right:12px;bottom:96px;background:#0b3e16;color:#c5ffd1;
         border-radius:10px;padding:10px 12px;font-size:13px;display:none}
  .toast.err{background:#5b1212;color:#ffd6d6}
</style>
</head>
<body>
  <div class="phone" id="phone">
    <!-- Статус-бар (упрощённый) -->
    <div class="statusbar">
      <div class="sb-left">9:16</div>
      <div class="sb-right"><div class="dot"></div><div class="dot"></div><div class="dot"></div></div>
    </div>

    <!-- AppBar -->
    <div class="appbar">
      <div class="back" aria-label="Назад" onclick="history.back()">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M15 18l-6-6 6-6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </div>
      <div class="title">Удостоверение личности</div>
    </div>

    <!-- Tabs -->
    <div class="tabs">
      <button class="tab active" type="button">Документ</button>
      <button class="tab" type="button" disabled>Реквизиты</button>
    </div>

    <!-- Content -->
    <div class="content">
      <!-- Карточка 1 -->
      <div class="id-card">
        <div class="frame" id="frontFrame">
          <input class="hidden-input" id="frontInput" type="file" accept="image/jpeg,image/png">
          <img id="frontPreview" alt="Лицевая сторона"/>
          <div class="hint">Лицевая сторона — нажмите для выбора фото</div>
        </div>
      </div>

      <!-- Карточка 2 -->
      <div class="id-card">
        <div class="frame" id="backFrame">
          <input class="hidden-input" id="backInput" type="file" accept="image/jpeg,image/png">
          <img id="backPreview" alt="Оборотная сторона"/>
          <div class="hint">Оборотная сторона — нажмите для выбора фото</div>
        </div>
      </div>
    </div>

    <!-- Bottom buttons -->
    <div class="footer">
      <button class="primary" id="showBtn" type="button">Предъявить документ</button>
      <a href="#" id="sendLink" class="link">Отправить документ</a>
    </div>

    <div class="toast" id="toast" role="status" aria-live="polite"></div>
  </div>

<script>
  const MAX_SIZE = 10 * 1024 * 1024; // 10MB
  const ACCEPT = ['image/jpeg','image/png'];
  const t = (id)=>document.getElementById(id);
  const toast = (msg, ok=true)=>{
    const el=t('toast'); el.textContent=msg; el.classList.toggle('err',!ok);
    el.style.display='block'; clearTimeout(el._to); el._to=setTimeout(()=>el.style.display='none',2200);
  };

  function bindUpload(frameId, inputId, previewId){
    const frame = t(frameId), input = t(inputId), preview = t(previewId);

    const validate = (file)=>{
      if(!file) return 'Файл не выбран';
      if(!ACCEPT.includes(file.type)) return 'Допустимы JPG/PNG';
      if(file.size>MAX_SIZE) return 'Файл больше 10 МБ';
      return null;
    };

    frame.addEventListener('dragover',(e)=>{e.preventDefault(); frame.classList.add('drag');});
    frame.addEventListener('dragleave',()=>frame.classList.remove('drag'));
    frame.addEventListener('drop',(e)=>{
      e.preventDefault(); frame.classList.remove('drag');
      const f = e.dataTransfer.files[0]; const err = validate(f);
      if(err) return toast(err,false);
      const url = URL.createObjectURL(f);
      preview.src = url; preview.style.display='block';
      frame.querySelector('.hint').style.display='none';
    });

    input.addEventListener('change',()=>{
      const f = input.files[0]; const err = validate(f);
      if(err) return toast(err,false);
      const url = URL.createObjectURL(f);
      preview.src = url; preview.style.display='block';
      frame.querySelector('.hint').style.display='none';
    });

    // клик по области открывает выбор файлов
    frame.addEventListener('click',()=>input.click());
  }

  bindUpload('frontFrame','frontInput','frontPreview');
  bindUpload('backFrame','backInput','backPreview');

  // Кнопки
  t('showBtn').addEventListener('click', ()=>{
    const fOK = t('frontInput').files.length>0;
    const bOK = t('backInput').files.length>0;
    if(!fOK || !bOK){ toast('Загрузите обе стороны документа', false); return; }
    toast('Документ готов к предъявлению ✅', true);
  });

  t('sendLink').addEventListener('click', (e)=>{
    e.preventDefault();
    const f = t('frontInput').files[0], b = t('backInput').files[0];
    if(!f || !b){ toast('Загрузите обе стороны документа', false); return; }

    // подготовка формы (пример)
    const fd = new FormData();
    fd.append('front', f);
    fd.append('back', b);

    // ЗАМЕНИТЕ на адрес вашего бэкенда:
    const UPLOAD_URL = '/api/upload-id';

    // демо-режим: не отправляем, только показываем тост
    console.log('Готово к отправке на', UPLOAD_URL, [...fd.entries()]);
    toast('Данные готовы к отправке (демо). Подключите сервер.', true);

    // Для реальной отправки раскомментируйте:
    // fetch(UPLOAD_URL, {method:'POST', body:fd}).then(r=>{
    //   if(!r.ok) throw new Error();
    //   toast('Отправлено. Спасибо!', true);
    // }).catch(()=>toast('Ошибка отправки', false));
  });
</script>
</body>
</html>
