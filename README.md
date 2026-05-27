<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black">
<meta name="apple-mobile-web-app-title" content="TME Tools">
<title>TME Tools</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=DM+Sans:wght@300;400;500;600&display=swap');
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  :root {
    --bg: #0f1117; --surface: #181c27; --surface2: #1e2333;
    --border: rgba(255,255,255,0.07); --accent: #f5a623; --accent2: #3b82f6;
    --success: #22c55e; --danger: #ef4444; --text: #e8eaf0; --muted: #6b7280;
    --font: 'DM Sans', sans-serif; --mono: 'DM Mono', monospace;
  }
  body { font-family: var(--font); background: var(--bg); color: var(--text); min-height: 100vh; font-size: 14px; }
  #login-screen { min-height: 100vh; display: flex; align-items: center; justify-content: center; background: var(--bg); padding: 20px; }
  .login-card { background: var(--surface); border: 1px solid var(--border); border-radius: 16px; padding: 40px 36px; width: 100%; max-width: 380px; }
  .login-logo { font-family: var(--mono); font-size: 11px; letter-spacing: 0.15em; color: var(--accent); text-transform: uppercase; margin-bottom: 6px; }
  .login-title { font-size: 22px; font-weight: 600; margin-bottom: 28px; line-height: 1.2; }
  .field-label { font-size: 11px; font-weight: 500; letter-spacing: 0.08em; text-transform: uppercase; color: var(--muted); margin-bottom: 6px; display: block; }
  .field-group { margin-bottom: 16px; }
  select, input[type="text"], input[type="password"], textarea {
    width: 100%; background: var(--bg); border: 1px solid var(--border); border-radius: 8px;
    color: var(--text); font-family: var(--font); font-size: 14px; padding: 10px 12px;
    outline: none; transition: border-color 0.15s; appearance: none;
  }
  select:focus, input:focus, textarea:focus { border-color: var(--accent); }
  select option { background: #1e2333; }
  .btn { display: inline-flex; align-items: center; justify-content: center; gap: 6px; padding: 10px 18px; border-radius: 8px; border: none; font-family: var(--font); font-size: 14px; font-weight: 500; cursor: pointer; transition: opacity 0.15s, transform 0.1s; }
  .btn:active { transform: scale(0.98); }
  .btn:hover { opacity: 0.88; }
  .btn-primary { background: var(--accent); color: #0f1117; width: 100%; }
  .btn-blue { background: var(--accent2); color: #fff; }
  .btn-success { background: var(--success); color: #fff; }
  .btn-ghost { background: var(--surface2); color: var(--text); border: 1px solid var(--border); }
  .btn-sm { padding: 6px 12px; font-size: 12px; }
  .btn:disabled { opacity: 0.4; cursor: not-allowed; }
  .error-msg { color: var(--danger); font-size: 12px; margin-top: 8px; display: none; }
  #app { display: none; flex-direction: column; min-height: 100vh; }
  .topbar { background: var(--surface); border-bottom: 1px solid var(--border); padding: 0 16px; height: 52px; display: flex; align-items: center; justify-content: space-between; position: sticky; top: 0; z-index: 100; }
  .topbar-logo { font-family: var(--mono); font-size: 11px; letter-spacing: 0.15em; color: var(--accent); text-transform: uppercase; }
  .topbar-user { display: flex; align-items: center; gap: 8px; }
  .user-badge { background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; padding: 3px 8px; font-size: 12px; font-weight: 500; color: var(--accent); font-family: var(--mono); }
  .nav { background: var(--surface); border-bottom: 1px solid var(--border); display: flex; overflow-x: auto; scrollbar-width: none; }
  .nav::-webkit-scrollbar { display: none; }
  .nav-item { padding: 12px 16px; font-size: 13px; font-weight: 500; color: var(--muted); cursor: pointer; border-bottom: 2px solid transparent; white-space: nowrap; transition: color 0.15s, border-color 0.15s; }
  .nav-item.active { color: var(--accent); border-bottom-color: var(--accent); }
  .nav-item:hover:not(.active) { color: var(--text); }
  .main { padding: 16px; max-width: 700px; margin: 0 auto; width: 100%; }
  .card { background: var(--surface); border: 1px solid var(--border); border-radius: 12px; padding: 20px; margin-bottom: 12px; }
  .card-title { font-size: 11px; font-weight: 500; letter-spacing: 0.1em; text-transform: uppercase; color: var(--muted); margin-bottom: 16px; }
  .stats-row { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 12px; }
  .stat-card { background: var(--surface); border: 1px solid var(--border); border-radius: 12px; padding: 16px; text-align: center; }
  .stat-val { font-family: var(--mono); font-size: 28px; font-weight: 500; line-height: 1; margin-bottom: 4px; }
  .stat-label { font-size: 11px; color: var(--muted); text-transform: uppercase; letter-spacing: 0.08em; }
  .tool-row { display: flex; align-items: center; justify-content: space-between; padding: 12px 0; border-bottom: 1px solid var(--border); gap: 10px; }
  .tool-row:last-child { border-bottom: none; }
  .tool-info { flex: 1; min-width: 0; }
  .tool-name { font-weight: 500; font-size: 14px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .tool-meta { font-size: 11px; color: var(--muted); margin-top: 2px; font-family: var(--mono); }
  .badge { display: inline-block; padding: 2px 8px; border-radius: 4px; font-size: 10px; font-weight: 500; font-family: var(--mono); letter-spacing: 0.05em; text-transform: uppercase; white-space: nowrap; }
  .badge-workshop { background: rgba(34,197,94,0.15); color: var(--success); }
  .badge-out { background: rgba(245,166,35,0.15); color: var(--accent); }
  .log-row { display: flex; gap: 12px; padding: 10px 0; border-bottom: 1px solid var(--border); align-items: flex-start; }
  .log-row:last-child { border-bottom: none; }
  .log-dot { width: 8px; height: 8px; border-radius: 50%; margin-top: 4px; flex-shrink: 0; }
  .log-dot-out { background: var(--accent); }
  .log-dot-return { background: var(--success); }
  .log-dot-transfer { background: var(--accent2); }
  .log-text { font-size: 13px; line-height: 1.5; flex: 1; }
  .log-time { font-size: 11px; color: var(--muted); font-family: var(--mono); white-space: nowrap; }
  .modal-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.7); z-index: 200; align-items: flex-end; justify-content: center; }
  .modal-overlay.open { display: flex; }
  .modal { background: var(--surface); border: 1px solid var(--border); border-radius: 16px 16px 0 0; padding: 24px 20px 32px; width: 100%; max-width: 500px; max-height: 90vh; overflow-y: auto; }
  .modal-title { font-size: 16px; font-weight: 600; margin-bottom: 20px; display: flex; align-items: center; justify-content: space-between; }
  .modal-close { background: none; border: none; color: var(--muted); font-size: 20px; cursor: pointer; padding: 0; line-height: 1; }
  .modal-actions { display: flex; gap: 8px; margin-top: 20px; }
  .modal-actions .btn { flex: 1; }
  .tab-row { display: flex; gap: 4px; margin-bottom: 16px; background: var(--bg); border-radius: 8px; padding: 3px; }
  .tab { flex: 1; text-align: center; padding: 7px; border-radius: 6px; font-size: 12px; font-weight: 500; cursor: pointer; color: var(--muted); transition: background 0.15s, color 0.15s; }
  .tab.active { background: var(--surface2); color: var(--text); }
  .search-wrap { position: relative; margin-bottom: 12px; }
  .search-wrap input { padding-left: 32px; }
  .search-icon { position: absolute; left: 10px; top: 50%; transform: translateY(-50%); color: var(--muted); font-size: 14px; pointer-events: none; }
  .empty-state { text-align: center; padding: 32px 16px; color: var(--muted); font-size: 13px; }
  textarea { resize: vertical; min-height: 60px; }
  .divider { height: 1px; background: var(--border); margin: 16px 0; }
  .section-label { font-size: 11px; font-weight: 500; letter-spacing: 0.1em; text-transform: uppercase; color: var(--muted); margin-bottom: 10px; }
  .site-pill { display: inline-block; background: var(--surface2); border: 1px solid var(--border); border-radius: 20px; padding: 4px 10px; font-size: 12px; font-family: var(--mono); color: var(--muted); margin: 3px; }
  .highlight { color: var(--accent); }
  .loading-bar { text-align: center; padding: 40px 16px; color: var(--muted); font-size: 13px; }
  .spinner { display: inline-block; width: 16px; height: 16px; border: 2px solid var(--border); border-top-color: var(--accent); border-radius: 50%; animation: spin 0.7s linear infinite; margin-right: 8px; vertical-align: middle; }
  @keyframes spin { to { transform: rotate(360deg); } }
  .toast { position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%); background: var(--surface2); border: 1px solid var(--border); border-radius: 8px; padding: 10px 18px; font-size: 13px; z-index: 300; opacity: 0; transition: opacity 0.2s; pointer-events: none; white-space: nowrap; }
  .toast.show { opacity: 1; }
  .date-filter { display: flex; gap: 8px; margin-bottom: 12px; align-items: center; flex-wrap: wrap; }
  .date-filter input[type="date"] { flex: 1; min-width: 130px; padding: 8px 10px; font-size: 13px; }
  .date-filter .btn { width: auto; padding: 8px 12px; font-size: 12px; }
</style>
</head>
<body>

<div id="login-screen">
  <div class="login-card">
    <div class="login-logo">TME Tools</div>
    <div class="login-title">Tool Tracker</div>
    <div class="field-group">
      <label class="field-label">Who are you?</label>
      <select id="login-user">
        <option value="">Select your name...</option>
        <option value="admin">Teejy (Admin)</option>
        <option value="pidge">Pidge</option>
        <option value="sam">Sam</option>
        <option value="smitty">Smitty</option>
        <option value="clarky">Clarky</option>
        <option value="vinnie">Vinnie</option>
        <option value="kirky">Kirky</option>
        <option value="hoppy">Hoppy</option>
        <option value="gibby">Gibby</option>
        <option value="user9">Supervisor 9</option>
        <option value="user10">Supervisor 10</option>
      </select>
    </div>
    <div class="field-group">
      <label class="field-label">PIN</label>
      <input type="password" id="login-pin" placeholder="Enter your PIN" maxlength="6">
    </div>
    <div class="error-msg" id="login-error">Incorrect PIN.</div>
    <button class="btn btn-primary" style="margin-top:8px" onclick="doLogin()">Sign in →</button>
  </div>
</div>

<div id="app">
  <div class="topbar">
    <div class="topbar-logo">⚡ TME Tools</div>
    <div class="topbar-user">
      <span class="user-badge" id="topbar-username"></span>
      <button class="btn btn-ghost btn-sm" onclick="doLogout()">Out</button>
    </div>
  </div>
  <div class="nav">
    <div class="nav-item active" onclick="showTab('dashboard')">Dashboard</div>
    <div class="nav-item" onclick="showTab('tools')">Tools</div>
    <div class="nav-item" onclick="showTab('log')">Activity Log</div>
    <div class="nav-item" id="nav-admin" style="display:none" onclick="showTab('admin')">Admin</div>
  </div>
  <div class="main">

    <div id="tab-dashboard">
      <div class="stats-row">
        <div class="stat-card"><div class="stat-val" id="stat-total">—</div><div class="stat-label">Total</div></div>
        <div class="stat-card"><div class="stat-val" style="color:var(--success)" id="stat-in">—</div><div class="stat-label">In workshop</div></div>
        <div class="stat-card"><div class="stat-val" style="color:var(--accent)" id="stat-out">—</div><div class="stat-label">On site</div></div>
      </div>
      <div class="card">
        <div class="card-title">Tools currently out</div>
        <div id="dashboard-out-list"><div class="loading-bar"><span class="spinner"></span>Loading from sheet...</div></div>
      </div>
      <div class="card">
        <div class="card-title">Quick actions</div>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px">
          <button class="btn btn-blue" onclick="openModal('checkout')">Check out tool</button>
          <button class="btn btn-success" onclick="openModal('return')">Return tool</button>
          <button class="btn btn-ghost" onclick="openModal('transfer')" style="grid-column:span 2">Transfer site → site</button>
        </div>
      </div>
    </div>

    <div id="tab-tools" style="display:none">
      <div class="search-wrap">
        <span class="search-icon">🔍</span>
        <input type="text" id="tool-search" placeholder="Search by name or asset number..." oninput="renderTools()">
      </div>
      <div class="card">
        <div class="card-title">All tools</div>
        <div id="tools-list"><div class="loading-bar"><span class="spinner"></span>Loading...</div></div>
      </div>
    </div>

    <div id="tab-log" style="display:none">
      <div class="date-filter">
        <input type="date" id="log-date-from" title="From date">
        <input type="date" id="log-date-to" title="To date">
        <button class="btn btn-ghost btn-sm" onclick="renderLog()">Filter</button>
        <button class="btn btn-ghost btn-sm" onclick="clearLogFilter()">Clear</button>
      </div>
      <div class="card">
        <div class="card-title">Activity log</div>
        <div id="activity-log-list"><div class="loading-bar"><span class="spinner"></span>Loading...</div></div>
      </div>
    </div>

    <div id="tab-admin" style="display:none">
      <div class="tab-row">
        <div class="tab active" onclick="showAdminTab('users')" id="atab-users">Users</div>
        <div class="tab" onclick="showAdminTab('sites')" id="atab-sites">Sites</div>
        <div class="tab" onclick="showAdminTab('addtool')" id="atab-addtool">Add tool</div>
      </div>
      <div id="admin-users">
        <div class="card">
          <div class="card-title">All users</div>
          <div id="users-list"></div>
        </div>
      </div>
      <div id="admin-sites" style="display:none">
        <div class="card">
          <div class="card-title">Job sites</div>
          <div id="sites-list"></div>
          <div class="divider"></div>
          <div class="section-label">Add new site</div>
          <div style="display:flex;gap:8px">
            <input type="text" id="new-site-name" placeholder="Site name e.g. Morwell">
            <button class="btn btn-primary" style="width:auto" onclick="addSite()">Add</button>
          </div>
        </div>
      </div>
      <div id="admin-addtool" style="display:none">
        <div class="card">
          <div class="card-title">Add new tool</div>
          <div class="field-group"><label class="field-label">Asset number</label><input type="text" id="new-tool-id" placeholder="e.g. A-042"></div>
          <div class="field-group"><label class="field-label">Tool name</label><input type="text" id="new-tool-name" placeholder="e.g. Makita Drop Saw"></div>
          <div class="field-group">
            <label class="field-label">Category</label>
            <select id="new-tool-cat">
              <option value="Batteries">Batteries</option>
              <option value="Battery tools">Battery tools</option>
              <option value="Hand tools">Hand tools</option>
              <option value="Office equipment">Office equipment</option>
              <option value="Rollers">Rollers</option>
              <option value="Safety equipment">Safety equipment</option>
              <option value="Test equipment">Test equipment</option>
              <option value="Tool accessories">Tool accessories</option>
            </select>
          </div>
          <div class="field-group"><label class="field-label">Serial number (optional)</label><input type="text" id="new-tool-serial" placeholder="e.g. SN-123456"></div>
          <div class="field-group"><label class="field-label">Notes (optional)</label><textarea id="new-tool-notes" placeholder="Any additional info..."></textarea></div>
          <button class="btn btn-primary" onclick="addTool()">Add to inventory</button>
          <div class="error-msg" id="add-tool-error">Please fill in asset number and tool name.</div>
        </div>
      </div>
    </div>

  </div>
</div>

<div class="modal-overlay" id="modal-checkout">
  <div class="modal">
    <div class="modal-title">Check out tool <button class="modal-close" onclick="closeModal('checkout')">×</button></div>
    <div class="field-group"><label class="field-label">Tool</label><select id="co-tool"></select></div>
    <div class="field-group"><label class="field-label">Going to site</label><select id="co-site"></select></div>
    <div class="field-group"><label class="field-label">Notes (optional)</label><input type="text" id="co-notes" placeholder="Any relevant details"></div>
    <div class="modal-actions">
      <button class="btn btn-ghost" onclick="closeModal('checkout')">Cancel</button>
      <button class="btn btn-blue" id="btn-co" onclick="doCheckout()">Confirm checkout</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="modal-return">
  <div class="modal">
    <div class="modal-title">Return tool <button class="modal-close" onclick="closeModal('return')">×</button></div>
    <div class="field-group"><label class="field-label">Tool returning</label><select id="ret-tool"></select></div>
    <div class="field-group"><label class="field-label">Notes (optional)</label><input type="text" id="ret-notes" placeholder="Condition, any issues etc."></div>
    <div class="modal-actions">
      <button class="btn btn-ghost" onclick="closeModal('return')">Cancel</button>
      <button class="btn btn-success" id="btn-ret" onclick="doReturn()">Confirm return</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="modal-transfer">
  <div class="modal">
    <div class="modal-title">Transfer site → site <button class="modal-close" onclick="closeModal('transfer')">×</button></div>
    <div class="field-group"><label class="field-label">Tool</label><select id="tr-tool"></select></div>
    <div class="field-group"><label class="field-label">Moving to site</label><select id="tr-site"></select></div>
    <div class="field-group"><label class="field-label">Notes (optional)</label><input type="text" id="tr-notes" placeholder="Any relevant details"></div>
    <div class="modal-actions">
      <button class="btn btn-ghost" onclick="closeModal('transfer')">Cancel</button>
      <button class="btn btn-blue" id="btn-tr" onclick="doTransfer()">Confirm transfer</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
const API = 'https://script.google.com/macros/s/AKfycbz4Rf1pbUMHbhRJbBdhBeOxtXH8WWyn9CcUpTqj4uRQEX_m9HoIFxiM2TyJXT5_Dk-loQ/exec';
const PINS = { admin:'0000', pidge:'1212', sam:'2323', smitty:'3434', clarky:'4545', vinnie:'5656', kirky:'6767', hoppy:'7878', gibby:'8989', user9:'1357', user10:'2468' };
const USER_LABELS = { admin:'Teejy', pidge:'Pidge', sam:'Sam', smitty:'Smitty', clarky:'Clarky', vinnie:'Vinnie', kirky:'Kirky', hoppy:'Hoppy', gibby:'Gibby', user9:'Supervisor 9', user10:'Supervisor 10' };

let currentUser = null;
let tools = [], activityLog = [], sites = [];

function fmtTime(d) {
  if (!d) return '';
  const dt = d instanceof Date ? d : new Date(d);
  if (isNaN(dt)) return String(d);
  return dt.toLocaleString('en-AU',{day:'2-digit',month:'short',year:'numeric',hour:'2-digit',minute:'2-digit',hour12:true});
}
function now() { return fmtTime(new Date()); }
function getTool(id) { return tools.find(t => t.id === id); }

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg; t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2500);
}

