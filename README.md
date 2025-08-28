# -SITE
<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Visualizador de PDF – Agosto Lilás 2025</title>
  <meta name="description" content="Site simples para abrir e visualizar um arquivo PDF, com opção de upload e download.">
  <style>
    :root {
      --bg: #0f172a; /* slate-900 */
      --card: #111827; /* gray-900 */
      --muted: #94a3b8; /* slate-400 */
      --accent: #a78bfa; /* violet-400 */
      --accent-strong: #7c3aed; /* violet-600 */
      --ring: #22d3ee; /* cyan-400 */
    }
    html, body {
      height: 100%;
    }
    body {
      margin: 0;
      font-family: system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, "Helvetica Neue", Noto Sans, Arial, "Apple Color Emoji", "Segoe UI Emoji";
      background: radial-gradient(1200px 800px at 20% 10%, rgba(167,139,250,.15), transparent 50%),
                  radial-gradient(800px 600px at 80% 0%, rgba(34,211,238,.12), transparent 50%),
                  var(--bg);
      color: #e5e7eb; /* gray-200 */
      display: grid;
      grid-template-rows: auto 1fr;
      gap: 14px;
    }
    header {
      backdrop-filter: blur(6px);
      background: linear-gradient(to right, rgba(17,24,39,.7), rgba(17,24,39,.4));
      border-bottom: 1px solid rgba(148,163,184,.15);
      padding: 14px clamp(12px, 3vw, 24px);
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      gap: 10px 16px;
    }
    h1 {
      font-size: clamp(18px, 2.2vw, 22px);
      margin: 0;
      letter-spacing: .2px;
    }
    .controls {
      margin-left: auto;
      display: flex;
      align-items: center;
      gap: 10px;
      flex-wrap: wrap;
    }
    .btn, label.file-btn {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 10px 14px;
      border-radius: 14px;
      border: 1px solid rgba(148,163,184,.25);
      background: linear-gradient(180deg, rgba(30,41,59,.9), rgba(15,23,42,.9));
      color: #e5e7eb;
      text-decoration: none;
      cursor: pointer;
      transition: transform .05s ease, border-color .2s ease, box-shadow .2s ease;
      box-shadow: 0 1px 0 rgba(255,255,255,.06) inset, 0 8px 20px rgba(124,58,237,.06);
      user-select: none;
    }
    .btn:hover, label.file-btn:hover { border-color: rgba(167,139,250,.65); }
    .btn:active, label.file-btn:active { transform: translateY(1px); }
    .btn.primary { border-color: rgba(124,58,237,.7); box-shadow: 0 0 0 2px rgba(124,58,237,.15); }
    .hint { color: var(--muted); font-size: 13px; }
    .viewer-wrap {
      height: calc(100vh - 90px);
      padding: 0 clamp(10px, 3vw, 18px) clamp(10px, 3vw, 16px);
    }
    .viewer {
      height: 100%;
      width: 100%;
      border: 1px solid rgba(148,163,184,.2);
      border-radius: 16px;
      overflow: hidden;
      background: var(--card);
    }
    iframe { width: 100%; height: 100%; border: 0; background: #111827; }
    input[type="file"] { display: none; }
    .name { font-size: 13px; color: var(--muted); max-width: 38ch; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
    .sep { width: 1px; height: 28px; background: rgba(148,163,184,.25); }
  </style>
</head>
<body>
  <header>
    <h1>Visualizador de PDF</h1>
    <div class="controls" role="group" aria-label="Controles do visualizador">
      <label class="file-btn" for="file-input" title="Abrir um PDF do seu dispositivo">
        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 5 17 10"/><line x1="12" y1="5" x2="12" y2="19"/></svg>
        Abrir PDF
      </label>
      <input id="file-input" type="file" accept="application/pdf" />
      <span class="name" id="file-name" aria-live="polite"></span>
      <span class="sep" aria-hidden="true"></span>
      <a id="download-link" class="btn" href="agosto-lilas-2025-cartilha-online%20(1).pdf" download title="Baixar o PDF atual">
        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 5 17 10"/><line x1="12" y1="5" x2="12" y2="19"/></svg>
        Download
      </a>
      <a id="open-native" class="btn primary" href="agosto-lilas-2025-cartilha-online%20(1).pdf" target="_blank" rel="noopener" title="Abrir o PDF em nova aba">
        Abrir em aba nova
      </a>
    </div>
    <div class="hint">Coloque o arquivo PDF na mesma pasta deste <em>index.html</em>. Um exemplo já está configurado.</div>
  </header>

  <main class="viewer-wrap">
    <div class="viewer" role="region" aria-label="Área do documento PDF">
      <!-- Usa o visualizador nativo do navegador. Para melhor compatibilidade, mantenha o PDF na mesma pasta. -->
      <iframe id="pdf-frame" title="Visualizador de PDF" src="agosto-lilas-2025-cartilha-online%20(1).pdf#toolbar=1&navpanes=0&view=FitH"></iframe>
    </div>
  </main>

  <script>
    (function(){
      const input = document.getElementById('file-input');
      const frame = document.getElementById('pdf-frame');
      const nameEl = document.getElementById('file-name');
      const dl = document.getElementById('download-link');
      const nativeOpen = document.getElementById('open-native');
      let currentObjectUrl = null;

      function setSrc(url, filename){
        frame.src = url + '#toolbar=1&navpanes=0&view=FitH';
        dl.href = url;
        nativeOpen.href = url;
        nameEl.textContent = filename ? filename : '';
      }

      input.addEventListener('change', () => {
        const file = input.files && input.files[0];
        if(!file) return;
        if(file.type !== 'application/pdf'){
          alert('Por favor, selecione um arquivo PDF.');
          input.value = '';
          return;
        }
        if(currentObjectUrl) URL.revokeObjectURL(currentObjectUrl);
        currentObjectUrl = URL.createObjectURL(file);
        setSrc(currentObjectUrl, file.name);
      });

      // Se o PDF padrão não carregar (ex.: nome diferente), mostra uma dica.
      frame.addEventListener('error', () => {
        nameEl.textContent = 'Não foi possível carregar o PDF padrão. Use "Abrir PDF".';
      });
    })();
  </script>
</body>
</html>
