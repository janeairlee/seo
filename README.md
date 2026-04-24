[workspace-article-generator.html](https://github.com/user-attachments/files/27039349/workspace-article-generator.html)
<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>窩客商務中心 — SEO 文章產生器</title>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --navy: #1a3a5c;
    --navy-light: #243f63;
    --orange: #e8572a;
    --orange-light: #f06a3a;
    --bg: #f4f6f9;
    --surface: #ffffff;
    --surface2: #f0f2f5;
    --border: #dde2ea;
    --text: #1c2535;
    --text2: #5a6478;
    --text3: #9aa3b2;
    --radius: 10px;
    --radius-sm: 6px;
  }

  body {
    font-family: 'Noto Sans TC', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    font-size: 15px;
  }

  /* Header */
  .header {
    background: var(--navy);
    padding: 0 2rem;
    height: 56px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .logo {
    font-size: 17px;
    font-weight: 700;
    color: #fff;
    letter-spacing: .02em;
  }
  .logo span { color: var(--orange); }
  .header-sub { font-size: 12px; color: rgba(255,255,255,.5); }

  /* API Key Banner */
  .api-banner {
    background: #fff8f0;
    border-bottom: 1px solid #fcd9b8;
    padding: .75rem 2rem;
    display: flex;
    align-items: center;
    gap: 12px;
    flex-wrap: wrap;
  }
  .api-banner label { font-size: 13px; color: #8a4a00; font-weight: 500; white-space: nowrap; }
  .api-banner input {
    flex: 1;
    min-width: 260px;
    padding: 7px 12px;
    border: 1px solid #f0b070;
    border-radius: var(--radius-sm);
    font-size: 13px;
    font-family: monospace;
    background: #fff;
    color: var(--text);
    outline: none;
  }
  .api-banner input:focus { border-color: var(--orange); }
  .api-hint { font-size: 12px; color: #a05a00; }
  .api-hint a { color: var(--orange); }

  /* Layout */
  .layout {
    display: grid;
    grid-template-columns: 240px 1fr;
    gap: 0;
    height: calc(100vh - 56px - 52px);
  }

  /* Sidebar */
  .sidebar {
    background: var(--surface);
    border-right: 1px solid var(--border);
    padding: 1.5rem 1.25rem;
    display: flex;
    flex-direction: column;
    gap: 1.1rem;
    overflow-y: auto;
  }
  .sidebar-heading {
    font-size: 11px;
    font-weight: 700;
    letter-spacing: .08em;
    color: var(--text3);
    text-transform: uppercase;
    margin-bottom: .25rem;
  }
  .field { display: flex; flex-direction: column; gap: 5px; }
  .field label { font-size: 13px; color: var(--text2); font-weight: 500; }
  .field select, .field input {
    padding: 8px 10px;
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    font-size: 13px;
    font-family: 'Noto Sans TC', sans-serif;
    color: var(--text);
    background: var(--surface);
    outline: none;
    transition: border-color .15s;
  }
  .field select:focus, .field input:focus { border-color: var(--navy); }
  .divider { border: none; border-top: 1px solid var(--border); }

  .gen-btn {
    width: 100%;
    padding: 11px;
    background: var(--orange);
    color: #fff;
    border: none;
    border-radius: var(--radius-sm);
    font-size: 14px;
    font-weight: 700;
    font-family: 'Noto Sans TC', sans-serif;
    cursor: pointer;
    transition: background .15s, transform .1s;
    margin-top: auto;
  }
  .gen-btn:hover { background: var(--orange-light); }
  .gen-btn:active { transform: scale(.98); }
  .gen-btn:disabled { background: var(--text3); cursor: not-allowed; transform: none; }

  /* Main panel */
  .main {
    display: flex;
    flex-direction: column;
    background: var(--bg);
    overflow: hidden;
  }

  .toolbar {
    background: var(--surface);
    border-bottom: 1px solid var(--border);
    padding: .75rem 1.5rem;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .toolbar-title {
    font-size: 15px;
    font-weight: 500;
    flex: 1;
    color: var(--text);
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    max-width: 500px;
  }
  .chip {
    font-size: 11px;
    padding: 3px 10px;
    background: #e8f5ee;
    color: #1a7a45;
    border-radius: 100px;
    font-weight: 500;
  }
  .tb-btn {
    padding: 6px 14px;
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    background: var(--surface);
    font-size: 13px;
    font-family: 'Noto Sans TC', sans-serif;
    color: var(--text);
    cursor: pointer;
    transition: background .12s;
  }
  .tb-btn:hover { background: var(--surface2); }

  /* Article area */
  .article-scroll {
    flex: 1;
    overflow-y: auto;
    padding: 2rem 2.5rem;
  }

  .placeholder {
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 14px;
    color: var(--text3);
  }
  .placeholder-icon {
    width: 56px; height: 56px;
    border-radius: 50%;
    background: var(--surface);
    border: 1px solid var(--border);
    display: flex; align-items: center; justify-content: center;
  }
  .placeholder-icon svg { width: 24px; height: 24px; stroke: var(--text3); fill: none; stroke-width: 1.5; }
  .placeholder p { font-size: 14px; color: var(--text3); text-align: center; max-width: 220px; line-height: 1.6; }

  /* Rendered article */
  .article-body { max-width: 680px; }
  .article-body h1 {
    font-size: 24px; font-weight: 700;
    color: var(--navy);
    line-height: 1.35;
    margin-bottom: 1.25rem;
    padding-bottom: 1rem;
    border-bottom: 2px solid var(--orange);
  }
  .article-body h2 {
    font-size: 18px; font-weight: 700;
    color: var(--navy);
    margin: 1.75rem 0 .6rem;
    display: flex; align-items: center; gap: 8px;
  }
  .article-body h2::before {
    content: '';
    display: inline-block;
    width: 4px; height: 18px;
    background: var(--orange);
    border-radius: 2px;
    flex-shrink: 0;
  }
  .article-body p {
    font-size: 15px;
    line-height: 1.9;
    color: var(--text);
    margin-bottom: .9rem;
  }
  .article-body ul {
    margin: .5rem 0 1rem 1.1rem;
  }
  .article-body ul li {
    font-size: 15px;
    line-height: 1.8;
    color: var(--text);
    margin-bottom: .3rem;
    padding-left: .25rem;
  }
  .article-body strong { color: var(--navy); }

  .cta-box {
    background: var(--navy);
    border-radius: var(--radius);
    padding: 1.5rem 1.75rem;
    margin: 2rem 0 .5rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    flex-wrap: wrap;
  }
  .cta-box p { color: rgba(255,255,255,.85); font-size: 15px; line-height: 1.5; }
  .cta-box p strong { color: #fff; }
  .cta-box a {
    display: inline-block;
    padding: 10px 22px;
    background: var(--orange);
    color: #fff;
    text-decoration: none;
    border-radius: var(--radius-sm);
    font-size: 14px;
    font-weight: 700;
    white-space: nowrap;
    transition: background .15s;
  }
  .cta-box a:hover { background: var(--orange-light); }

  /* Tags & meta footer */
  .meta-footer {
    background: var(--surface);
    border-top: 1px solid var(--border);
    padding: .75rem 1.5rem;
    display: flex;
    align-items: center;
    gap: 10px;
    flex-wrap: wrap;
  }
  .meta-label { font-size: 12px; color: var(--text3); }
  .tag {
    font-size: 12px;
    padding: 3px 10px;
    background: var(--surface2);
    color: var(--text2);
    border: 1px solid var(--border);
    border-radius: 100px;
  }
  .wc { font-size: 12px; color: var(--text3); margin-left: auto; }

  /* Loading skeleton */
  .skeleton { max-width: 680px; }
  .skel-line {
    height: 14px; background: var(--border); border-radius: 4px;
    margin-bottom: 12px;
    animation: shimmer 1.3s ease-in-out infinite;
  }
  .skel-h1 { height: 28px; width: 75%; margin-bottom: 20px; }
  .skel-h2 { height: 18px; width: 45%; margin: 24px 0 12px; }
  @keyframes shimmer { 0%,100%{opacity:.35} 50%{opacity:.7} }

  /* Spinner */
  .spinner {
    display: inline-block; width: 14px; height: 14px;
    border: 2px solid rgba(255,255,255,.4);
    border-top-color: #fff;
    border-radius: 50%;
    animation: spin .7s linear infinite;
    vertical-align: middle; margin-right: 6px;
  }
  @keyframes spin { to { transform: rotate(360deg); } }

  /* Scrollbar */
  ::-webkit-scrollbar { width: 6px; }
  ::-webkit-scrollbar-track { background: transparent; }
  ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }
</style>
</head>
<body>

<div class="header">
  <div class="logo">窩客<span>商務中心</span> <span style="font-weight:400;font-size:13px;opacity:.6;margin-left:8px">SEO 文章產生器</span></div>
  <div class="header-sub">workspace.com.tw</div>
</div>

<div class="api-banner">
  <label>Anthropic API Key</label>
  <input type="password" id="api-key" placeholder="sk-ant-api03-..." />
  <span class="api-hint">在 <a href="https://console.anthropic.com/keys" target="_blank">console.anthropic.com</a> 取得免費金鑰</span>
</div>

<div class="layout">
  <div class="sidebar">
    <div>
      <div class="sidebar-heading">文章設定</div>
    </div>

    <div class="field">
      <label>主題</label>
      <select id="topic">
        <option value="virtual_office">虛擬辦公室介紹</option>
        <option value="company_reg">公司登記流程</option>
        <option value="meeting_room">會議室出租</option>
        <option value="startup_cost">創業省錢攻略</option>
        <option value="address_reg">地址登記好處</option>
        <option value="accounting">會計稅務服務</option>
        <option value="coworking">共享辦公空間比較</option>
      </select>
    </div>

    <div class="field">
      <label>目標受眾</label>
      <select id="audience">
        <option value="startup">新創業者</option>
        <option value="freelancer">自由工作者</option>
        <option value="sme">中小企業主</option>
        <option value="ecommerce">電商賣家</option>
      </select>
    </div>

    <div class="field">
      <label>文章風格</label>
      <select id="style">
        <option value="guide">實用教學</option>
        <option value="comparison">比較分析</option>
        <option value="faq">常見問題</option>
        <option value="tips">省錢技巧</option>
      </select>
    </div>

    <hr class="divider">

    <div class="field">
      <label>補充關鍵字（選填）</label>
      <input type="text" id="kw" placeholder="例：台北辦公室租賃, 省成本" />
    </div>

    <button class="gen-btn" id="gen-btn" onclick="generate()">產生文章</button>
  </div>

  <div class="main">
    <div class="toolbar">
      <span class="toolbar-title" id="toolbar-title">文章預覽</span>
      <span class="chip" id="chip" style="display:none">已產生</span>
      <button class="tb-btn" id="copy-btn" onclick="copyMd()" style="display:none">複製 Markdown</button>
      <button class="tb-btn" id="copy-plain-btn" onclick="copyPlain()" style="display:none">複製純文字</button>
    </div>

    <div class="article-scroll" id="article-scroll">
      <div class="placeholder" id="placeholder">
        <div class="placeholder-icon">
          <svg viewBox="0 0 24 24"><path d="M12 20h9"/><path d="M16.5 3.5a2.121 2.121 0 013 3L7 19l-4 1 1-4L16.5 3.5z"/></svg>
        </div>
        <p>在左側選好設定，按「產生文章」即可</p>
      </div>
      <div class="article-body" id="article-body" style="display:none"></div>
    </div>

    <div class="meta-footer" id="meta-footer" style="display:none">
      <span class="meta-label">SEO 關鍵字：</span>
      <div id="tags-wrap" style="display:flex;gap:6px;flex-wrap:wrap"></div>
      <span class="wc" id="wc"></span>
    </div>
  </div>
</div>

<script>
const topicMap = {
  virtual_office: '虛擬辦公室介紹',
  company_reg: '公司登記流程',
  meeting_room: '會議室出租',
  startup_cost: '創業省錢攻略',
  address_reg: '地址登記好處',
  accounting: '會計稅務服務',
  coworking: '共享辦公空間比較'
};
const audienceMap = { startup:'新創業者', freelancer:'自由工作者', sme:'中小企業主', ecommerce:'電商賣家' };
const styleMap = { guide:'實用教學', comparison:'比較分析', faq:'常見問題', tips:'省錢技巧' };

let mdText = '';

function showSkeleton() {
  document.getElementById('placeholder').style.display = 'none';
  document.getElementById('article-body').style.display = 'none';
  document.getElementById('meta-footer').style.display = 'none';
  document.getElementById('article-scroll').innerHTML = `<div class="skeleton">
    <div class="skel-line skel-h1"></div>
    <div class="skel-line" style="width:100%"></div>
    <div class="skel-line" style="width:90%"></div>
    <div class="skel-line" style="width:80%"></div>
    <div class="skel-line skel-h2"></div>
    <div class="skel-line" style="width:100%"></div>
    <div class="skel-line" style="width:95%"></div>
    <div class="skel-line" style="width:75%"></div>
    <div class="skel-line skel-h2"></div>
    <div class="skel-line" style="width:100%"></div>
    <div class="skel-line" style="width:88%"></div>
    <div class="skel-line" style="width:65%"></div>
  </div>`;
}

function mdToHtml(md) {
  let lines = md.split('\n');
  let html = '';
  let inUl = false;
  for (let line of lines) {
    if (line.startsWith('# ')) {
      if (inUl) { html += '</ul>'; inUl = false; }
      html += `<h1>${esc(line.slice(2))}</h1>`;
    } else if (line.startsWith('## ')) {
      if (inUl) { html += '</ul>'; inUl = false; }
      html += `<h2>${esc(line.slice(3))}</h2>`;
    } else if (line.startsWith('### ')) {
      if (inUl) { html += '</ul>'; inUl = false; }
      html += `<h2 style="font-size:16px">${esc(line.slice(4))}</h2>`;
    } else if (line.startsWith('- ') || line.startsWith('* ')) {
      if (!inUl) { html += '<ul>'; inUl = true; }
      html += `<li>${inlineFormat(line.slice(2))}</li>`;
    } else if (line.trim() === '') {
      if (inUl) { html += '</ul>'; inUl = false; }
    } else {
      if (inUl) { html += '</ul>'; inUl = false; }
      html += `<p>${inlineFormat(line)}</p>`;
    }
  }
  if (inUl) html += '</ul>';
  return html;
}

function inlineFormat(s) {
  return esc(s)
    .replace(/\*\*(.+?)\*\*/g, '<strong>$1</strong>')
    .replace(/\*(.+?)\*/g, '<em>$1</em>');
}

function esc(s) {
  return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
}

async function generate() {
  const apiKey = document.getElementById('api-key').value.trim();
  if (!apiKey) { alert('請先輸入 Anthropic API Key'); return; }

  const topic = document.getElementById('topic').value;
  const audience = document.getElementById('audience').value;
  const style = document.getElementById('style').value;
  const kw = document.getElementById('kw').value.trim();

  const btn = document.getElementById('gen-btn');
  btn.innerHTML = '<span class="spinner"></span>產生中...';
  btn.disabled = true;
  document.getElementById('toolbar-title').textContent = '產生中...';
  document.getElementById('chip').style.display = 'none';
  document.getElementById('copy-btn').style.display = 'none';
  document.getElementById('copy-plain-btn').style.display = 'none';
  showSkeleton();

  const prompt = `你是台灣頂尖SEO內容寫手，請為「窩客商務中心」(https://www.workspace.com.tw) 撰寫一篇高品質繁體中文SEO部落格文章。

窩客商務中心服務：虛擬辦公室（公司地址登記、郵件代收）、會議室出租、實體辦公室、公司設立代辦、專業會計服務。地點：台北、新北、板橋，均鄰近捷運站。

主題：${topicMap[topic]}
目標讀者：${audienceMap[audience]}
文章風格：${styleMap[style]}
${kw ? '包含關鍵字：' + kw : ''}

格式（用 Markdown）：
- 第一行：# 吸引人的標題（含主要關鍵字）
- 引言2-3句說明文章價值
- 2-3個 ## 小標題，每段150-200字
- 適時用 - 條列重點
- 自然帶入「窩客商務中心」2-3次
- 結尾段落引導讀者聯絡（不用額外加按鈕）
- 總字數700-900字

最後一行（單獨一行，不算在文章內）：
TAGS: tag1,tag2,tag3,tag4,tag5

只輸出文章Markdown與最後TAGS行，不要其他說明。`;

  try {
    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': apiKey,
        'anthropic-version': '2023-06-01',
        'anthropic-dangerous-allow-any-origin': 'true'
      },
      body: JSON.stringify({
        model: 'claude-opus-4-5',
        max_tokens: 2000,
        messages: [{ role: 'user', content: prompt }]
      })
    });

    if (!res.ok) {
      const err = await res.json().catch(() => ({}));
      throw new Error(err.error?.message || `HTTP ${res.status}`);
    }

    const data = await res.json();
    const raw = data.content.map(i => i.text || '').join('');
    const tagsMatch = raw.match(/TAGS:\s*(.+)/);
    mdText = raw.replace(/TAGS:.+/g, '').trim();

    const titleMatch = mdText.match(/^# (.+)$/m);
    const title = titleMatch ? titleMatch[1] : topicMap[topic];
    document.getElementById('toolbar-title').textContent = title;

    const scroll = document.getElementById('article-scroll');
    scroll.innerHTML = '<div class="article-body" id="article-body"></div>';
    const body = document.getElementById('article-body');
    body.innerHTML = mdToHtml(mdText);

    body.innerHTML += `<div class="cta-box">
      <p><strong>想了解更多？</strong><br>窩客商務中心提供免費諮詢，協助您找到最適合的辦公方案。</p>
      <a href="https://www.workspace.com.tw/contactus" target="_blank">立即免費諮詢</a>
    </div>`;

    if (tagsMatch) {
      const tags = tagsMatch[1].split(',').map(t => t.trim());
      document.getElementById('tags-wrap').innerHTML = tags.map(t => `<span class="tag">${t}</span>`).join('');
    }

    const wc = mdText.replace(/<[^>]*>/g, '').replace(/[#*`\-]/g, '').replace(/\s+/g, '').length;
    document.getElementById('wc').textContent = `約 ${wc} 字`;

    document.getElementById('meta-footer').style.display = 'flex';
    document.getElementById('chip').style.display = 'inline';
    document.getElementById('copy-btn').style.display = 'inline';
    document.getElementById('copy-plain-btn').style.display = 'inline';

  } catch (e) {
    document.getElementById('article-scroll').innerHTML = `<div class="placeholder"><div class="placeholder-icon"><svg viewBox="0 0 24 24" style="stroke:#e8572a"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg></div><p style="color:#e8572a">錯誤：${e.message}</p></div>`;
    document.getElementById('toolbar-title').textContent = '文章預覽';
  }

  btn.innerHTML = '產生文章';
  btn.disabled = false;
}

function copyMd() {
  if (!mdText) return;
  navigator.clipboard.writeText(mdText).then(() => {
    const btn = document.getElementById('copy-btn');
    btn.textContent = '已複製！';
    setTimeout(() => btn.textContent = '複製 Markdown', 2000);
  });
}

function copyPlain() {
  if (!mdText) return;
  const plain = mdText.replace(/^#{1,3} /gm, '').replace(/\*\*(.+?)\*\*/g, '$1').replace(/\*/g, '');
  navigator.clipboard.writeText(plain).then(() => {
    const btn = document.getElementById('copy-plain-btn');
    btn.textContent = '已複製！';
    setTimeout(() => btn.textContent = '複製純文字', 2000);
  });
}
</script>
</body>
</html>