async function apiCall(params) {
  const url = API + '?' + new URLSearchParams(params);
  const r = await fetch(url);
  return r.json();
}

async function loadAll() {
  try {
    const data = await apiCall({ action: 'getAll' });
    tools = (data.tools||[]).slice(1).filter(r=>r[0]).map(r=>({ id:String(r[0]), name:String(r[1]), cat:String(r[2]), location:String(r[3]), serial:String(r[4]||''), notes:String(r[5]||'') }));
    activityLog = (data.log||[]).slice(1).filter(r=>r[0]).map(r=>({ time:String(r[0]), type:String(r[1]), tool:String(r[2]), toolName:String(r[3]), from:String(r[4]||''), to:String(r[5]||''), user:String(r[6]||'') }));
    const raw = (data.sites||[]).filter(r=>r[0]).map(r=>String(r[0]));
    sites = [...new Set(raw)];
    if (!sites.length) sites = ['Workshop'];
  } catch(e) { showToast('Could not connect to sheet'); }
}

function doLogin() {
  const user = document.getElementById('login-user').value;
  const pin = document.getElementById('login-pin').value;
  const err = document.getElementById('login-error');
  if (!user) { err.style.display='block'; err.textContent='Please select your name.'; return; }
  if (PINS[user] !== pin) { err.style.display='block'; err.textContent='Incorrect PIN. Try again.'; return; }
  err.style.display='none';
  currentUser = user;
  document.getElementById('login-screen').style.display='none';
  document.getElementById('app').style.display='flex';
  document.getElementById('topbar-username').textContent = USER_LABELS[user];
  document.getElementById('nav-admin').style.display = user==='admin' ? 'block' : 'none';
  loadAll().then(() => showTab('dashboard'));
}

