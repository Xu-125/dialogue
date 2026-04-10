
  <title>嘴替 · Translator</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    *, *::before, *::after { box-sizing: border-box; }
    :root {
      --ease-expo:  cubic-bezier(0.19, 1, 0.22, 1);
      --ease-quart: cubic-bezier(0.76, 0, 0.24, 1);
      --ease-back:  cubic-bezier(0.34, 1.56, 0.64, 1);
    }
    body {
      font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display",
        "Helvetica Neue", Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      background: #fff; color: #000; overflow-x: hidden;
    }

    /* entrance */
    @keyframes fadeUp {
      from { opacity:0; transform:translateY(14px); }
      to   { opacity:1; transform:translateY(0); }
    }
    .fu{animation:fadeUp .65s var(--ease-expo) both;}
    .d1{animation-delay:.05s}.d2{animation-delay:.12s}
    .d3{animation-delay:.19s}.d4{animation-delay:.26s}
    .d5{animation-delay:.33s}

    /* key banner */
    #keyBanner {
      background: #000; color: #fff;
      padding: 10px 20px;
      display: flex; align-items: center; justify-content: center; gap: 16px;
      font-size: 11px; letter-spacing: .08em;
    }
    #keyBanner a {
      color: #fff; text-decoration: underline;
      text-underline-offset: 3px; opacity: .7;
      transition: opacity .2s;
    }
    #keyBanner a:hover { opacity: 1; }
    #keyBanner.hidden { display: none; }

    /* key drawer */
    #keyDrawer {
      position: fixed; inset: 0;
      background: rgba(255,255,255,.97);
      backdrop-filter: blur(8px);
      z-index: 60;
      display: flex; align-items: center; justify-content: center;
      opacity: 0; pointer-events: none;
      transition: opacity .35s ease;
    }
    #keyDrawer.open { opacity: 1; pointer-events: all; }

    .drawer-box {
      width: 100%; max-width: 420px;
      padding: 0 32px;
    }

    .field-label {
      font-size: 9px; letter-spacing: .22em; text-transform: uppercase;
      color: #c0c0c0; display: block; margin-bottom: 8px;
    }
    .field-input {
      width: 100%; border: none; border-bottom: 1px solid #e8e8e8;
      outline: none; background: transparent;
      font-size: 14px; color: #000; padding: 10px 0;
      font-family: inherit; letter-spacing: .01em;
      transition: border-color .25s ease;
    }
    .field-input:focus { border-bottom-color: #000; }
    .field-input::placeholder { color: #d8d8d8; }

    /* role tabs */
    .role-wrap {
      position: relative; display: flex;
      border-bottom: 1px solid #f0f0f0;
      overflow-x: auto; scrollbar-width: none;
    }
    .role-wrap::-webkit-scrollbar { display: none; }
    .role-tab {
      position: relative; z-index: 1;
      padding: 9px 15px; font-size: 12px; letter-spacing: .07em;
      color: #c8c8c8; background: none; border: none;
      cursor: pointer; font-family: inherit; white-space: nowrap;
      transition: color .2s; flex-shrink: 0;
    }
    .role-tab:hover { color: #666; }
    .role-tab.active { color: #000; font-weight: 500; }
    .role-bar {
      position: absolute; bottom: -1px; height: 2px; background: #000;
      transition: left .36s var(--ease-quart), width .36s var(--ease-quart);
      pointer-events: none;
    }

    /* textarea */
    textarea {
      resize: none; border: none; outline: none;
      background: transparent; caret-color: #000;
    }
    textarea::placeholder { color: #e4e4e4; }
    .input-line { height:1px; background:#f0f0f0; transition:background .3s; }
    .input-wrap:focus-within .input-line { background: #000; }

    /* buttons */
    .btn {
      cursor: pointer; font-family: inherit;
      letter-spacing: .1em; text-transform: uppercase;
      transition: transform .35s var(--ease-expo),
        background .28s var(--ease-quart), color .28s var(--ease-quart),
        box-shadow .3s, opacity .2s;
    }
    .btn:hover  { transform: translateY(-2px); }
    .btn:active { transform: scale(.98); opacity: .8; }
    .btn-solid  { background:#000; color:#fff; }
    .btn-solid:hover  { box-shadow: 0 8px 28px rgba(0,0,0,.16); }
    .btn-outline { background:transparent; color:#000; border:1px solid #d0d0d0; }
    .btn-outline:hover { background:#000; color:#fff; border-color:#000;
                         box-shadow: 0 8px 28px rgba(0,0,0,.13); }
    .btn-ghost {
      background:transparent; border:none; font-family:inherit;
      cursor:pointer; font-size:10px; letter-spacing:.12em;
      text-transform:uppercase; color:#c8c8c8; padding:0;
      transition:color .2s, transform .25s var(--ease-back);
      display:inline-flex; align-items:center; gap:4px;
    }
    .btn-ghost:hover { color:#000; transform:translateY(-1px); }

    /* divider */
    .vdiv { width:1px; background:#f0f0f0; align-self:stretch; margin:0 52px; flex-shrink:0; }

    /* status pill */
    .ai-pill {
      display: inline-flex; align-items: center; gap: 8px;
      padding: 6px 14px; border: 1px solid #f0f0f0;
      font-size: 10px; letter-spacing: .1em; color: #c0c0c0;
      text-transform: uppercase; cursor: pointer;
      transition: border-color .25s, color .25s;
    }
    .ai-pill:hover { border-color: #000; color: #000; }
    .ai-dot {
      width: 6px; height: 6px; border-radius: 50%; flex-shrink: 0;
      transition: background .3s;
    }
    .dot-ready   { background:#22c55e; animation:pulse-green 2.4s infinite; }
    .dot-slow    { background:#f59e0b; animation:pulse-amber 2.4s infinite; }
    .dot-think   { background:#f59e0b; animation:pulse-amber .8s infinite; }
    .dot-error   { background:#ef4444; }
    @keyframes pulse-green {
      0%,100%{box-shadow:0 0 0 0 rgba(34,197,94,.4)}
      50%{box-shadow:0 0 0 4px rgba(34,197,94,0)}
    }
    @keyframes pulse-amber {
      0%,100%{box-shadow:0 0 0 0 rgba(245,158,11,.4)}
      50%{box-shadow:0 0 0 4px rgba(245,158,11,0)}
    }

    /* loading bars */
    .bar-wrap { display:flex; align-items:flex-end; gap:3px; height:20px; }
    @keyframes bb {
      0%,100%{transform:scaleY(.2);opacity:.2}
      50%{transform:scaleY(1);opacity:1}
    }
    .bar { width:3px; height:100%; background:#d0d0d0;
           transform-origin:bottom; animation:bb .9s ease-in-out infinite; }
    .bar:nth-child(2){animation-delay:.15s}
    .bar:nth-child(3){animation-delay:.3s}
    .bar:nth-child(4){animation-delay:.45s}
    .bar:nth-child(5){animation-delay:.6s}

    /* result */
    @keyframes resultIn {
      from{opacity:0;transform:translateY(10px)}
      to{opacity:1;transform:translateY(0)}
    }
    .result-in { animation:resultIn .5s var(--ease-expo) both; }
    .quote-wrap { position:relative; padding-left:4px; }
    .quote-wrap::before {
      content:'\201C'; position:absolute; top:-8px; left:-2px;
      font-size:52px; line-height:1; color:#f0f0f0;
      font-family:Georgia,serif; pointer-events:none; user-select:none;
    }

    /* badge */
    .badge {
      font-size:9px; letter-spacing:.2em; text-transform:uppercase;
      padding:3px 10px; border:1px solid;
      display:inline-flex; align-items:center; gap:5px;
    }
    .badge::before { content:''; width:5px; height:5px; border-radius:50%; flex-shrink:0; }
    .badge-eq    { border-color:#000; color:#000; }
    .badge-eq::before { background:#000; }
    .badge-crazy { border-color:#dc2626; color:#dc2626; }
    .badge-crazy::before { background:#dc2626; animation:bb .7s infinite; }
    .badge-groq  { border-color:#f97316; color:#f97316; }
    .badge-groq::before { background:#f97316; }

    /* error */
    .err-box {
      padding:14px 16px; border-left:2px solid #ef4444; background:#fff9f9;
    }
    .err-box p { font-size:12px; color:#ef4444; margin:0; line-height:1.6; }
    .retry-btn {
      font-size:10px; letter-spacing:.1em; text-transform:uppercase;
      background:none; border:1px solid #ef4444; color:#ef4444;
      padding:5px 12px; cursor:pointer; font-family:inherit; margin-top:10px;
      transition:background .2s, color .2s;
    }
    .retry-btn:hover { background:#ef4444; color:#fff; }

    /* toast */
    #toast {
      position:fixed; bottom:28px; left:50%;
      transform:translateX(-50%) translateY(50px);
      background:#000; color:#fff;
      font-size:10px; letter-spacing:.15em; text-transform:uppercase;
      padding:11px 24px;
      transition:transform .45s var(--ease-expo), opacity .4s ease;
      opacity:0; pointer-events:none; white-space:nowrap;
      display:flex; align-items:center; gap:8px;
    }
    #toast::before { content:'✓'; }
    #toast.show { transform:translateX(-50%) translateY(0); opacity:1; }

    /* shake */
    @keyframes shake {
      0%,100%{transform:translateX(0)}
      20%{transform:translateX(-5px)} 40%{transform:translateX(5px)}
      60%{transform:translateX(-3px)} 80%{transform:translateX(3px)}
    }
    .shake { animation:shake .4s ease; }

    /* streaming cursor */
    .cursor::after {
      content:'|'; animation:blink .7s step-end infinite;
      color:#000; margin-left:1px;
    }
    @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }

    #charCount { transition:color .25s; }

    @media(max-width:768px){
      .main-grid { flex-direction:column!important; }
      .vdiv { display:none; }
      .right-col { border-top:1px solid #f0f0f0; padding-top:36px; margin-top:36px; }
    }
  </style>
</head>
<body>

<!-- ══════════ KEY BANNER（无 Groq Key 时显示） ══════════ -->
<div id="keyBanner">
  <span>⚡ 配置 Groq 可提速 10 倍 · 免费</span>
  <button onclick="openKeyDrawer()"
    style="background:rgba(255,255,255,.15);border:1px solid rgba(255,255,255,.3);
           color:#fff;padding:4px 12px;font-size:10px;letter-spacing:.1em;
           text-transform:uppercase;cursor:pointer;font-family:inherit;
           transition:background .2s;"
    onmouseover="this.style.background='rgba(255,255,255,.25)'"
    onmouseout="this.style.background='rgba(255,255,255,.15)'">
    立即配置
  </button>
  <a href="https://console.groq.com/keys" target="_blank" rel="noopener">
    免费领 Key →
  </a>
  <button onclick="document.getElementById('keyBanner').classList.add('hidden')"
    style="background:none;border:none;color:rgba(255,255,255,.4);
           font-size:16px;cursor:pointer;padding:0;line-height:1;
           transition:color .2s;margin-left:4px;"
    onmouseover="this.style.color='rgba(255,255,255,.9)'"
    onmouseout="this.style.color='rgba(255,255,255,.4)'">
    ×
  </button>
</div>

<!-- ══════════ KEY DRAWER ══════════ -->
<div id="keyDrawer" onclick="handleOverlayClick(event)">
  <div class="drawer-box">

    <p style="font-size:9px;letter-spacing:.25em;color:#c8c8c8;text-transform:uppercase;margin:0 0 12px;">
      Groq · 极速 AI
    </p>
    <h2 style="font-size:22px;font-weight:600;letter-spacing:-.025em;margin:0 0 8px;">
      配置免费 API Key
    </h2>
    <p style="font-size:12px;color:#b0b0b0;margin:0 0 32px;line-height:1.7;">
      Groq 使用专用 LPU 芯片，速度比普通 AI 快 10 倍以上。<br>
      免费额度每天约 14,400 次，完全够用。
    </p>

    <!-- step 1 -->
    <div style="display:flex;gap:12px;margin-bottom:20px;align-items:flex-start;">
      <span style="width:20px;height:20px;border:1px solid #000;border-radius:50%;
                   display:flex;align-items:center;justify-content:center;
                   font-size:10px;flex-shrink:0;margin-top:12px;">1</span>
      <div style="flex:1;">
        <label class="field-label">前往 Groq 控制台免费领取</label>
        <a href="https://console.groq.com/keys" target="_blank" rel="noopener"
           style="display:inline-flex;align-items:center;gap:6px;
                  font-size:12px;color:#000;letter-spacing:.04em;
                  border-bottom:1px solid #000;padding-bottom:2px;
                  text-decoration:none;transition:opacity .2s;"
           onmouseover="this.style.opacity='.6'" onmouseout="this.style.opacity='1'">
          console.groq.com/keys <span style="font-size:11px;">↗</span>
        </a>
      </div>
    </div>

    <!-- step 2 -->
    <div style="display:flex;gap:12px;align-items:flex-start;">
      <span style="width:20px;height:20px;border:1px solid #000;border-radius:50%;
                   display:flex;align-items:center;justify-content:center;
                   font-size:10px;flex-shrink:0;margin-top:12px;">2</span>
      <div style="flex:1;">
        <label class="field-label">粘贴你的 API Key</label>
        <input class="field-input" id="groqKeyInput"
               type="password" placeholder="gsk_..."
               autocomplete="off" />
      </div>
    </div>

    <p id="keyError"
       style="font-size:11px;color:#ef4444;margin:12px 0 0 32px;min-height:16px;"></p>

    <div style="display:flex;gap:10px;margin-top:24px;margin-left:32px;">
      <button onclick="saveGroqKey()"
        style="flex:1;padding:14px;background:#000;color:#fff;border:none;
               font-family:inherit;font-size:11px;letter-spacing:.12em;
               text-transform:uppercase;cursor:pointer;transition:opacity .2s;"
        onmouseover="this.style.opacity='.8'" onmouseout="this.style.opacity='1'">
        保存并使用
      </button>
      <button onclick="closeKeyDrawer()"
        style="padding:14px 18px;background:none;color:#c0c0c0;
               border:1px solid #ebebeb;font-family:inherit;font-size:11px;
               letter-spacing:.12em;text-transform:uppercase;cursor:pointer;
               transition:color .2s,border-color .2s;"
        onmouseover="this.style.color='#000';this.style.borderColor='#000'"
        onmouseout="this.style.color='#c0c0c0';this.style.borderColor='#ebebeb'">
        取消
      </button>
    </div>

    <p style="font-size:10px;color:#e0e0e0;margin:20px 0 0 32px;letter-spacing:.03em;">
      Key 仅保存在本地浏览器，不经过任何服务器
    </p>
  </div>
</div>

<!-- ══════════ HEADER ══════════ -->
<header style="padding:52px 0 36px;text-align:center;">
  <p class="fu d1"
     style="font-size:10px;letter-spacing:.28em;color:#c8c8c8;text-transform:uppercase;margin:0 0 10px;">
    Universal Translator
  </p>
  <h1 class="fu d2"
      style="font-size:28px;font-weight:600;letter-spacing:-.035em;margin:0 0 8px;">
    嘴替
  </h1>
  <p class="fu d3"
     style="font-size:12px;color:#c0c0c0;font-weight:300;letter-spacing:.06em;margin:0 0 22px;">
    想说的话，让我来说。
  </p>

  <!-- status pill -->
  <div class="fu d4">
    <div class="ai-pill" onclick="openKeyDrawer()">
      <span class="ai-dot dot-slow" id="aiDot"></span>
      <span id="aiLabel">加载中...</span>
    </div>
  </div>
</header>

<!-- ══════════ MAIN ══════════ -->
<main style="max-width:1000px;margin:0 auto;padding:0 36px 90px;">
  <div class="main-grid fu d5" style="display:flex;align-items:flex-start;">

    <!-- LEFT -->
    <div style="flex:1;display:flex;flex-direction:column;min-width:0;">

      <p style="font-size:9px;letter-spacing:.22em;color:#d0d0d0;text-transform:uppercase;margin:0 0 14px;">
        对象 / Who
      </p>

      <div class="role-wrap" id="roleWrap" style="margin-bottom:30px;">
        <div class="role-bar" id="roleBar"></div>
        <button class="role-tab active" data-role="同事">同事</button>
        <button class="role-tab" data-role="领导">领导</button>
        <button class="role-tab" data-role="家人">家人</button>
        <button class="role-tab" data-role="伴侣">伴侣</button>
        <button class="role-tab" data-role="朋友">朋友</button>
        <button class="role-tab" data-role="老师">老师</button>
        <button class="role-tab" data-role="甲方">甲方</button>
      </div>

      <p style="font-size:9px;letter-spacing:.22em;color:#d0d0d0;text-transform:uppercase;margin:0 0 14px;">
        想法 / Input
      </p>

      <div class="input-wrap">
        <textarea id="inputText" rows="6"
          style="font-size:19px;line-height:1.7;font-weight:300;color:#000;width:100%;"
          placeholder="说说你的真实想法..."
          maxlength="500"></textarea>
        <div style="display:flex;justify-content:flex-end;margin-top:6px;">
          <span id="charCount" style="font-size:10px;color:#e4e4e4;">0 / 500</span>
        </div>
        <div class="input-line" style="margin-top:4px;"></div>
      </div>

      <div style="display:flex;gap:10px;margin-top:22px;">
        <button class="btn btn-solid" id="btnHigh"
          style="flex:1.3;padding:16px 12px;font-size:11px;font-weight:500;">
          一键高情商
        </button>
        <button class="btn btn-outline" id="btnCrazy"
          style="flex:1;padding:16px 12px;font-size:11px;font-weight:500;">
          彻底发疯
        </button>
      </div>

      <p style="font-size:10px;color:#e4e4e4;margin-top:12px;letter-spacing:.03em;">
        Enter 翻译 &nbsp;·&nbsp; Shift+Enter 换行
      </p>
    </div>

    <div class="vdiv"></div>

    <!-- RIGHT -->
    <div class="right-col" style="flex:1;display:flex;flex-direction:column;min-width:0;">

      <p style="font-size:9px;letter-spacing:.22em;color:#d0d0d0;text-transform:uppercase;margin:0 0 14px;">
        翻译 / Output
      </p>

      <div id="stPlaceholder">
        <p style="font-size:19px;font-weight:300;color:#ebebeb;line-height:1.7;margin:0;">
          翻译结果在这里出现...
        </p>
      </div>

      <div id="stLoading" style="display:none;padding-top:6px;">
        <div class="bar-wrap">
          <div class="bar"></div><div class="bar"></div>
          <div class="bar"></div><div class="bar"></div>
          <div class="bar"></div>
        </div>
        <p id="loadingHint"
           style="font-size:11px;color:#d8d8d8;margin:12px 0 0;letter-spacing:.06em;">
          思考中...
        </p>
      </div>

      <div id="stError" style="display:none;flex-direction:column;">
        <div class="err-box">
          <p id="errMsg">网络异常，请稍后重试。</p>
          <button class="retry-btn" onclick="translate(lastMode)">重试</button>
        </div>
      </div>

      <div id="stResult" style="display:none;flex-direction:column;">
        <div id="resultBadge" style="margin-bottom:18px;"></div>
        <div class="quote-wrap" style="min-height:80px;">
          <p id="resultText"
             style="font-size:19px;font-weight:300;line-height:1.8;
                    margin:0;color:#000;word-break:break-all;">
          </p>
        </div>
        <div style="margin-top:28px;padding-top:18px;border-top:1px solid #f5f5f5;
                    display:flex;gap:18px;align-items:center;">
          <button class="btn-ghost" id="copyBtn" onclick="copyResult()">
            复制 <span style="font-size:12px;">↗</span>
          </button>
          <button class="btn-ghost" onclick="resetAll()">重新来过</button>
          <button class="btn-ghost" onclick="translate(lastMode)" style="margin-left:auto;">
            换一条 ↺
          </button>
        </div>
      </div>
    </div>
  </div>

  <div style="margin-top:80px;display:flex;align-items:center;gap:20px;">
    <div style="flex:1;height:1px;background:#f8f8f8;"></div>
    <span style="font-size:9px;letter-spacing:.18em;color:#e8e8e8;">@zeyi</span>
    <div style="flex:1;height:1px;background:#f8f8f8;"></div>
  </div>
</main>

<div id="toast">已复制到剪贴板</div>

<script>
/* ═══════════════════════════════════
   ROLE CONFIG
═══════════════════════════════════ */
const ROLE_DESC = {
  同事: '平级同事，日常工作协作，互相平等',
  领导: '直属上级，有上下级关系，需保持尊重',
  家人: '父母或家庭成员，亲近但有代际压力',
  伴侣: '恋人或配偶，最亲密但也最容易摩擦',
  朋友: '普通朋友，平等社交，可以直接但不伤感情',
  老师: '老师或教授，有权威性，需尊重也要维护自己',
  甲方: '甲方客户，服务与被服务关系，需专业周到',
};

const PLACEHOLDERS = {
  同事: '比如：这需求改了十几版了我不想改了...',
  领导: '比如：凭什么加班的总是我...',
  家人: '比如：别再催我结婚了烦死了...',
  伴侣: '比如：你根本不理解我的感受...',
  朋友: '比如：感觉你只有有事才找我...',
  老师: '比如：这门课完全听不懂但不敢说...',
  甲方: '比如：改了这么多版还要改我快崩了...',
};

/* ═══════════════════════════════════
   CONFIG & KEY MANAGEMENT
═══════════════════════════════════ */
let groqKey = localStorage.getItem('zr_groq_key') || '';

function openKeyDrawer() {
  document.getElementById('groqKeyInput').value = groqKey ? '••••••••••••••••' : '';
  document.getElementById('keyError').textContent = '';
  document.getElementById('keyDrawer').classList.add('open');
  setTimeout(() => document.getElementById('groqKeyInput').focus(), 300);
}

function closeKeyDrawer() {
  document.getElementById('keyDrawer').classList.remove('open');
}

function handleOverlayClick(e) {
  if (e.target === document.getElementById('keyDrawer')) closeKeyDrawer();
}

function saveGroqKey() {
  const val = document.getElementById('groqKeyInput').value.trim();
  const err = document.getElementById('keyError');

  if (!val || val === '••••••••••••••••') {
    err.textContent = '请填写 API Key'; return;
  }
  if (!val.startsWith('gsk_')) {
    err.textContent = 'Groq 的 Key 通常以 gsk_ 开头，请检查'; return;
  }

  groqKey = val;
  localStorage.setItem('zr_groq_key', groqKey);
  closeKeyDrawer();
  refreshStatus();
  // 隐藏顶部 banner
  document.getElementById('keyBanner').classList.add('hidden');
}

function clearGroqKey() {
  groqKey = '';
  localStorage.removeItem('zr_groq_key');
  refreshStatus();
}

function refreshStatus() {
  const dot   = document.getElementById('aiDot');
  const label = document.getElementById('aiLabel');
  const banner = document.getElementById('keyBanner');

  if (groqKey) {
    dot.className     = 'ai-dot dot-ready';
    label.textContent = 'Groq 极速模式 · 点击管理';
    banner.classList.add('hidden');
  } else {
    dot.className     = 'ai-dot dot-slow';
    label.textContent = 'Pollinations 普通模式 · 点击提速';
    banner.classList.remove('hidden');
  }
}

/* ═══════════════════════════════════
   PROMPTS
═══════════════════════════════════ */
const SYS = `你是一个精通中国人际关系和沟通的助手，专门帮人把话说得恰到好处。
规则：只输出翻译结果，不加任何解释、前缀、引号。语言自然口语化，像真实的人在说话。`;

function buildPrompt(input, role, mode) {
  const desc = ROLE_DESC[role] || role;
  if (mode === 'high') {
    return `把下面这句话翻译成高情商版本。

说话对象：${role}（${desc}）
原话：「${input}」

要求：
- 保留核心意思，换一种成熟得体的表达
- 语气符合与"${role}"的关系
- 自然口语化，不说废话官腔，不用"您"
- 1到2句话，不超过55字
- 直接输出结果`;
  } else {
    return `把下面这句话翻译成夸张发疯版本。

说话对象：${role}（${desc}）
原话：「${input}」

要求：
- 极度夸张，充满崩溃感和戏剧性
- 多用！！！，情绪爆炸
- 结合"${role}"这个对象，发疯有针对性，接地气让人想笑
- 2到3句话，不超过75字
- 直接输出结果`;
  }
}

/* ═══════════════════════════════════
   GROQ CALL（极速）
   模型：llama-3.3-70b-versatile
   通常 < 1 秒响应
═══════════════════════════════════ */
async function callGroq(input, role, mode) {
  const res = await fetch('https://api.groq.com/openai/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type':  'application/json',
      'Authorization': `Bearer ${groqKey}`,
    },
    body: JSON.stringify({
      model:       'llama-3.3-70b-versatile',
      messages: [
        { role: 'system', content: SYS },
        { role: 'user',   content: buildPrompt(input, role, mode) },
      ],
      temperature: mode === 'crazy' ? 1.1 : 0.85,
      max_tokens:  200,
    }),
  });

  if (!res.ok) {
    const body = await res.json().catch(() => ({}));
    throw new Error(body?.error?.message || `HTTP ${res.status}`);
  }

  const data = await res.json();
  const text = data?.choices?.[0]?.message?.content?.trim();
  if (!text) throw new Error('返回内容为空');
  return clean(text);
}

/* ═══════════════════════════════════
   POLLINATIONS CALL（免费兜底）
═══════════════════════════════════ */
async function callPollinations(input, role, mode) {
  const res = await fetch('https://text.pollinations.ai/', {
    method:  'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      messages: [
        { role: 'system', content: SYS },
        { role: 'user',   content: buildPrompt(input, role, mode) },
      ],
      model:       'openai',
      temperature: mode === 'crazy' ? 1.1 : 0.88,
      seed:        Math.floor(Math.random() * 9999),
      private:     true,
    }),
  });

  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  const text = await res.text();
  if (!text?.trim()) throw new Error('返回内容为空');
  return clean(text.trim());
}

function clean(t) {
  return t.replace(/^["「『【]|["」』】]$/g, '').trim();
}

/* ═══════════════════════════════════
   ROLE TABS
═══════════════════════════════════ */
let currentRole = '同事';

function moveBar(tab) {
  const bar  = document.getElementById('roleBar');
  const wrap = document.getElementById('roleWrap');
  const wr   = wrap.getBoundingClientRect();
  const tr   = tab.getBoundingClientRect();
  bar.style.left  = (tr.left - wr.left + wrap.scrollLeft) + 'px';
  bar.style.width = tr.width + 'px';
}

document.querySelectorAll('.role-tab').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('.role-tab').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    currentRole = btn.dataset.role;
    moveBar(btn);
    resetOutput();
    document.getElementById('inputText').placeholder =
      PLACEHOLDERS[currentRole] || '说说你的真实想法...';
  });
});

/* ═══════════════════════════════════
   STATE
═══════════════════════════════════ */
let isLoading  = false;
let lastResult = '';
let lastMode   = 'high';

function showState(s) {
  document.getElementById('stPlaceholder').style.display = s==='placeholder' ? 'block' : 'none';
  document.getElementById('stLoading').style.display     = s==='loading'     ? 'block' : 'none';
  document.getElementById('stError').style.display       = s==='error'       ? 'flex'  : 'none';
  document.getElementById('stResult').style.display      = s==='result'      ? 'flex'  : 'none';
}

/* ═══════════════════════════════════
   TRANSLATE
═══════════════════════════════════ */
async function translate(mode) {
  if (isLoading) return;

  const input = document.getElementById('inputText').value.trim();
  if (!input) {
    const ta = document.getElementById('inputText');
    ta.classList.add('shake');
    ta.focus();
    setTimeout(() => ta.classList.remove('shake'), 420);
    return;
  }

  lastMode  = mode;
  isLoading = true;
  showState('loading');

  // Update dot to thinking
  const dot   = document.getElementById('aiDot');
  const label = document.getElementById('aiLabel');
  dot.className = 'ai-dot dot-think';

  const usingGroq = !!groqKey;
  document.getElementById('loadingHint').textContent =
    usingGroq ? '⚡ Groq 极速翻译中...' : 'AI 翻译中...';

  let text    = '';
  let success = false;
  let errMsg  = '';

  if (usingGroq) {
    try {
      text    = await callGroq(input, currentRole, mode);
      success = true;
    } catch (e) {
      console.warn('Groq failed:', e.message);
      errMsg = parseError(e.message, 'groq');
    }
  }

  // Fallback to Pollinations
  if (!success) {
    try {
      label.textContent = '切换备用通道...';
      text    = await callPollinations(input, currentRole, mode);
      success = true;
      errMsg  = '';
    } catch (e) {
      console.error('Pollinations failed:', e.message);
      if (!errMsg) errMsg = parseError(e.message, 'net');
    }
  }

  if (!success) {
    dot.className = 'ai-dot dot-error';
    setTimeout(refreshStatus, 3500);
    document.getElementById('errMsg').textContent = errMsg;
    showState('error');
    isLoading = false;
    return;
  }

  lastResult = text;

  // Badge
  const badge = document.getElementById('resultBadge');
  const modeLabel = mode === 'high' ? '高情商' : '发疯版';
  if (usingGroq && success) {
    badge.innerHTML = `<span class="badge badge-groq">⚡ ${modeLabel} · Groq · ${currentRole}</span>`;
  } else if (mode === 'high') {
    badge.innerHTML = `<span class="badge badge-eq">${modeLabel} · ${currentRole}</span>`;
  } else {
    badge.innerHTML = `<span class="badge badge-crazy">${modeLabel} · ${currentRole}</span>`;
  }

  const resultEl = document.getElementById('resultText');
  resultEl.textContent = '';
  resultEl.classList.remove('cursor');
  showState('result');
  resultEl.parentElement.classList.add('result-in');

  // Groq 很快，打字机速度快一点
  await typeWriter(resultEl, text, usingGroq ? 3 : 5);

  refreshStatus();
  isLoading = false;
}

function parseError(msg, ctx) {
  if (msg.includes('401') || msg.includes('invalid_api_key') || msg.includes('Unauthorized'))
    return 'Groq Key 无效，请检查后重新配置。';
  if (msg.includes('429') || msg.includes('rate_limit'))
    return '请求太频繁，稍等几秒再试。';
  if (msg.includes('402') || msg.includes('quota'))
    return '免费额度已用完，明天自动恢复。';
  if (msg.includes('Failed to fetch') || msg.includes('NetworkError') || msg.includes('fetch'))
    return '网络连接失败，请检查网络后重试。';
  if (msg.includes('500') || msg.includes('503'))
    return 'AI 服务暂时繁忙，点击重试。';
  return '出了点问题，点击重试。';
}

/* ═══════════════════════════════════
   TYPEWRITER
═══════════════════════════════════ */
async function typeWriter(el, text, speed) {
  el.classList.add('cursor');
  for (let i = 0; i <= text.length; i++) {
    el.textContent = text.slice(0, i);
    await sleep(speed + Math.random() * 4);
  }
  el.classList.remove('cursor');
}

function sleep(ms) { return new Promise(r => setTimeout(r, ms)); }

/* ═══════════════════════════════════
   HELPERS
═══════════════════════════════════ */
function resetOutput() {
  lastResult = ''; isLoading = false;
  showState('placeholder');
}

function resetAll() {
  document.getElementById('inputText').value = '';
  document.getElementById('charCount').textContent = '0 / 500';
  document.getElementById('charCount').style.color = '#e4e4e4';
  resetOutput();
  document.getElementById('inputText').focus();
}

async function copyResult() {
  if (!lastResult) return;
  const btn = document.getElementById('copyBtn');
  await navigator.clipboard.writeText(lastResult);
  btn.textContent = '已复制 ✓';
  btn.style.color = '#000';
  document.getElementById('toast').classList.add('show');
  setTimeout(() => {
    btn.innerHTML   = '复制 <span style="font-size:12px">↗</span>';
    btn.style.color = '';
    document.getElementById('toast').classList.remove('show');
  }, 2000);
}

/* ═══════════════════════════════════
   EVENTS
═══════════════════════════════════ */
document.getElementById('inputText').addEventListener('keydown', e => {
  if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); translate('high'); }
});

document.getElementById('inputText').addEventListener('input', function () {
  const len = this.value.length;
  const c   = document.getElementById('charCount');
  c.textContent = `${len} / 500`;
  c.style.color = len > 400 ? '#dc2626' : len > 300 ? '#f59e0b' : '#e4e4e4';
});

document.getElementById('btnHigh').addEventListener('click',  () => translate('high'));
document.getElementById('btnCrazy').addEventListener('click', () => translate('crazy'));

document.addEventListener('keydown', e => {
  if (e.key === 'Escape') closeKeyDrawer();
});

/* ═══════════════════════════════════
   INIT
═══════════════════════════════════ */
window.addEventListener('load', () => {
  const first = document.querySelector('.role-tab.active');
  if (first) moveBar(first);
  refreshStatus();
});
</script>
</body>
