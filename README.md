<!doctype html>
<html lang="ru">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>Удостоверение личности</title>
<style>
  :root {
    --kaspi-blue:#007BFF;
    --kaspi-blue-dark:#0062d1;
    --gray-border:#e0e0e0;
    --gray-bg:#f7f7f7;
    --text-main:#1a1a1a;
    --text-muted:#6b6b6b;
  }
  * { box-sizing: border-box; }
  body {
    margin: 0;
    font-family: -apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Inter,Arial,sans-serif;
    background-color: #fff;
    color: var(--text-main);
  }
  header {
    display: flex;
    align-items: center;
    gap: 10px;
    height: 56px;
    border-bottom: 1px solid var(--gray-border);
    padding: 0 12px;
  }
  header svg {
    width: 24px; height: 24px;
    stroke: #000;
    cursor: pointer;
  }
  header h1 {
    font-size: 17px;
    font-weight: 600;
    flex: 1;
    text-align: center;
    margin: 0;
  }
  .tabs {
    display: flex;
    margin: 12px;
    background: var(--gray-bg);
    border-radius: 8px;
    overflow: hidden;
  }
  .tab {
    flex: 1;
    text-align: center;
    padding: 10px 0;
    font-weight: 600;
    color: var(--text-muted);
    cursor: pointer;
  }
  .tab.active {
    background: #fff;
    color: var(--text-main);
    box-shadow: 0 0 0 1px var(--gray-border);
  }
  .doc-image {
    display: flex;
    justify-content: center;
    padding: 16px;
  }
  .doc-image img {
    width: 90%;
    border-radius: 12px;
    border: 1px solid var(--gray-border);
    box-shadow: 0 1px 4px rgba(0,0,0,0.1);
  }
  .buttons {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    background: #fff;
    padding: 16px 12px 28px;
    box-shadow: 0 -2px 6px rgba(0,0,0,0.05);
  }
  .btn {
    width: 100%;
    border-radius: 10px;
    font-weight: 600;
    padding: 14px 0;
    font-size: 16px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
  }
  .btn-primary {
    background: var(--kaspi-blue);
    color: #fff;
    border: none;
    margin-bottom: 10px;
  }
  .btn-primary:hover { background: var(--kaspi-blue-dark); }
  .btn-outline {
    border: 1.5px solid var(--gray-border);
    color: var(--text-main);
    background: #fff;
  }
  .btn svg {
    width: 20px;
    height: 20px;
    stroke: currentColor;
  }
</style>
</head>
<body>

<header>
  <svg fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" viewBox="0 0 24 24">
    <path d="M15 18l-6-6 6-6"/>
  </svg>
  <h1>Удостоверение личности</h1>
</header>

<div class="tabs">
  <div class="tab active">Документ</div>
  <div class="tab">Реквизиты</div>
</div>

<div class="doc-image">
  <img id="docPreview" src="https://via.placeholder.com/600x400.png?text=Фото+удостоверения" alt="Фото удостоверения">
</div>

<div class="buttons">
  <button class="btn btn-primary" onclick="documentUpload()">
    <svg viewBox="0 0 24 24" fill="none"><path d="M9 12h6m-3-3v6m-9 3a9 9 0 1 0 18 0 9 9 0 0 0-18 0z"/></svg>
    Предъявить документ
  </button>
  <button class="btn btn-outline" onclick="sendDocument()">
    <svg viewBox="0 0 24 24" fill="none"><path d="M22 2L11 13" /><path d="M22 2L15 22L11 13L2 9L22 2Z" /></svg>
    Отправить документ
  </button>
</div>

<input type="file" accept="image/*" id="fileInput" style="display:none" />

<script>
  const fileInput = document.getElementById('fileInput');
  const docPreview = document.getElementById('docPreview');

  // По клику на фото открываем выбор файла
  docPreview.addEventListener('click', () => fileInput.click());
  fileInput.addEventListener('change', e => {
    const file = e.target.files[0];
    if (!file) return;
    const url = URL.createObjectURL(file);
    docPreview.src = url;
  });

  function documentUpload() {
    alert('Документ готов к предъявлению ✅');
  }

  function sendDocument() {
    alert('Данные подготовлены к отправке (демо).');
  }
</script>

</body>
</html>