document.getElementById('login-pin').addEventListener('keydown', e => { if(e.key==='Enter') doLogin(); });

function doLogout() {
  currentUser = null;
  document.getElementById('login-screen').style.display='flex';
  document.getElementById('app').style.display='none';
  document.getElementById('login-user').value='';
  document.getElementById('login-pin').value='';
}

function showTab(name) {
  ['dashboard','tools','log','admin'].forEach(t => { document.getElementById('tab-'+t).style.display = t===name?'block':'none'; });
  document.querySelectorAll('.nav-item').forEach(el => { el.classList.toggle('active', el.textContent.toLowerCase().startsWith(name==='admin'?'admin':name)); });
  if(name==='dashboard') renderDashboard();
  if(name==='tools') renderTools();
  if(name==='log') renderLog();
  if(name==='admin') renderAdmin();
}

function showAdminTab(name) {
  ['users','sites','addtool'].forEach(t => { document.getElementById('admin-'+t).style.display=t===name?'block':'none'; document.getElementById('atab-'+t).classList.toggle('active',t===name); });
}

function renderDashboard() {
  const out = tools.filter(t=>t.location!=='Workshop');
  document.getElementById('stat-total').textContent = tools.length;
  document.getElementById('stat-in').textContent = tools.filter(t=>t.location==='Workshop').length;
  document.getElementById('stat-out').textContent = out.length;
  const el = document.getElementById('dashboard-out-list');
  if(!tools.length){el.innerHTML='<div class="empty-state">No tools yet — add tools in Admin</div>';return;}
  if(!out.length){el.innerHTML='<div class="empty-state">All tools are in the workshop ✓</div>';return;}
  el.innerHTML=out.map(t=>`<div class="tool-row"><div class="tool-info"><div class="tool-name">${t.name}</div><div class="tool-meta">${t.id} · ${t.cat}${t.serial?' · S/N: '+t.serial:''}</div></div><span class="badge badge-out">${t.location}</span></div>`).join('');
}

function renderTools() {
  const q=(document.getElementById('tool-search').value||'').toLowerCase();
  const f=tools.filter(t=>t.name.toLowerCase().includes(q)||t.id.toLowerCase().includes(q)||t.cat.toLowerCase().includes(q)||t.serial.toLowerCase().includes(q));
  const el=document.getElementById('tools-list');
  if(!f.length){el.innerHTML='<div class="empty-state">No tools found</div>';return;}
  el.innerHTML=f.map(t=>`<div class="tool-row"><div class="tool-info"><div class="tool-name">${t.name}</div><div class="tool-meta">${t.id} · ${t.cat}${t.serial?' · S/N: '+t.serial:''}${t.notes?' · '+t.notes:''}</div></div><span class="badge ${t.location==='Workshop'?'badge-workshop':'badge-out'}">${t.location}</span></div>`).join('');
}

function clearLogFilter(){ document.getElementById('log-date-from').value=''; document.getElementById('log-date-to').value=''; renderLog(); }

function renderLog() {
  const el=document.getElementById('activity-log-list');
  const fromVal=document.getElementById('log-date-from').value;
  const toVal=document.getElementById('log-date-to').value;
  let entries=[...activityLog].reverse();
  if(fromVal){const from=new Date(fromVal);entries=entries.filter(e=>new Date(e.time)>=from);}
  if(toVal){const to=new Date(toVal);to.setHours(23,59,59);entries=entries.filter(e=>new Date(e.time)<=to);}
  if(!entries.length){el.innerHTML='<div class="empty-state">No activity in this period</div>';return;}
  el.innerHTML=entries.map(e=>{
    let dotClass='log-dot-out',text='';
    if(e.type==='checkout') text=`<strong>${e.toolName}</strong> (${e.tool}) checked out to <span class="highlight">${e.to}</span> by ${e.user}`;
    else if(e.type==='return'){dotClass='log-dot-return';text=`<strong>${e.toolName}</strong> (${e.tool}) returned to workshop by ${e.user}${e.notes?' — '+e.notes:''}`;}
    else if(e.type==='transfer'){dotClass='log-dot-transfer';text=`<strong>${e.toolName}</strong> (${e.tool}) transferred <span class="highlight">${e.from}</span> → <span class="highlight">${e.to}</span> by ${e.user}`;}
    return `<div class="log-row"><div class="log-dot ${dotClass}"></div><div class="log-text">${text}</div><div class="log-time">${e.time}</div></div>`;
  }).join('');
}

function renderAdmin() {
  document.getElementById('users-list').innerHTML=Object.entries(USER_LABELS).map(([k,v])=>`<div class="tool-row"><div class="tool-info"><div class="tool-name">${v}</div><div class="tool-meta">${k==='admin'?'Administrator':'Supervisor'}</div></div><span class="badge" style="background:var(--surface2);color:var(--muted)">${k==='admin'?'admin':'user'}</span></div>`).join('');
  document.getElementById('sites-list').innerHTML=sites.map(s=>`<span class="site-pill">${s}</span>`).join('');
}

function openModal(type) {
  const out=tools.filter(t=>t.location!=='Workshop');
  const inWs=tools.filter(t=>t.location==='Workshop');
  const nonWs=sites.filter(s=>s!=='Workshop');
  if(type==='checkout'){
    document.getElementById('co-tool').innerHTML=inWs.length?inWs.map(t=>`<option value="${t.id}">${t.name} (${t.id})</option>`).join(''):'<option>No tools in workshop</option>';
    document.getElementById('co-site').innerHTML=nonWs.map(s=>`<option>${s}</option>`).join('');
    document.getElementById('co-notes').value='';
  }
  if(type==='return'){
    document.getElementById('ret-tool').innerHTML=out.length?out.map(t=>`<option value="${t.id}">${t.name} — ${t.location} (${t.id})</option>`).join(''):'<option>No tools out on site</option>';
    document.getElementById('ret-notes').value='';
  }
  if(type==='transfer'){
    document.getElementById('tr-tool').innerHTML=out.length?out.map(t=>`<option value="${t.id}">${t.name} — ${t.location} (${t.id})</option>`).join(''):'<option>No tools out on site</option>';
    document.getElementById('tr-site').innerHTML=nonWs.map(s=>`<option>${s}</option>`).join('');
    document.getElementById('tr-notes').value='';
  }
  document.getElementById('modal-'+type).classList.add('open');
}

function closeModal(type){document.getElementById('modal-'+type).classList.remove('open');}

async function doCheckout(){
  const toolId=document.getElementById('co-tool').value;
  const site=document.getElementById('co-site').value;
  const notes=document.getElementById('co-notes').value;
  if(!toolId||!site)return;
  const tool=getTool(toolId);if(!tool)return;
  const btn=document.getElementById('btn-co');btn.disabled=true;btn.textContent='Saving...';
  const ts=now();
  try{
    await apiCall({action:'updateTool',data:JSON.stringify({id:toolId,location:site,condition:tool.notes})});
    await apiCall({action:'addLog',data:JSON.stringify({timestamp:ts,type:'checkout',tool:toolId,toolName:tool.name,from:tool.location,to:site,user:USER_LABELS[currentUser]})});
    tool.location=site;
    activityLog.push({time:ts,type:'checkout',tool:toolId,toolName:tool.name,from:'Workshop',to:site,user:USER_LABELS[currentUser]});
    closeModal('checkout');renderDashboard();showToast('✓ Checked out to '+site);
  }catch(e){showToast('Error saving — try again');}
  btn.disabled=false;btn.textContent='Confirm checkout';
}

async function doReturn(){
  const toolId=document.getElementById('ret-tool').value;
  const notes=document.getElementById('ret-notes').value;
  if(!toolId)return;
  const tool=getTool(toolId);if(!tool)return;
  const btn=document.getElementById('btn-ret');btn.disabled=true;btn.textContent='Saving...';
  const ts=now();
  try{
    await apiCall({action:'updateTool',data:JSON.stringify({id:toolId,location:'Workshop',condition:notes})});
    await apiCall({action:'addLog',data:JSON.stringify({timestamp:ts,type:'return',tool:toolId,toolName:tool.name,from:tool.location,to:'Workshop',user:USER_LABELS[currentUser],notes})});
    tool.location='Workshop';tool.notes=notes;
    activityLog.push({time:ts,type:'return',tool:toolId,toolName:tool.name,from:tool.location,to:'Workshop',user:USER_LABELS[currentUser],notes});
    closeModal('return');renderDashboard();showToast('✓ '+tool.name+' returned to workshop');
  }catch(e){showToast('Error saving — try again');}
  btn.disabled=false;btn.textContent='Confirm return';
}

async function doTransfer(){
  const toolId=document.getElementById('tr-tool').value;
  const toSite=document.getElementById('tr-site').value;
  const notes=document.getElementById('tr-notes').value;
  if(!toolId||!toSite)return;
  const tool=getTool(toolId);if(!tool)return;
  const btn=document.getElementById('btn-tr');btn.disabled=true;btn.textContent='Saving...';
  const from=tool.location;const ts=now();
  try{
    await apiCall({action:'updateTool',data:JSON.stringify({id:toolId,location:toSite,condition:tool.notes})});
    await apiCall({action:'addLog',data:JSON.stringify({timestamp:ts,type:'transfer',tool:toolId,toolName:tool.name,from,to:toSite,user:USER_LABELS[currentUser]})});
    tool.location=toSite;
    activityLog.push({time:ts,type:'transfer',tool:toolId,toolName:tool.name,from,to:toSite,user:USER_LABELS[currentUser]});
    closeModal('transfer');renderDashboard();showToast('✓ Transferred to '+toSite);
  }catch(e){showToast('Error saving — try again');}
  btn.disabled=false;btn.textContent='Confirm transfer';
}

async function addSite(){
  const name=document.getElementById('new-site-name').value.trim();
  if(!name||sites.includes(name))return;
  try{
    await apiCall({action:'addSite',data:JSON.stringify({name})});
    sites.push(name);document.getElementById('new-site-name').value='';
    renderAdmin();showToast('✓ Site added: '+name);
  }catch(e){showToast('Error adding site');}
}

async function addTool(){
  const id=document.getElementById('new-tool-id').value.trim();
  const name=document.getElementById('new-tool-name').value.trim();
  const cat=document.getElementById('new-tool-cat').value;
  const serial=document.getElementById('new-tool-serial').value.trim();
  const notes=document.getElementById('new-tool-notes').value.trim();
  const err=document.getElementById('add-tool-error');
  if(!id||!name){err.style.display='block';return;}
  if(tools.find(t=>t.id===id)){err.style.display='block';err.textContent='Asset number already exists.';return;}
  err.style.display='none';
  try{
    await apiCall({action:'addTool',data:JSON.stringify({id,name,cat,notes:serial,condition:notes})});
    await apiCall({action:'addLog',data:JSON.stringify({timestamp:now(),type:'checkout',tool:id,toolName:name,from:'',to:'Workshop (added)',user:USER_LABELS[currentUser]})});
    tools.push({id,name,cat,location:'Workshop',serial,notes});
    document.getElementById('new-tool-id').value='';
    document.getElementById('new-tool-name').value='';
    document.getElementById('new-tool-serial').value='';
    document.getElementById('new-tool-notes').value='';
    showToast('✓ '+name+' added to inventory');
  }catch(e){showToast('Error adding tool');}
}

document.querySelectorAll('.modal-overlay').forEach(el=>{el.addEventListener('click',e=>{if(e.target===el)el.classList.remove('open');});});
</script>
</body>
</html>
