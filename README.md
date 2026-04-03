[index.html](https://github.com/user-attachments/files/26452208/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RAPCLASS — Artist Operating System</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Mono:wght@300;400;500&family=Syne:wght@400;500;600;700&display=swap');

:root {
  --bg: #080808;
  --s1: #0f0f0f;
  --s2: #141414;
  --s3: #1a1a1a;
  --border: #202020;
  --border2: #2a2a2a;
  --text: #e8e8e8;
  --muted: #555;
  --dim: #333;
  --c0: #6dbe45;   /* framework - green */
  --c1: #d4b832;   /* human - yellow */
  --c2: #3d8fe0;   /* business - blue */
  --c3: #d63f82;   /* growth - pink */
  --c4: #9b4de8;   /* artistry - purple */
  --cn: #666;      /* neutral */
  --sidebar: 240px;
}

* { margin:0; padding:0; box-sizing:border-box; }
body { background:var(--bg); color:var(--text); font-family:'Syne',sans-serif; font-size:13px; line-height:1.6; overflow-x:hidden; }
::-webkit-scrollbar { width:3px; height:3px; }
::-webkit-scrollbar-track { background:transparent; }
::-webkit-scrollbar-thumb { background:var(--dim); border-radius:2px; }

/* ── APP SHELL ── */
#app { display:flex; flex-direction:column; min-height:100vh; }

.topbar {
  height:48px; background:var(--s1); border-bottom:1px solid var(--border);
  display:flex; align-items:center; padding:0 20px; gap:16px;
  position:sticky; top:0; z-index:90; flex-shrink:0;
}
.topbar-logo { font-family:'Bebas Neue',sans-serif; font-size:20px; letter-spacing:4px; color:var(--text); cursor:pointer; }
.topbar-logo span { color:var(--c0); }

.search-wrap {
  flex:1; max-width:400px; position:relative;
}
.search-input {
  width:100%; padding:7px 12px 7px 32px;
  background:var(--s2); border:1px solid var(--border);
  color:var(--text); font-family:'DM Mono',monospace; font-size:11px;
  outline:none; transition:border-color 0.2s;
}
.search-input:focus { border-color:var(--border2); }
.search-input::placeholder { color:var(--muted); }
.search-icon { position:absolute; left:10px; top:50%; transform:translateY(-50%); color:var(--muted); font-size:11px; }

.search-results {
  position:absolute; top:calc(100% + 4px); left:0; right:0;
  background:var(--s2); border:1px solid var(--border2);
  z-index:200; max-height:320px; overflow-y:auto;
  display:none;
}
.search-results.open { display:block; }
.sr-item {
  padding:10px 14px; display:flex; align-items:center; gap:10px;
  cursor:pointer; border-bottom:1px solid var(--border); transition:background 0.15s;
}
.sr-item:hover { background:var(--s3); }
.sr-item:last-child { border-bottom:none; }
.sr-tag { font-family:'DM Mono',monospace; font-size:8px; letter-spacing:1px; text-transform:uppercase; padding:2px 6px; border:1px solid var(--border2); color:var(--muted); flex-shrink:0; }
.sr-title { font-size:12px; flex:1; }
.sr-dept { font-family:'DM Mono',monospace; font-size:9px; }

.topbar-right { margin-left:auto; display:flex; align-items:center; gap:12px; }
.mode-badge {
  font-family:'DM Mono',monospace; font-size:8px; letter-spacing:2px;
  text-transform:uppercase; padding:3px 8px; border:1px solid var(--border2);
  color:var(--muted); cursor:pointer; transition:all 0.2s;
}
.mode-badge:hover { color:var(--text); border-color:var(--dim); }
.mode-badge.full { border-color:var(--c0); color:var(--c0); }

.user-chip {
  display:flex; align-items:center; gap:8px;
  font-family:'DM Mono',monospace; font-size:10px; color:var(--muted);
  cursor:pointer;
}
.user-avatar {
  width:26px; height:26px; border-radius:50%;
  background:var(--s3); border:1px solid var(--border2);
  display:flex; align-items:center; justify-content:center;
  font-size:10px; color:var(--text);
}

/* ── MAIN LAYOUT ── */
.app-body { display:flex; flex:1; min-height:0; }

/* ── SIDEBAR ── */
.sidebar {
  width:var(--sidebar); flex-shrink:0;
  background:var(--s1); border-right:1px solid var(--border);
  overflow-y:auto; position:sticky; top:48px;
  height:calc(100vh - 48px);
}
.nav-section { padding:6px 0; border-bottom:1px solid var(--border); }
.nav-item {
  display:flex; align-items:center; gap:9px;
  padding:9px 16px; cursor:pointer; transition:all 0.15s;
  font-size:12px; color:var(--muted); position:relative;
}
.nav-item:hover { color:var(--text); background:var(--s2); }
.nav-item.active { color:var(--text); background:var(--s2); }
.nav-item.active::before {
  content:''; position:absolute; left:0; top:0; bottom:0;
  width:2px; background:var(--c0);
}
.nav-icon { width:14px; text-align:center; font-size:11px; flex-shrink:0; }
.nav-label { font-family:'DM Mono',monospace; font-size:9px; letter-spacing:1px; text-transform:uppercase; }
.nav-count { margin-left:auto; font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); background:var(--s3); padding:1px 5px; border-radius:2px; }

.nav-sub-section { padding:4px 0; }
.nav-sub-hdr { padding:6px 16px 3px; font-family:'DM Mono',monospace; font-size:8px; letter-spacing:2px; color:var(--muted); text-transform:uppercase; }
.nav-sub-item {
  display:flex; align-items:center; gap:8px;
  padding:6px 16px 6px 28px; cursor:pointer; transition:all 0.15s;
  font-family:'DM Mono',monospace; font-size:9px; letter-spacing:0.5px; color:var(--muted);
}
.nav-sub-item:hover { color:var(--text); }
.nav-sub-item.active { color:var(--text); }
.nav-sub-pip { width:5px; height:5px; border-radius:50%; flex-shrink:0; }

/* ── MAIN CONTENT ── */
.main { flex:1; overflow-y:auto; }
.view { display:none; padding:36px 40px; max-width:1100px; animation:fadeIn 0.2s ease; }
.view.active { display:block; }
@keyframes fadeIn { from { opacity:0; } to { opacity:1; } }

/* ── SHARED COMPONENTS ── */
.page-hdr { margin-bottom:32px; }
.page-title { font-family:'Bebas Neue',sans-serif; font-size:44px; letter-spacing:3px; line-height:1; margin-bottom:6px; }
.page-sub { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); letter-spacing:2px; text-transform:uppercase; }

.card { background:var(--s2); border:1px solid var(--border); padding:20px; }
.card-sm { background:var(--s2); border:1px solid var(--border); padding:14px 16px; }

.section-lbl {
  font-family:'DM Mono',monospace; font-size:9px; letter-spacing:2px;
  text-transform:uppercase; color:var(--muted); margin-bottom:12px;
  display:flex; align-items:center; gap:8px;
}
.section-lbl::after { content:''; flex:1; height:1px; background:var(--border); }

.tag {
  font-family:'DM Mono',monospace; font-size:8px; letter-spacing:1px;
  text-transform:uppercase; padding:2px 7px; border:1px solid currentColor;
  display:inline-block; opacity:0.7;
}
.btn {
  padding:10px 18px; font-family:'DM Mono',monospace; font-size:10px;
  letter-spacing:2px; text-transform:uppercase; cursor:pointer; border:1px solid;
  transition:all 0.2s; background:transparent;
}
.btn-outline { border-color:var(--border2); color:var(--muted); }
.btn-outline:hover { border-color:var(--text); color:var(--text); }
.btn-primary { border-color:var(--c0); color:var(--c0); }
.btn-primary:hover { background:var(--c0); color:#000; }

/* ── HOME / DASHBOARD ── */
.home-grid { display:grid; grid-template-columns:1fr 300px; gap:24px; margin-bottom:32px; }
.daily-card {
  border:1px solid var(--border); border-left:3px solid var(--c0);
  background:var(--s2); padding:18px 22px;
}
.daily-lbl { font-family:'DM Mono',monospace; font-size:9px; letter-spacing:2px; color:var(--c0); text-transform:uppercase; margin-bottom:8px; }
.daily-txt { font-size:13px; line-height:1.75; color:var(--text); }
.stats-stack { display:flex; flex-direction:column; gap:8px; }
.stat-row {
  background:var(--s2); border:1px solid var(--border);
  padding:12px 16px; display:flex; align-items:center; justify-content:space-between;
}
.stat-n { font-family:'Bebas Neue',sans-serif; font-size:28px; letter-spacing:2px; line-height:1; }
.stat-lbl { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); text-transform:uppercase; letter-spacing:1px; }

.quick-access-grid { display:grid; grid-template-columns:repeat(3,1fr); gap:10px; margin-bottom:32px; }
.qa-tile {
  background:var(--s2); border:1px solid var(--border);
  padding:18px 16px; cursor:pointer; transition:all 0.2s;
  position:relative; overflow:hidden;
}
.qa-tile:hover { border-color:var(--border2); background:var(--s3); }
.qa-tile::before { content:''; position:absolute; top:0; left:0; right:0; height:2px; }
.qa-icon { font-size:18px; margin-bottom:10px; }
.qa-title { font-family:'DM Mono',monospace; font-size:10px; letter-spacing:1px; text-transform:uppercase; margin-bottom:4px; }
.qa-desc { font-size:11px; color:var(--muted); line-height:1.5; }

.dept-progress { display:grid; grid-template-columns:repeat(5,1fr); gap:8px; }
.dept-card {
  background:var(--s2); border:1px solid var(--border);
  padding:14px; cursor:pointer; transition:border-color 0.2s;
}
.dept-card:hover { border-color:var(--border2); }
.dept-label { font-family:'DM Mono',monospace; font-size:8px; letter-spacing:1px; text-transform:uppercase; margin-bottom:8px; }
.dept-pct { font-family:'Bebas Neue',sans-serif; font-size:28px; letter-spacing:1px; line-height:1; margin-bottom:8px; }
.dept-bar { height:2px; background:var(--dim); margin-bottom:6px; }
.dept-fill { height:100%; transition:width 0.5s ease; }
.dept-count { font-family:'DM Mono',monospace; font-size:8px; color:var(--muted); }

/* ── SOP LIBRARY ── */
.filter-row { display:flex; gap:8px; margin-bottom:20px; flex-wrap:wrap; }
.filter-btn {
  font-family:'DM Mono',monospace; font-size:9px; letter-spacing:1px;
  text-transform:uppercase; padding:5px 10px; border:1px solid var(--border);
  color:var(--muted); cursor:pointer; transition:all 0.2s; background:transparent;
}
.filter-btn:hover { color:var(--text); border-color:var(--border2); }
.filter-btn.active { color:var(--text); border-color:var(--text); }

.sop-grid { display:flex; flex-direction:column; gap:3px; }
.sop-row {
  background:var(--s2); border:1px solid var(--border);
  display:flex; align-items:center; gap:16px;
  padding:13px 18px; cursor:pointer; transition:border-color 0.15s;
}
.sop-row:hover { border-color:var(--border2); }
.sop-dept-pip { width:6px; height:6px; border-radius:50%; flex-shrink:0; }
.sop-title { flex:1; font-size:13px; }
.sop-dept-tag { font-family:'DM Mono',monospace; font-size:8px; letter-spacing:1px; text-transform:uppercase; color:var(--muted); width:80px; flex-shrink:0; }
.sop-type { font-family:'DM Mono',monospace; font-size:8px; border:1px solid var(--border); padding:2px 6px; color:var(--muted); letter-spacing:1px; text-transform:uppercase; }
.sop-arrow { color:var(--muted); font-size:11px; margin-left:6px; }

/* ── SOP VIEWER ── */
.sop-viewer { display:none; }
.sop-viewer.active { display:block; }
.sop-back { display:flex; align-items:center; gap:8px; margin-bottom:24px; cursor:pointer; font-family:'DM Mono',monospace; font-size:9px; letter-spacing:1px; color:var(--muted); transition:color 0.15s; text-transform:uppercase; }
.sop-back:hover { color:var(--text); }
.sop-content { background:var(--s2); border:1px solid var(--border); padding:28px; }
.sop-step-list { display:flex; flex-direction:column; gap:2px; margin-top:16px; }
.sop-step {
  display:flex; align-items:flex-start; gap:14px;
  padding:14px 16px; background:var(--s1); border:1px solid var(--border);
  cursor:pointer; transition:border-color 0.15s;
}
.sop-step.done { opacity:0.5; }
.sop-step-num {
  font-family:'Bebas Neue',sans-serif; font-size:20px; letter-spacing:1px;
  line-height:1; color:var(--muted); flex-shrink:0; width:24px;
}
.sop-step-body { flex:1; }
.sop-step-title { font-size:13px; margin-bottom:3px; }
.sop-step-note { font-size:11px; color:var(--muted); line-height:1.5; }
.sop-step-chk {
  width:18px; height:18px; border:1px solid var(--dim);
  flex-shrink:0; margin-top:1px; display:flex; align-items:center;
  justify-content:center; font-size:10px; transition:all 0.2s;
}
.sop-step-chk.done { color:#000; }
.sop-progress-bar { height:3px; background:var(--dim); margin-bottom:24px; }
.sop-progress-fill { height:100%; transition:width 0.3s ease; }
.sop-progress-txt { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); margin-bottom:16px; }

/* ── WORKSHEETS ── */
.ws-dept-tabs { display:flex; gap:4px; margin-bottom:20px; flex-wrap:wrap; }
.ws-tab {
  font-family:'DM Mono',monospace; font-size:9px; letter-spacing:1px;
  text-transform:uppercase; padding:6px 12px; border:1px solid var(--border);
  color:var(--muted); cursor:pointer; transition:all 0.2s; background:transparent;
}
.ws-tab:hover { color:var(--text); }
.ws-tab.active { color:var(--text); border-color:var(--text); }

.ws-grid { display:grid; grid-template-columns:repeat(2,1fr); gap:10px; }
.ws-card {
  background:var(--s2); border:1px solid var(--border);
  padding:18px; cursor:pointer; transition:border-color 0.2s;
  display:flex; flex-direction:column; gap:8px;
}
.ws-card:hover { border-color:var(--border2); }
.ws-card-title { font-size:13px; }
.ws-card-meta { display:flex; gap:8px; align-items:center; }
.ws-card-dept { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); }
.ws-card-type { font-family:'DM Mono',monospace; font-size:8px; border:1px solid var(--border); padding:2px 5px; color:var(--muted); letter-spacing:1px; text-transform:uppercase; }
.ws-card-status { margin-top:auto; font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); display:flex; align-items:center; gap:6px; }
.ws-dot { width:5px; height:5px; border-radius:50%; background:var(--dim); }
.ws-dot.started { background:var(--c1); }
.ws-dot.done { background:var(--c0); }

/* Worksheet form */
.ws-form { background:var(--s2); border:1px solid var(--border); padding:28px; }
.ws-field-group { margin-bottom:24px; }
.ws-field-lbl { font-family:'DM Mono',monospace; font-size:9px; letter-spacing:2px; text-transform:uppercase; color:var(--muted); margin-bottom:8px; display:block; }
.ws-input {
  width:100%; background:var(--s1); border:1px solid var(--border);
  color:var(--text); font-family:'DM Mono',monospace; font-size:12px;
  padding:10px 12px; outline:none; resize:vertical;
  transition:border-color 0.2s;
}
.ws-input:focus { border-color:var(--border2); }
.ws-select {
  width:100%; background:var(--s1); border:1px solid var(--border);
  color:var(--text); font-family:'DM Mono',monospace; font-size:12px;
  padding:10px 12px; outline:none; cursor:pointer;
}
.ws-table { width:100%; border-collapse:collapse; }
.ws-table th, .ws-table td { border:1px solid var(--border); padding:8px 10px; text-align:left; }
.ws-table th { font-family:'DM Mono',monospace; font-size:8px; letter-spacing:1px; text-transform:uppercase; color:var(--muted); background:var(--s1); }
.ws-table td input { width:100%; background:transparent; border:none; color:var(--text); font-family:'DM Mono',monospace; font-size:11px; outline:none; }
.ws-save-row { display:flex; gap:10px; margin-top:24px; align-items:center; }
.ws-save-msg { font-family:'DM Mono',monospace; font-size:9px; color:var(--c0); }

/* ── GOAL TRACKER ── */
.goal-overview { display:grid; grid-template-columns:repeat(3,1fr); gap:12px; margin-bottom:28px; }
.goal-stat { background:var(--s2); border:1px solid var(--border); padding:16px; }
.goal-stat-n { font-family:'Bebas Neue',sans-serif; font-size:36px; letter-spacing:2px; line-height:1; margin-bottom:4px; }
.goal-stat-l { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); text-transform:uppercase; letter-spacing:1px; }

.goal-form { background:var(--s2); border:1px solid var(--border); padding:20px; margin-bottom:20px; }
.goal-form-title { font-family:'DM Mono',monospace; font-size:9px; letter-spacing:2px; text-transform:uppercase; color:var(--muted); margin-bottom:14px; }
.goal-inputs { display:grid; grid-template-columns:1fr 1fr 120px auto; gap:8px; align-items:end; }
.goal-list { display:flex; flex-direction:column; gap:4px; }
.goal-item {
  background:var(--s2); border:1px solid var(--border);
  padding:14px 18px; display:flex; align-items:flex-start; gap:14px;
}
.goal-item.done { opacity:0.5; }
.goal-chk {
  width:16px; height:16px; border:1px solid var(--dim); flex-shrink:0;
  margin-top:2px; cursor:pointer; display:flex; align-items:center;
  justify-content:center; font-size:9px; transition:all 0.2s;
}
.goal-body { flex:1; }
.goal-title-txt { font-size:13px; margin-bottom:4px; }
.goal-meta { display:flex; gap:12px; }
.goal-meta-item { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); }
.goal-actions { display:flex; gap:6px; flex-shrink:0; }
.goal-del { background:none; border:none; color:var(--muted); cursor:pointer; font-size:11px; padding:2px; transition:color 0.15s; }
.goal-del:hover { color:#e05050; }

.goal-timeline { margin-top:28px; }
.timeline-grid { display:grid; grid-template-columns:repeat(12,1fr); gap:4px; }
.month-cell {
  background:var(--s2); border:1px solid var(--border);
  padding:8px; text-align:center; cursor:pointer; transition:border-color 0.2s;
}
.month-cell:hover { border-color:var(--border2); }
.month-cell.has-goal { border-color:var(--c1); }
.month-name { font-family:'DM Mono',monospace; font-size:8px; letter-spacing:1px; text-transform:uppercase; color:var(--muted); margin-bottom:4px; }
.month-count { font-family:'Bebas Neue',sans-serif; font-size:18px; line-height:1; }

/* ── PROGRESS ── */
.progress-header { display:grid; grid-template-columns:repeat(4,1fr); gap:10px; margin-bottom:28px; }
.prog-card { background:var(--s2); border:1px solid var(--border); padding:16px; }
.prog-num { font-family:'Bebas Neue',sans-serif; font-size:36px; letter-spacing:2px; line-height:1; margin-bottom:4px; }
.prog-lbl { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); text-transform:uppercase; letter-spacing:1px; }

.streak-banner {
  border:1px solid var(--border); border-left:3px solid var(--c1);
  background:var(--s2); padding:16px 20px; margin-bottom:28px;
  display:flex; align-items:center; gap:20px;
}
.streak-n { font-family:'Bebas Neue',sans-serif; font-size:48px; letter-spacing:2px; line-height:1; color:var(--c1); }
.streak-lbl { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); letter-spacing:2px; text-transform:uppercase; }
.streak-txt { font-size:12px; color:var(--text); margin-top:4px; }

.dept-progress-full { display:flex; flex-direction:column; gap:8px; margin-bottom:28px; }
.dept-prog-row { background:var(--s2); border:1px solid var(--border); padding:14px 18px; }
.dept-prog-hdr { display:flex; align-items:center; gap:10px; margin-bottom:10px; }
.dept-prog-name { font-size:13px; flex:1; }
.dept-prog-pct { font-family:'DM Mono',monospace; font-size:10px; }
.dept-prog-bar { height:3px; background:var(--dim); }
.dept-prog-fill { height:100%; transition:width 0.4s ease; }

.recent-list { display:flex; flex-direction:column; gap:3px; }
.recent-item {
  background:var(--s2); border:1px solid var(--border);
  padding:10px 16px; display:flex; align-items:center; gap:12px; font-size:12px;
}
.recent-pip { width:5px; height:5px; border-radius:50%; flex-shrink:0; }
.recent-time { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); margin-left:auto; }

/* ── SETTINGS ── */
.settings-section { background:var(--s2); border:1px solid var(--border); padding:24px; margin-bottom:16px; }
.settings-title { font-family:'DM Mono',monospace; font-size:10px; letter-spacing:2px; text-transform:uppercase; color:var(--text); margin-bottom:16px; padding-bottom:12px; border-bottom:1px solid var(--border); }
.setting-row { display:flex; align-items:center; justify-content:space-between; padding:10px 0; border-bottom:1px solid var(--border); }
.setting-row:last-child { border-bottom:none; padding-bottom:0; }
.setting-label { font-size:12px; }
.setting-desc { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); margin-top:2px; }
.toggle {
  width:36px; height:20px; background:var(--dim); border-radius:10px;
  cursor:pointer; position:relative; transition:background 0.2s; flex-shrink:0;
}
.toggle.on { background:var(--c0); }
.toggle::after {
  content:''; position:absolute; width:14px; height:14px;
  background:var(--text); border-radius:50%; top:3px; left:3px;
  transition:transform 0.2s;
}
.toggle.on::after { transform:translateX(16px); }
.setting-select {
  background:var(--s1); border:1px solid var(--border); color:var(--text);
  font-family:'DM Mono',monospace; font-size:10px; padding:5px 8px; outline:none;
}

/* ── CURRICULUM (LEARN MODE) ── */
.learn-grid { display:grid; grid-template-columns:repeat(2,1fr); gap:12px; }
.learn-card {
  background:var(--s2); border:1px solid var(--border);
  padding:20px; cursor:pointer; transition:border-color 0.2s;
  position:relative; overflow:hidden;
}
.learn-card:hover { border-color:var(--border2); }
.learn-bg-num {
  position:absolute; top:8px; right:14px;
  font-family:'Bebas Neue',sans-serif; font-size:64px; letter-spacing:4px;
  line-height:1; opacity:0.06; pointer-events:none;
}
.learn-lbl { font-family:'DM Mono',monospace; font-size:8px; letter-spacing:2px; text-transform:uppercase; margin-bottom:5px; }
.learn-title { font-family:'Bebas Neue',sans-serif; font-size:22px; letter-spacing:2px; margin-bottom:8px; }
.learn-desc { font-size:11px; color:var(--muted); margin-bottom:14px; line-height:1.6; }
.learn-bar { height:2px; background:var(--dim); margin-bottom:6px; }
.learn-fill { height:100%; transition:width 0.4s ease; }
.learn-count { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); }

/* ── LOADING ── */
.skel { height:11px; background:linear-gradient(90deg,var(--border) 0%,var(--dim) 50%,var(--border) 100%); background-size:200% 100%; animation:shimmer 1.4s infinite; border-radius:2px; }
.skel.m { width:75%; } .skel.s { width:50%; }
@keyframes shimmer { 0% { background-position:200% 0; } 100% { background-position:-200% 0; } }
.skeleton-stack { display:flex; flex-direction:column; gap:7px; }

/* ── LEARN MODE BADGE ── */
.learn-only { display:none; }
body.full-mode .learn-only { display:flex; }
body.full-mode .learn-only-block { display:block; }
.learn-only-block { display:none; }

/* ── STRIPPED MODE NOTICE ── */
.stripped-notice {
  background:var(--s2); border:1px solid var(--border); border-left:3px solid var(--c1);
  padding:12px 16px; margin-bottom:20px;
  font-family:'DM Mono',monospace; font-size:10px; color:var(--muted); line-height:1.6;
}

/* ── TOAST ── */
.toast {
  position:fixed; bottom:24px; right:24px; background:var(--s3);
  border:1px solid var(--border2); padding:10px 16px;
  font-family:'DM Mono',monospace; font-size:10px; color:var(--text);
  letter-spacing:1px; z-index:999; opacity:0; transform:translateY(8px);
  transition:all 0.25s; pointer-events:none;
}
.toast.show { opacity:1; transform:translateY(0); }

/* ── CURRICULUM MODULE / LESSON VIEWS ── */
.module-list { display:flex; flex-direction:column; gap:2px; }
.module-row {
  background:var(--s2); border:1px solid var(--border);
  padding:14px 18px; display:flex; align-items:center; gap:14px;
  cursor:pointer; transition:border-color 0.15s;
}
.module-row:hover { border-color:var(--border2); }
.mod-id { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); width:32px; flex-shrink:0; }
.mod-name { flex:1; font-size:13px; }
.mod-prog { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); }

.lesson-list { display:flex; flex-direction:column; gap:2px; }
.lesson-row {
  background:var(--s2); border:1px solid var(--border);
  padding:12px 18px; display:flex; align-items:center; gap:12px;
  cursor:pointer; transition:border-color 0.15s;
}
.lesson-row:hover { border-color:var(--border2); }
.les-id { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); width:36px; }
.les-name { flex:1; font-size:12px; }
.les-status { font-family:'DM Mono',monospace; font-size:9px; color:var(--muted); display:flex; align-items:center; gap:5px; }
.les-dot { width:5px; height:5px; border-radius:50%; background:var(--dim); }

/* lesson view */
.lesson-view-content { display:grid; grid-template-columns:1fr 280px; gap:24px; }
.content-box { background:var(--s2); border:1px solid var(--border); padding:22px; margin-bottom:16px; }
.box-lbl { font-family:'DM Mono',monospace; font-size:9px; letter-spacing:2px; text-transform:uppercase; margin-bottom:12px; display:flex; align-items:center; gap:8px; }
.box-lbl::after { content:''; flex:1; height:1px; background:var(--border); }
.les-txt { font-size:13px; line-height:1.85; }
.les-txt p { margin-bottom:11px; }
.les-txt p:last-child { margin-bottom:0; }
.takeaway-row { display:flex; gap:9px; padding:8px 0; border-bottom:1px solid var(--border); font-size:12px; }
.takeaway-row:last-child { border-bottom:none; padding-bottom:0; }
.t-arr { font-family:'DM Mono',monospace; flex-shrink:0; margin-top:1px; }
.chk-item { display:flex; align-items:flex-start; gap:9px; padding:9px 0; border-bottom:1px solid var(--border); cursor:pointer; }
.chk-item:last-child { border-bottom:none; padding-bottom:0; }
.chkbox { width:13px; height:13px; border:1px solid var(--dim); flex-shrink:0; margin-top:2px; display:flex; align-items:center; justify-content:center; font-size:8px; transition:all 0.2s; }
.chktxt { font-size:11px; line-height:1.5; }
.chktxt.done { color:var(--muted); text-decoration:line-through; }
.complete-btn { width:100%; padding:12px; background:transparent; border:1px solid var(--border); color:var(--muted); font-family:'DM Mono',monospace; font-size:10px; letter-spacing:2px; text-transform:uppercase; cursor:pointer; transition:all 0.2s; margin-bottom:8px; }
.complete-btn:hover { border-color:var(--text); color:var(--text); }
.complete-btn.done { color:#000!important; }
.nav-btns { display:flex; gap:7px; }
.nav-btn { flex:1; padding:9px; background:transparent; border:1px solid var(--border); color:var(--muted); font-family:'DM Mono',monospace; font-size:9px; letter-spacing:1px; cursor:pointer; transition:all 0.2s; text-align:center; }
.nav-btn:hover { border-color:var(--border2); color:var(--text); }

/* back nav */
.back-nav { display:flex; align-items:center; gap:8px; margin-bottom:24px; cursor:pointer; font-family:'DM Mono',monospace; font-size:9px; letter-spacing:1px; color:var(--muted); transition:color 0.15s; text-transform:uppercase; }
.back-nav:hover { color:var(--text); }
</style>
</head>
<body>

<!-- APP -->
<div id="app">
  <div class="topbar">
    <div class="topbar-logo" onclick="showView('home')">RAP<span>CLASS</span></div>
    <div class="search-wrap">
      <span class="search-icon">⌕</span>
      <input class="search-input" id="searchInput" type="text" placeholder="Search SOPs, worksheets, topics..." autocomplete="off" oninput="handleSearch(this.value)" onfocus="openSearch()" onblur="setTimeout(closeSearch,200)">
      <div class="search-results" id="searchResults"></div>
    </div>
    <div class="topbar-right">
      <div class="mode-badge" id="modeBadge" onclick="toggleMode()">Stripped Mode</div>
      <div class="user-chip" onclick="showView('settings')">
        <div class="user-avatar" id="userAvatar">A</div>
        <span id="userName" style="font-family:'DM Mono',monospace;font-size:10px;color:var(--muted);">Artist</span>
      </div>
    </div>
  </div>

  <div class="app-body">
    <nav class="sidebar">
      <div class="nav-section">
        <div class="nav-item active" id="nav-home" onclick="showView('home')">
          <span class="nav-icon">◈</span>
          <span class="nav-label">Home</span>
        </div>
        <div class="nav-item" id="nav-search" onclick="showView('search')">
          <span class="nav-icon">⌕</span>
          <span class="nav-label">Search</span>
        </div>
      </div>
      <div class="nav-section">
        <div class="nav-sub-hdr">Tools</div>
        <div class="nav-item" id="nav-sops" onclick="showView('sops')">
          <span class="nav-icon">≡</span>
          <span class="nav-label">SOPs</span>
          <span class="nav-count">32</span>
        </div>
        <div class="nav-item" id="nav-worksheets" onclick="showView('worksheets')">
          <span class="nav-icon">⊡</span>
          <span class="nav-label">Worksheets</span>
        </div>
        <div class="nav-item" id="nav-goals" onclick="showView('goals')">
          <span class="nav-icon">◎</span>
          <span class="nav-label">Goal Tracker</span>
        </div>
      </div>
      <div class="nav-section">
        <div class="nav-sub-hdr">You</div>
        <div class="nav-item" id="nav-progress" onclick="showView('progress')">
          <span class="nav-icon">▲</span>
          <span class="nav-label">My Progress</span>
        </div>
        <div class="nav-item learn-only" id="nav-learn" onclick="showView('learn')">
          <span class="nav-icon">◧</span>
          <span class="nav-label">Curriculum</span>
        </div>
        <div class="nav-item" id="nav-settings" onclick="showView('settings')">
          <span class="nav-icon">⚙</span>
          <span class="nav-label">Settings</span>
        </div>
      </div>
      <div class="nav-section">
        <div class="nav-sub-hdr">Departments</div>
        <div class="nav-sub-item" onclick="filterSOPs('human')">
          <div class="nav-sub-pip" style="background:var(--c1);"></div>Human System
        </div>
        <div class="nav-sub-item" onclick="filterSOPs('business')">
          <div class="nav-sub-pip" style="background:var(--c2);"></div>Business System
        </div>
        <div class="nav-sub-item" onclick="filterSOPs('growth')">
          <div class="nav-sub-pip" style="background:var(--c3);"></div>Growth System
        </div>
        <div class="nav-sub-item" onclick="filterSOPs('artist')">
          <div class="nav-sub-pip" style="background:var(--c4);"></div>Artistry System
        </div>
      </div>
    </nav>

    <main class="main">

      <!-- HOME -->
      <div class="view active" id="view-home">
        <div class="page-hdr">
          <div class="page-title" id="homeGreeting">WELCOME BACK</div>
          <div class="page-sub" id="homeDate">artist operating system</div>
        </div>

        <div class="stripped-notice learn-only-block" style="display:none;">
          Full mode is active. The Curriculum section and all advanced tools are unlocked.
        </div>

        <div class="home-grid">
          <div class="daily-card">
            <div class="daily-lbl">Daily Focus</div>
            <div class="daily-txt" id="dailyPrompt">Loading...</div>
          </div>
          <div class="stats-stack">
            <div class="stat-row">
              <div>
                <div class="stat-n" id="homeStat1" style="color:var(--c0);">0</div>
                <div class="stat-lbl">SOPs Run</div>
              </div>
              <div style="text-align:right;">
                <div class="stat-n" id="homeStat2" style="color:var(--c1);">0</div>
                <div class="stat-lbl">Worksheets</div>
              </div>
            </div>
            <div class="stat-row">
              <div>
                <div class="stat-n" id="homeStat3" style="color:var(--c2);">0</div>
                <div class="stat-lbl">Goals Set</div>
              </div>
              <div style="text-align:right;">
                <div class="stat-n" id="homeStat4" style="color:var(--c3);">0%</div>
                <div class="stat-lbl">Overall</div>
              </div>
            </div>
          </div>
        </div>

        <div class="section-lbl" style="margin-bottom:14px;">Quick Access</div>
        <div class="quick-access-grid" style="margin-bottom:32px;">
          <div class="qa-tile" onclick="showView('sops')" style="--qa-c:var(--c0);">
            <div style="width:3px;height:100%;position:absolute;left:0;top:0;background:var(--c0);"></div>
            <div style="padding-left:8px;">
              <div class="qa-icon">≡</div>
              <div class="qa-title" style="color:var(--c0);">Run a SOP</div>
              <div class="qa-desc">Step-by-step procedures for releases, shows, sessions, and more.</div>
            </div>
          </div>
          <div class="qa-tile" onclick="showView('worksheets')" style="">
            <div style="width:3px;height:100%;position:absolute;left:0;top:0;background:var(--c1);"></div>
            <div style="padding-left:8px;">
              <div class="qa-icon">⊡</div>
              <div class="qa-title" style="color:var(--c1);">Fill a Worksheet</div>
              <div class="qa-desc">Spending trackers, release calendars, goal sheets, and KPI forms.</div>
            </div>
          </div>
          <div class="qa-tile" onclick="showView('goals')">
            <div style="width:3px;height:100%;position:absolute;left:0;top:0;background:var(--c2);"></div>
            <div style="padding-left:8px;">
              <div class="qa-icon">◎</div>
              <div class="qa-title" style="color:var(--c2);">Track a Goal</div>
              <div class="qa-desc">Set quarterly and annual targets, break them into action steps.</div>
            </div>
          </div>
          <div class="qa-tile" onclick="searchFor('release')">
            <div style="width:3px;height:100%;position:absolute;left:0;top:0;background:var(--c3);"></div>
            <div style="padding-left:8px;">
              <div class="qa-icon">▷</div>
              <div class="qa-title" style="color:var(--c3);">Plan a Release</div>
              <div class="qa-desc">All SOPs and tools related to dropping music.</div>
            </div>
          </div>
          <div class="qa-tile" onclick="searchFor('show')">
            <div style="width:3px;height:100%;position:absolute;left:0;top:0;background:var(--c4);"></div>
            <div style="padding-left:8px;">
              <div class="qa-icon">◉</div>
              <div class="qa-title" style="color:var(--c4);">Book a Show</div>
              <div class="qa-desc">Performance SOPs, events checklists, and live show prep.</div>
            </div>
          </div>
          <div class="qa-tile" onclick="searchFor('money')">
            <div style="width:3px;height:100%;position:absolute;left:0;top:0;background:var(--cn);"></div>
            <div style="padding-left:8px;">
              <div class="qa-icon">$</div>
              <div class="qa-title" style="color:var(--cn);">Manage Money</div>
              <div class="qa-desc">ROI tracking, spending categories, budget worksheets.</div>
            </div>
          </div>
        </div>

        <div class="section-lbl" style="margin-bottom:14px;">Department Progress</div>
        <div class="dept-progress">
          <div class="dept-card" onclick="showView('learn')" style="border-top:2px solid var(--c0);">
            <div class="dept-label" style="color:var(--c0);">0.0 Framework</div>
            <div class="dept-pct" id="dp0" style="color:var(--c0);">0%</div>
            <div class="dept-bar"><div class="dept-fill" id="df0" style="background:var(--c0);width:0%;"></div></div>
            <div class="dept-count" id="dc0">0/42</div>
          </div>
          <div class="dept-card" onclick="showView('learn')" style="border-top:2px solid var(--c1);">
            <div class="dept-label" style="color:var(--c1);">1.0 Human</div>
            <div class="dept-pct" id="dp1" style="color:var(--c1);">0%</div>
            <div class="dept-bar"><div class="dept-fill" id="df1" style="background:var(--c1);width:0%;"></div></div>
            <div class="dept-count" id="dc1">0/42</div>
          </div>
          <div class="dept-card" onclick="showView('learn')" style="border-top:2px solid var(--c2);">
            <div class="dept-label" style="color:var(--c2);">2.0 Business</div>
            <div class="dept-pct" id="dp2" style="color:var(--c2);">0%</div>
            <div class="dept-bar"><div class="dept-fill" id="df2" style="background:var(--c2);width:0%;"></div></div>
            <div class="dept-count" id="dc2">0/42</div>
          </div>
          <div class="dept-card" onclick="showView('learn')" style="border-top:2px solid var(--c3);">
            <div class="dept-label" style="color:var(--c3);">3.0 Growth</div>
            <div class="dept-pct" id="dp3" style="color:var(--c3);">0%</div>
            <div class="dept-bar"><div class="dept-fill" id="df3" style="background:var(--c3);width:0%;"></div></div>
            <div class="dept-count" id="dc3">0/42</div>
          </div>
          <div class="dept-card" onclick="showView('learn')" style="border-top:2px solid var(--c4);">
            <div class="dept-label" style="color:var(--c4);">4.0 Artistry</div>
            <div class="dept-pct" id="dp4" style="color:var(--c4);">0%</div>
            <div class="dept-bar"><div class="dept-fill" id="df4" style="background:var(--c4);width:0%;"></div></div>
            <div class="dept-count" id="dc4">0/42</div>
          </div>
        </div>
      </div>

      <!-- SEARCH VIEW -->
      <div class="view" id="view-search">
        <div class="page-hdr">
          <div class="page-title">SEARCH</div>
          <div class="page-sub">SOPs and worksheets — find what you need fast</div>
        </div>
        <div style="margin-bottom:20px;">
          <input class="ws-input" id="searchViewInput" type="text" placeholder="Type a keyword: release, show, money, collab, contract..." oninput="renderSearchView(this.value)" style="font-size:14px;padding:14px 16px;">
        </div>
        <div id="searchViewResults"></div>
      </div>

      <!-- SOP LIBRARY -->
      <div class="view" id="view-sops">
        <div class="sop-viewer" id="sopViewer">
          <div class="back-nav" onclick="closeSopViewer()">← Back to SOPs</div>
          <div id="sopViewerContent"></div>
        </div>
        <div id="sopLibrary">
          <div class="page-hdr">
            <div class="page-title">SOP LIBRARY</div>
            <div class="page-sub">Standard operating procedures — run them step by step</div>
          </div>
          <div class="filter-row" id="sopFilters">
            <button class="filter-btn active" onclick="filterSOPs('all')">All</button>
            <button class="filter-btn" onclick="filterSOPs('human')" style="border-color:var(--c1);color:var(--c1);">Human</button>
            <button class="filter-btn" onclick="filterSOPs('business')" style="border-color:var(--c2);color:var(--c2);">Business</button>
            <button class="filter-btn" onclick="filterSOPs('growth')" style="border-color:var(--c3);color:var(--c3);">Growth</button>
            <button class="filter-btn" onclick="filterSOPs('artist')" style="border-color:var(--c4);color:var(--c4);">Artistry</button>
          </div>
          <div class="sop-grid" id="sopGrid"></div>
        </div>
      </div>

      <!-- WORKSHEETS -->
      <div class="view" id="view-worksheets">
        <div id="wsLibrary">
          <div class="page-hdr">
            <div class="page-title">WORKSHEETS</div>
            <div class="page-sub">Fill out, save, and revisit your working documents</div>
          </div>
          <div class="ws-dept-tabs" id="wsTabs">
            <button class="ws-tab active" onclick="filterWS('all')">All</button>
            <button class="ws-tab" onclick="filterWS('human')" style="border-color:var(--c1);color:var(--c1);">Human</button>
            <button class="ws-tab" onclick="filterWS('business')" style="border-color:var(--c2);color:var(--c2);">Business</button>
            <button class="ws-tab" onclick="filterWS('growth')" style="border-color:var(--c3);color:var(--c3);">Growth</button>
            <button class="ws-tab" onclick="filterWS('artist')" style="border-color:var(--c4);color:var(--c4);">Artistry</button>
          </div>
          <div class="ws-grid" id="wsGrid"></div>
        </div>
        <div id="wsViewer" style="display:none;">
          <div class="back-nav" onclick="closeWsViewer()">← Back to Worksheets</div>
          <div id="wsViewerContent"></div>
        </div>
      </div>

      <!-- GOAL TRACKER -->
      <div class="view" id="view-goals">
        <div class="page-hdr">
          <div class="page-title">GOAL TRACKER</div>
          <div class="page-sub">Set targets, break them down, track progress</div>
        </div>
        <div class="goal-overview">
          <div class="goal-stat">
            <div class="goal-stat-n" id="gTotal" style="color:var(--c0);">0</div>
            <div class="goal-stat-l">Total Goals</div>
          </div>
          <div class="goal-stat">
            <div class="goal-stat-n" id="gDone" style="color:var(--c1);">0</div>
            <div class="goal-stat-l">Completed</div>
          </div>
          <div class="goal-stat">
            <div class="goal-stat-n" id="gActive" style="color:var(--c2);">0</div>
            <div class="goal-stat-l">In Progress</div>
          </div>
        </div>
        <div class="goal-form">
          <div class="goal-form-title">Add New Goal</div>
          <div class="goal-inputs">
            <div>
              <label class="ws-field-lbl">Goal</label>
              <input class="ws-input" id="goalTitle" type="text" placeholder="e.g. Release single by April 30th">
            </div>
            <div>
              <label class="ws-field-lbl">Department</label>
              <select class="ws-select" id="goalDept">
                <option value="human">Human</option>
                <option value="business">Business</option>
                <option value="growth">Growth</option>
                <option value="artist">Artistry</option>
              </select>
            </div>
            <div>
              <label class="ws-field-lbl">Due Date</label>
              <input class="ws-input" id="goalDate" type="date">
            </div>
            <div style="padding-top:18px;">
              <button class="btn btn-primary" onclick="addGoal()">+ Add</button>
            </div>
          </div>
        </div>
        <div class="section-lbl" style="margin-bottom:12px;">Active Goals</div>
        <div class="goal-list" id="goalList"></div>
        <div class="goal-timeline" style="margin-top:28px;">
          <div class="section-lbl" style="margin-bottom:14px;">Year at a Glance</div>
          <div class="timeline-grid" id="timelineGrid"></div>
        </div>
      </div>

      <!-- PROGRESS -->
      <div class="view" id="view-progress">
        <div class="page-hdr">
          <div class="page-title">MY PROGRESS</div>
          <div class="page-sub">Curriculum completion, streaks, and activity</div>
        </div>
        <div class="progress-header">
          <div class="prog-card">
            <div class="prog-num" id="progLessons" style="color:var(--c0);">0</div>
            <div class="prog-lbl">Lessons Done</div>
          </div>
          <div class="prog-card">
            <div class="prog-num" id="progMods" style="color:var(--c1);">0</div>
            <div class="prog-lbl">Modules Done</div>
          </div>
          <div class="prog-card">
            <div class="prog-num" id="progSOPs" style="color:var(--c2);">0</div>
            <div class="prog-lbl">SOPs Run</div>
          </div>
          <div class="prog-card">
            <div class="prog-num" id="progPct" style="color:var(--c3);">0%</div>
            <div class="prog-lbl">Overall</div>
          </div>
        </div>
        <div class="streak-banner">
          <div>
            <div class="streak-n" id="streakNum">0</div>
            <div class="streak-lbl">Day Streak</div>
          </div>
          <div>
            <div class="streak-txt" id="streakTxt">Complete a lesson or run a SOP to start your streak.</div>
          </div>
        </div>
        <div class="section-lbl" style="margin-bottom:14px;">By Department</div>
        <div class="dept-progress-full" id="deptProgFull"></div>
        <div class="section-lbl" style="margin-bottom:14px; margin-top:28px;">Recent Activity</div>
        <div class="recent-list" id="recentList"></div>
      </div>

      <!-- CURRICULUM (FULL MODE) -->
      <div class="view learn-only-block" id="view-learn">
        <div id="learnHome">
          <div class="page-hdr">
            <div class="page-title">CURRICULUM</div>
            <div class="page-sub">Full course — 168 lessons across 5 systems</div>
          </div>
          <div class="learn-grid" id="learnGrid"></div>
        </div>
        <div id="learnModule" style="display:none;">
          <div class="back-nav" onclick="closeLearnModule()">← Back to Curriculum</div>
          <div id="learnModuleContent"></div>
        </div>
        <div id="learnLesson" style="display:none;">
          <div class="back-nav" onclick="closeLearnLesson()">← Back to Module</div>
          <div id="learnLessonContent"></div>
        </div>
      </div>

      <!-- SETTINGS -->
      <div class="view" id="view-settings">
        <div class="page-hdr">
          <div class="page-title">SETTINGS</div>
          <div class="page-sub">Profile, mode, and preferences</div>
        </div>
        <div class="settings-section">
          <div class="settings-title">Account</div>
          <div class="setting-row">
            <div>
              <div class="setting-label" id="settingsName">Artist Name</div>
              <div class="setting-desc" id="settingsEmail"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2b4e464a42476b4e534a465b474e05484446">[email&#160;protected]</a></div>
            </div>
            <!-- Sign Out: restore handleSignOut() here when auth is added -->
            <span style="font-family:'DM Mono',monospace;font-size:9px;color:var(--muted);border:1px solid var(--border);padding:6px 10px;">Auth on deploy</span>
          </div>
        </div>
        <div class="settings-section">
          <div class="settings-title">Interface Mode</div>
          <div class="setting-row">
            <div>
              <div class="setting-label">Full Mode</div>
              <div class="setting-desc">Unlocks Curriculum, advanced tools, and all content sections.</div>
            </div>
            <div class="toggle" id="modeToggle" onclick="toggleMode()"></div>
          </div>
        </div>
        <div class="settings-section">
          <div class="settings-title">Preferences</div>
          <div class="setting-row">
            <div>
              <div class="setting-label">Daily Focus Prompt</div>
              <div class="setting-desc">Show the daily discipline question on the home screen.</div>
            </div>
            <div class="toggle on" id="promptToggle" onclick="togglePref('prompt', this)"></div>
          </div>
          <div class="setting-row">
            <div>
              <div class="setting-label">Department Progress on Home</div>
              <div class="setting-desc">Show the 5-department progress grid on the home screen.</div>
            </div>
            <div class="toggle on" id="deptToggle" onclick="togglePref('dept', this)"></div>
          </div>
        </div>
        <div class="settings-section">
          <div class="settings-title">Data</div>
          <div class="setting-row">
            <div>
              <div class="setting-label">Clear All Progress</div>
              <div class="setting-desc">Resets lesson completions, SOP history, and streaks. Worksheets and goals are kept.</div>
            </div>
            <button class="btn btn-outline" onclick="clearProgress()">Reset</button>
          </div>
          <div class="setting-row">
            <div>
              <div class="setting-label">Export My Data</div>
              <div class="setting-desc">Download your worksheets, goals, and progress as a JSON file.</div>
            </div>
            <button class="btn btn-outline" onclick="exportData()">Export</button>
          </div>
        </div>
      </div>

    </main>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
// ════════════════════════════════════════
// DATA
// ════════════════════════════════════════

const PROMPTS = [
  'What is one system in your career running on vibes instead of structure? Write it down and give it a real process today.',
  'Identify your biggest time leak this week. What consumed energy and produced zero return?',
  'List every revenue stream your music career currently has. Now list every one it could have. What is the gap?',
  'What decision have you been stalling on? What are you actually waiting for — versus what fear are you rationalizing?',
  'Rate your emotional control this week 1-10. What situation tested it most and how did you respond?',
  'Who in your network adds direct value to your career right now? Who is noise? Act accordingly.',
  'What system or process have you been meaning to build but haven\'t started? Begin it today.',
  'How many pieces of content did you produce this week versus how many you planned? What caused the gap?',
  'Name one financial habit costing your career and one you need to build. Start one of them this week.',
  'What does your catalog look like in 3 years at your current output pace? Is that acceptable?',
  'What does being the CEO of yourself actually require this week? List three specific actions.',
  'Where are you spending creative energy on things that do not compound? What would you do instead?',
  'What collaboration should you be building right now that you are not? Why aren\'t you?',
  'Name the actual bottleneck in your career — not a symptom. What breaks it?',
  'What are you doing right now to protect your intellectual property? If the answer is nothing, fix it today.',
];

const DEPT_COLORS = {
  human: 'var(--c1)', business: 'var(--c2)', growth: 'var(--c3)', artist: 'var(--c4)', framework: 'var(--c0)'
};
const DEPT_NAMES = {
  human: 'Human System', business: 'Business System', growth: 'Growth System', artist: 'Artistry System', framework: 'Main Framework'
};

const SOPS = [
  // Human
  {id:'h-schedule',title:'Weekly Scheduling SOP',dept:'human',tags:['time','schedule','week'],steps:[
    {t:'Block your sleep constraints first',n:'Non-negotiable. If you work a day job, block those hours next.'},
    {t:'List all non-work obligations for the week',n:'Family, medical, transport. These are constraints, not options.'},
    {t:'Identify your weekly capacity in creative hours',n:'Be honest. Overcommitting kills momentum faster than laziness.'},
    {t:'Assign top 3 priorities for the week',n:'One per department: Business, Creative, Growth.'},
    {t:'Block M-F schedule in 1-hour increments',n:'Morning blocks are for deep work. Afternoons for admin. Evenings for creative.'},
    {t:'Build weekend schedule separately',n:'Different energy, different tasks. Use the weekend schedule sheet.'},
    {t:'Identify hard limits for the week',n:'What will you refuse to let interrupt the schedule?'},
    {t:'Review against last week\'s plan',n:'What got done, what didn\'t, and why.'},
  ]},
  {id:'h-financial',title:'Financial Literacy Audit SOP',dept:'human',tags:['money','budget','finance'],steps:[
    {t:'List all personal income sources',n:'Job, freelance, royalties, gigs, everything.'},
    {t:'Fill out Bills & Spending Categories sheet',n:'Every dollar should belong to a category.'},
    {t:'Calculate your monthly burn rate',n:'Total obligations minus income. This is your constraint.'},
    {t:'Identify one spending habit to cut immediately',n:'Not forever. For 30 days. See what happens.'},
    {t:'Set a savings target as a fixed percentage',n:'Even 5% is a system. Zero is not.'},
    {t:'Separate business spending from personal',n:'Open a business account if you haven\'t.'},
    {t:'Update the Spending Tracker',n:'Monthly. Every month. Without exception.'},
  ]},
  {id:'h-decision',title:'Decision Making SOP',dept:'human',tags:['decision','thinking','focus'],steps:[
    {t:'Name the decision clearly in one sentence',n:'If you can\'t name it, you can\'t solve it.'},
    {t:'Set a deadline for the decision',n:'Undated decisions don\'t get made.'},
    {t:'List what information you actually need',n:'Separate real missing info from rationalized fear.'},
    {t:'Identify the emotional stake',n:'What are you afraid of? Name it.'},
    {t:'Write out the two most realistic outcomes',n:'Not best case. Not worst case. Most likely.'},
    {t:'Make the call',n:'Imperfect action beats perfect inaction.'},
  ]},
  // Business
  {id:'b-executive-release',title:'Executive Release SOP',dept:'business',tags:['release','rollout','music'],steps:[
    {t:'Confirm mastered audio files are complete',n:'Main, instrumental, and radio edit if applicable.'},
    {t:'Verify split sheet and IP agreement is signed',n:'All contributors must be documented before release.'},
    {t:'Confirm metadata: title, features, ISRC, UPC',n:'Wrong metadata cannot be fixed after distribution goes live.'},
    {t:'Upload to distributor — set release date minimum 7 days out',n:'DistroKid, TuneCore, or CD Baby. Confirm delivery to all platforms.'},
    {t:'Deliver album art — 3000x3000px minimum, RGB, no text near edges',n:'Check platform-specific requirements.'},
    {t:'Build rollout timeline: announcement, content, release day, follow-up',n:'Minimum 2 weeks of planned posts.'},
    {t:'Prepare Drops & Rollout SOP separately',n:'This SOP covers the business side. Rollout SOP covers the public-facing execution.'},
    {t:'Confirm file system is organized: stems, masters, artwork, documents',n:'File System SOP applies here.'},
  ]},
  {id:'b-kpi',title:'KPI Tracking SOP',dept:'business',tags:['kpi','metrics','analytics','tracking'],steps:[
    {t:'Check Audience & Demand indicators',n:'Monthly listeners trending up? Songs getting saved? Fans returning?'},
    {t:'Check Engagement metrics',n:'Comments with sentences, not just emojis. DMs. Shares.'},
    {t:'Check Content Performance',n:'Stream retention after 30 seconds. Video watch time. Skip rate.'},
    {t:'Check Brand & Marketability',n:'Can a stranger describe you in one sentence? Visuals consistent?'},
    {t:'Check Financial Health',n:'Revenue from fans, not just streams. Merch selling. Shows profitable.'},
    {t:'Check Live Performance data',n:'Tickets sold without giveaways. Fans return. Promoters rebook.'},
    {t:'Record results in KPI worksheet',n:'Monthly. Compare to last 30 and 60 days.'},
    {t:'Identify one metric to improve next cycle',n:'One. Not five. One.'},
  ]},
  {id:'b-file-system',title:'File System & Deliverables SOP',dept:'business',tags:['files','storage','backup','deliverables'],steps:[
    {t:'Session minimum — save before closing any session',n:'Stems, session file, rough mix. Minimum. No exceptions.'},
    {t:'Create project folder with standardized naming',n:'ARTISTNAME_SONGNAME_YYYY-MM-DD'},
    {t:'Save final deliverables: stereo master, instrumental, radio edit',n:'Separate folder labeled FINAL_DELIVERABLES'},
    {t:'Save all artwork files — layered and flattened',n:'PSD/AI source + exported PNG/JPG at 3000x3000'},
    {t:'Upload to cloud backup immediately',n:'Google Drive, Dropbox, or iCloud. Not just local storage.'},
    {t:'Add IP documentation: split sheets, contracts, clearances',n:'LEGAL folder inside the project.'},
    {t:'Archive the full project folder — never delete source files',n:'Storage is cheap. Recreating a session is not.'},
  ]},
  {id:'b-legal-ip',title:'Legal & Rights Documentation SOP',dept:'business',tags:['legal','contracts','splits','copyright'],steps:[
    {t:'Complete split sheet for every collaboration',n:'Name, percentage, PRO affiliation, role (writer vs producer).'},
    {t:'Get producer agreement signed before release',n:'Beat license or producer agreement. Verbal agreements are not agreements.'},
    {t:'Register copyright if commercially significant',n:'Copyright.gov. $65. Takes 3 months. Do it anyway.'},
    {t:'Confirm PRO registration (ASCAP or BMI)',n:'You need to be registered to collect performance royalties.'},
    {t:'File with SoundExchange for digital performance royalties',n:'Separate from ASCAP/BMI. Covers streaming and internet radio.'},
    {t:'Document all clearances for samples',n:'No uncleared samples on distributed music. Ever.'},
  ]},
  {id:'b-llc',title:'LLC Formation SOP',dept:'business',tags:['llc','business','legal','entity'],steps:[
    {t:'Choose a business name — check state availability',n:'Secretary of State website. Also check trademark database.'},
    {t:'File Articles of Organization',n:'Your state\'s Secretary of State. Fee varies $50-$500. Do it online.'},
    {t:'Appoint a registered agent',n:'Can be yourself in most states. Must have a physical address.'},
    {t:'Create an Operating Agreement',n:'Even solo. It defines ownership, decision authority, and exit procedures.'},
    {t:'Get an EIN from the IRS',n:'Free at IRS.gov. Takes 5 minutes. Required for bank accounts and hiring.'},
    {t:'Open a dedicated business bank account',n:'Never mix personal and business money. This is the most important step.'},
    {t:'Consult a CPA about tax election',n:'Sole prop vs S-Corp status. This decision has real dollar impact.'},
  ]},
  {id:'b-roi',title:'ROI Tracking SOP',dept:'business',tags:['roi','money','tracking','investment'],steps:[
    {t:'List every music-related expense this month',n:'Recording, mixing, mastering, promotion, gear, software, travel.'},
    {t:'Assign each expense to a category',n:'Music, Promo/Events, Marketing, Merch, Networking, Gear.'},
    {t:'Identify what each expense was supposed to return',n:'More streams? New clients? More shows? Be specific.'},
    {t:'Track what actually returned from each investment',n:'Be honest. No return is data too.'},
    {t:'Calculate cost per result for repeatable activities',n:'Cost per 1000 streams from ads. Cost per show ticket sold.'},
    {t:'Identify what to double down on and what to cut',n:'Data, not feeling.'},
  ]},
  // Growth
  {id:'g-content-pillars',title:'Content Pillars SOP',dept:'growth',tags:['content','social','posting','platform'],steps:[
    {t:'Define your 3-5 content pillars',n:'What categories of content will you consistently make? Music, BTS, education, lifestyle, promo.'},
    {t:'Assign each pillar a percentage of your output',n:'e.g. 40% music/promo, 30% BTS/process, 30% lifestyle/personality.'},
    {t:'Define platform fit for each pillar',n:'Long-form pillars go to YouTube. Short-form to TikTok/Reels. Static to Instagram.'},
    {t:'Build the 9-Box Content Grid',n:'3 pillars x 3 formats = 9 content types you rotate through.'},
    {t:'Set output cadence per platform',n:'Minimum viable: 3x/week Instagram, 4x/week TikTok, 1x/week YouTube.'},
    {t:'Batch content 2 weeks in advance',n:'Film/record multiple pieces in one session. Never be scrambling.'},
    {t:'Review performance by pillar each month',n:'Which content type is growing the audience vs just getting likes?'},
  ]},
  {id:'g-rollout',title:'Drops & Rollout SOP',dept:'growth',tags:['release','rollout','marketing','drop'],steps:[
    {t:'Define the core story and concept of the release',n:'One sentence. Everything in the rollout should connect to this.'},
    {t:'Build the rollout calendar — minimum 3 weeks',n:'Week 1: tease. Week 2: announce. Week 3: release + push.'},
    {t:'Plan teaser content: snippets, behind-the-scenes, countdowns',n:'No reveals until the announcement date.'},
    {t:'Prepare announcement post — all platforms on the same day',n:'Cover art, release date, pre-save link.'},
    {t:'Line up playlist pitching and press outreach',n:'SubmitHub, DistroKid pre-release playlisting, local press contacts.'},
    {t:'Plan release day content: multiple posts, stories, live if possible',n:'Stay active the entire release day. Algorithm rewards it.'},
    {t:'Execute follow-up week content',n:'Reactions, fan posts, performance clips, lyric breakdowns.'},
    {t:'Review analytics at 7 days and 30 days post-release',n:'Save the KPI data. Compare to previous releases.'},
  ]},
  {id:'g-collab',title:'Collaboration SOP',dept:'growth',tags:['collab','feature','artist','network'],steps:[
    {t:'Define the purpose of this collab',n:'Audience expansion? Creative challenge? Relationship building? Be clear.'},
    {t:'Vet the artist — quality of work, professionalism, audience fit',n:'A bad collab can hurt your brand. Check their catalog and how they operate.'},
    {t:'Send a professional outreach DM or email',n:'Short, specific, complimentary but not desperate. Know their work.'},
    {t:'Align on creative direction before entering the studio',n:'Concept, roles, production source, timeline.'},
    {t:'Sign a collaboration agreement or at minimum document splits via text/email',n:'Who owns what. What can be released. Timeline.'},
    {t:'Execute the session — come prepared',n:'Have your verses, ideas, reference tracks. Don\'t waste their time.'},
    {t:'Agree on rollout coordination',n:'Who announces first. When. How you cross-post.'},
  ]},
  {id:'g-paid-organic',title:'Paid vs Organic Decision SOP',dept:'growth',tags:['ads','organic','paid','promotion'],steps:[
    {t:'Define the opportunity: what are you trying to promote?',n:'Be specific. A single, a show, a merch item, your profile.'},
    {t:'Assess your current organic reach',n:'If organic isn\'t working, paid will not fix a content problem.'},
    {t:'Define your budget and acceptable cost-per-result',n:'$50 test budget minimum. Know what result you\'re buying.'},
    {t:'Choose the platform — Meta Ads, TikTok Spark Ads, or YouTube Ads',n:'Match platform to content type and audience location.'},
    {t:'Run a small test first — never start with your full budget',n:'$10-20 for 3 days. Analyze before scaling.'},
    {t:'Assess: who controls more in this deal?',n:'If the platform gets more value than you, reconsider.'},
    {t:'Decide: scale, pause, or cut',n:'Based on data from the test. Not instinct.'},
  ]},
  {id:'g-industry-rels',title:'Industry Relationships SOP',dept:'growth',tags:['network','industry','relationships','contacts'],steps:[
    {t:'Define your target relationship categories',n:'DJs, promoters, media contacts, other artists, producers, engineers, brands.'},
    {t:'Set weekly outreach targets by category',n:'Consistency beats intensity. 3-5 genuine touches per week.'},
    {t:'Lead with value, not a request',n:'Share their content. Comment thoughtfully. Show up to their events.'},
    {t:'Follow up systematically',n:'If they respond, respond within 24 hours. Don\'t let conversations go cold.'},
    {t:'Document key contacts in a running list',n:'Name, platform, relationship status, last contact, notes.'},
    {t:'Convert relationships into tangible opportunities',n:'Features, shows, press, referrals. This is the goal.'},
  ]},
  // Artist
  {id:'a-artists-release',title:'Artist Release Approval SOP',dept:'artist',tags:['release','quality','standard'],steps:[
    {t:'Verify audio quality — no clipping, no phase issues',n:'Listen on multiple playback systems before submitting.'},
    {t:'Check that the release feels current — not dated or rushed',n:'Compare to recent releases in the same genre.'},
    {t:'Confirm hook is strong — repeatable and memorable',n:'If you can\'t remember it 10 minutes after hearing it, it\'s not there yet.'},
    {t:'Verify artist has a plan for the release, not just the music',n:'No plan, no release date. The rollout is part of the product.'},
    {t:'Confirm all credits and splits are documented',n:'No release without paperwork.'},
    {t:'Get a second opinion from a trusted ear',n:'Someone who will tell you the truth, not what you want to hear.'},
  ]},
  {id:'a-producers',title:'Producer Approval SOP',dept:'artist',tags:['producer','beat','instrumental'],steps:[
    {t:'Verify audio quality of the instrumental',n:'No clipping, no low-quality samples, production-ready mix.'},
    {t:'Confirm genre and tempo alignment with the project',n:'Does this beat fit the artist\'s direction?'},
    {t:'Check arrangement for clean intro and clear sections',n:'Hook must be identifiable. Verse/chorus transitions must be clear.'},
    {t:'Confirm producer agreement terms',n:'Flat fee, royalty split, exclusive vs non-exclusive. Get it in writing.'},
    {t:'Verify producer credits are documented',n:'Name, percentage, PRO info.'},
    {t:'Confirm delivery format: WAV stems, tagged MP3, BPM and key labeled',n:'Stems required for professional mixing.'},
  ]},
  {id:'a-studio-session',title:'Studio Session SOP',dept:'artist',tags:['studio','recording','session'],steps:[
    {t:'Create a new session — label with artist, song, date',n:'ARTISTNAME_SONGNAME_YYYY-MM-DD'},
    {t:'Set sample rate 44.1kHz / 24-bit minimum',n:'48kHz if delivering for video.'},
    {t:'Import beat or produce scratch track before artist arrives',n:'No wasted time in session.'},
    {t:'Record scratch vocal pass first — before setup changes',n:'Capture energy before perfectionism sets in.'},
    {t:'Record minimum 3 takes of each section',n:'Best take rarely comes first.'},
    {t:'Comp the vocal before the artist leaves',n:'Rough comp is better than starting cold next session.'},
    {t:'Save session — stems, session file, comp\'d vocal — before session ends',n:'File System SOP applies here.'},
    {t:'Send rough mix to artist within 24 hours',n:'Keep momentum alive.'},
  ]},
  {id:'a-music-video',title:'Music Video Storyboard SOP',dept:'artist',tags:['video','visual','storyboard','shoot'],steps:[
    {t:'Define the core idea and vibe in one sentence',n:'Concept must be clear before any other decisions are made.'},
    {t:'Identify 3-5 non-negotiable visual elements',n:'Colors, locations, wardrobe direction, mood.'},
    {t:'Map song structure to visual moments',n:'What happens on the hook? What changes in the bridge?'},
    {t:'Scout and confirm locations',n:'Permission, lighting time of day, backup location.'},
    {t:'Build production budget',n:'Camera, crew, locations, wardrobe, editing. Be realistic.'},
    {t:'Create shot list — minimum 2 angles per scene',n:'No shot list means wasted time on set.'},
    {t:'Execute shoot — get every shot on the list plus 20% extra coverage',n:'More footage is always better in the edit.'},
    {t:'Deliver edit timeline: rough cut within 3 days, final within 2 weeks',n:'Momentum matters.'},
  ]},
  {id:'a-events',title:'Live Performance & Events SOP',dept:'artist',tags:['show','performance','live','event'],steps:[
    {t:'Confirm event details: date, venue, time, load-in, soundcheck',n:'Get it in writing. Verbal agreements are not agreements.'},
    {t:'Confirm payment terms before agreeing',n:'Deposit required if you don\'t know the promoter. Protect your time.'},
    {t:'Prepare set list — timed to your slot with 2 minutes buffer',n:'Know your set cold. No reading lyrics on stage.'},
    {t:'Prepare a show file: backing tracks, stems, DJ cue points labeled',n:'Label everything by song title and order.'},
    {t:'Confirm sound system specs: PA, monitor, DI or aux send needed',n:'Know what you\'re walking into.'},
    {t:'Arrive at load-in time — not showtime',n:'Soundcheck is non-negotiable.'},
    {t:'Post-show: collect payment, connect with people who approached you, document for KPIs',n:'Shows are also networking events.'},
  ]},
  {id:'a-song-world',title:'Song World Building SOP',dept:'artist',tags:['concept','branding','rollout','story'],steps:[
    {t:'Define the world of this song in 2-3 sentences',n:'Setting, emotion, perspective. Where does this song live?'},
    {t:'Identify who this song is for — be specific',n:'Not everyone. Who is the person that needed to hear this?'},
    {t:'Build visual direction: colors, mood, wardrobe reference',n:'Before shooting anything, have a visual brief.'},
    {t:'Write the 1-sheet: song story, context, talking points',n:'Used for press, pitching, and your own rollout content.'},
    {t:'Map content opportunities: behind the scenes, reaction, meaning video',n:'One song can generate 5-10 pieces of content.'},
  ]},
  {id:'a-skill-dev',title:'Skill Development SOP',dept:'artist',tags:['skills','writing','performance','beatmaking','engineering'],steps:[
    {t:'Identify the specific skill to develop this month',n:'Writing, performance, beatmaking, engineering, or taste. Pick one.'},
    {t:'Set a measurable target',n:'Write 10 hooks by end of month. Mix 5 records. Perform 3 sets.'},
    {t:'Study the craft — find 3 reference artists to analyze',n:'Taste Refinement: what are they doing that you aren\'t?'},
    {t:'Dedicate minimum 30 minutes daily to deliberate practice',n:'Not working — practicing. The goal is getting better, not output.'},
    {t:'Get feedback from someone better than you',n:'Not your friends. Someone who will challenge your assumptions.'},
    {t:'Evaluate at the end of the month',n:'Did you hit the target? What improved? What next?'},
  ]},
  {id:'a-goals-long',title:'Long-Term Goals SOP',dept:'artist',tags:['goals','planning','vision','future'],steps:[
    {t:'Write your 3-year vision in one paragraph',n:'Be specific. Not just famous. Revenue number, catalog size, team size.'},
    {t:'Break it into 1-year milestones',n:'What needs to be true by year 1 for year 3 to be possible?'},
    {t:'Break year 1 into quarterly targets',n:'Four 90-day windows. Each one should have a clear output.'},
    {t:'Identify the 3 biggest obstacles to the 3-year vision',n:'These become the focus of your decision-making systems.'},
    {t:'Assign resources: time, money, people to each major goal',n:'Unresourced goals are wishes.'},
    {t:'Review quarterly — adjust as needed',n:'Goals are not permanent. Targets should evolve with reality.'},
  ]},
  {id:'b-quarterly',title:'Quarterly Alignment Meeting SOP',dept:'business',tags:['review','planning','quarterly','team'],steps:[
    {t:'Review goals vs results from the past 90 days',n:'What did you commit to? What happened? No excuses.'},
    {t:'Run KPI audit across all 8 indicators',n:'Audience, engagement, content, brand, financial, live, producer/engineer, momentum.'},
    {t:'Identify root causes of missed targets',n:'Not symptoms. Causes.'},
    {t:'Assess team and relationships — who is adding value?',n:'Producers, engineers, collaborators, management. Who should you invest more in?'},
    {t:'Run risk check — identify 3 vulnerabilities',n:'Legal exposure? Single-platform dependency? Cash flow gap?'},
    {t:'Set goals and deadlines for the next 90 days',n:'Specific, dated, resourced.'},
    {t:'Document the meeting — decisions, action items, owners',n:'Undocumented meetings don\'t produce accountability.'},
  ]},
  {id:'b-brand',title:'Brand Positioning SOP',dept:'business',tags:['brand','identity','positioning','market'],steps:[
    {t:'Write your brand statement in one sentence',n:'Who you are, who you serve, what makes you different.'},
    {t:'Identify your 3-5 core visual non-negotiables',n:'Colors, fonts, photography style, wardrobe direction. Not vibes — rules.'},
    {t:'Audit all platforms for visual consistency',n:'Instagram, YouTube, Spotify, website. Same energy everywhere.'},
    {t:'Define your cultural positioning',n:'What scene, movement, or conversation does your music belong to?'},
    {t:'Identify your top 3 competitor/reference artists',n:'Not to copy. To understand what space you occupy relative to them.'},
    {t:'Define what your audience gets from you that they can\'t get elsewhere',n:'This is your positioning statement.'},
  ]},
];

const WORKSHEETS = [
  {id:'ws-spending-personal',title:'Personal Spending Tracker',dept:'human',type:'tracker',
   fields:[
     {id:'month',label:'Month',type:'select',opts:['January','February','March','April','May','June','July','August','September','October','November','December']},
     {id:'income',label:'Total Income / Paycheck Amount ($)',type:'text',placeholder:'e.g. 2400'},
     {id:'house',label:'Housing & Utilities ($)',type:'text',placeholder:'Rent, electric, internet...'},
     {id:'food',label:'Food ($)',type:'text',placeholder:'Groceries, restaurants...'},
     {id:'transport',label:'Transportation ($)',type:'text',placeholder:'Gas, insurance, Uber...'},
     {id:'family',label:'Family ($)',type:'text',placeholder:'Child support, family obligations...'},
     {id:'debt',label:'Debt Payments ($)',type:'text',placeholder:'Credit cards, loans...'},
     {id:'wellness',label:'Wellness ($)',type:'text',placeholder:'Gym, medical, therapy...'},
     {id:'savings',label:'Savings / Investments ($)',type:'text',placeholder:'Target amount to save'},
     {id:'misc',label:'Miscellaneous ($)',type:'text',placeholder:'Everything else'},
     {id:'notes',label:'Notes / Observations',type:'textarea',placeholder:'Anything unusual this month? What to change next month?'},
   ]},
  {id:'ws-spending-business',title:'Business Spending Tracker',dept:'business',type:'tracker',
   fields:[
     {id:'month',label:'Month',type:'select',opts:['January','February','March','April','May','June','July','August','September','October','November','December']},
     {id:'music',label:'Music / Recording ($)',type:'text',placeholder:'Studio time, features, session fees'},
     {id:'promo',label:'Promo / Events ($)',type:'text',placeholder:'Flyers, event costs, ticket fees'},
     {id:'marketing',label:'Marketing ($)',type:'text',placeholder:'Ads, content creation, PR'},
     {id:'merch',label:'Merch ($)',type:'text',placeholder:'Printing, inventory, shipping'},
     {id:'networking',label:'Networking ($)',type:'text',placeholder:'Events, subscriptions, travel for biz'},
     {id:'gear',label:'Gear / Purchases ($)',type:'text',placeholder:'Equipment, software, plugins'},
     {id:'cashbuffer',label:'Cash Buffer / Reserve ($)',type:'text',placeholder:'Emergency fund for business'},
     {id:'notes',label:'ROI Notes',type:'textarea',placeholder:'What returned value this month? What was wasted?'},
   ]},
  {id:'ws-schedule-mf',title:'M-F Weekly Schedule',dept:'human',type:'schedule',
   fields:[
     {id:'capacity',label:'Weekly Creative Capacity (hours)',type:'text',placeholder:'How many hours can you realistically work on music this week?'},
     {id:'constraints',label:'Sleep / Health / Work Constraints',type:'textarea',placeholder:'Day job hours, medical, sleep requirements...'},
     {id:'hard_limits',label:'Hard Limits This Week',type:'textarea',placeholder:'What will you refuse to let interrupt the plan?'},
     {id:'obligations',label:'Non-Work Obligations',type:'textarea',placeholder:'Family, appointments, recurring commitments...'},
     {id:'priorities',label:'Top 3 Priorities This Week',type:'textarea',placeholder:'One per department: Business, Creative, Growth'},
     {id:'todo',label:'To Do List',type:'textarea',placeholder:'Everything that needs to happen this week'},
     {id:'schedule',label:'M-F Schedule (7am-10pm)',type:'textarea',placeholder:'7:00 — \n8:00 — \n9:00 — \n10:00 — \n11:00 — \n12:00 — \n13:00 — \n14:00 — \n15:00 — \n16:00 — \n17:00 — \n18:00 — \n19:00 — \n20:00 — \n21:00 — \n22:00 — '},
   ]},
  {id:'ws-schedule-weekend',title:'Weekend Schedule',dept:'human',type:'schedule',
   fields:[
     {id:'priorities',label:'Weekend Top Priorities',type:'textarea',placeholder:'What 3 things matter most this weekend?'},
     {id:'schedule',label:'Fri-Sun Schedule',type:'textarea',placeholder:'7:00 — \n8:00 — \n9:00 — \n10:00 — \n11:00 — \n12:00 — \n13:00 — \n14:00 — \n15:00 — \n16:00 — \n17:00 — \n18:00 — \n19:00 — \n20:00 — \n21:00 — \n22:00 — '},
     {id:'monday_prep',label:'Monday Prep Notes',type:'textarea',placeholder:'What do you need to be ready for next week?'},
   ]},
  {id:'ws-financial-literacy',title:'Financial Literacy System',dept:'human',type:'system',
   fields:[
     {id:'problem',label:'Financial shortcoming to address',type:'textarea',placeholder:'Be specific: What is the habit, gap, or pattern that is costing you?'},
     {id:'hack',label:'System / Hack to overcome it',type:'textarea',placeholder:'Concrete: an app, a rule, a process, a trigger. Not motivation — a system.'},
     {id:'deadline',label:'When will you implement this?',type:'text',placeholder:'Give it a date.'},
     {id:'accountability',label:'How will you hold yourself accountable?',type:'textarea',placeholder:'Weekly check-in? Monthly review? Tell someone?'},
   ]},
  {id:'ws-emotional-regulation',title:'Emotional Regulation System',dept:'human',type:'system',
   fields:[
     {id:'trigger',label:'Pressure situation that derails you',type:'textarea',placeholder:'What type of situation most reliably causes you to react instead of respond?'},
     {id:'pattern',label:'Your current reaction pattern',type:'textarea',placeholder:'What do you typically do? Post something? Go quiet? Make a bad decision?'},
     {id:'hack',label:'System / Hack to regulate instead',type:'textarea',placeholder:'A pause, a rule, a process. Make it specific and repeatable.'},
     {id:'practice',label:'How will you practice this?',type:'textarea',placeholder:'Where in your week can you deliberately practice emotional control?'},
   ]},
  {id:'ws-decision-speed',title:'Decision Making Speed System',dept:'human',type:'system',
   fields:[
     {id:'slow_decision',label:'A decision you have been slow to make',type:'textarea',placeholder:'Name it.'},
     {id:'real_blocker',label:'What is the actual blocker?',type:'textarea',placeholder:'Missing information? Fear? Waiting on someone else? Be honest.'},
     {id:'deadline',label:'Decision deadline',type:'text',placeholder:'Give it a date. Undated decisions don\'t get made.'},
     {id:'system',label:'Your decision-making process',type:'textarea',placeholder:'How will you make decisions faster in general? What is your framework?'},
   ]},
  {id:'ws-roi',title:'ROI Tracking Sheet',dept:'business',type:'tracker',
   fields:[
     {id:'period',label:'Period (Month / Quarter)',type:'text',placeholder:'e.g. Q1 2026'},
     {id:'investments',label:'Investments Made (list each)',type:'textarea',placeholder:'Category | Amount Spent | Expected Return | Actual Return\n\nRecording — $XXX — streams/fans — \nPromo — $XXX — plays/reach — \nEvents — $XXX — ticket revenue — '},
     {id:'best_roi',label:'Highest ROI activity this period',type:'textarea',placeholder:'What gave you the most return per dollar?'},
     {id:'worst_roi',label:'Lowest ROI activity this period',type:'textarea',placeholder:'What was the biggest waste?'},
     {id:'next_period',label:'Adjustments for next period',type:'textarea',placeholder:'What will you do more of? What are you cutting?'},
   ]},
  {id:'ws-goal-sheet',title:'Goal Setting Sheet',dept:'business',type:'planning',
   fields:[
     {id:'main_goal',label:'Main Goal',type:'textarea',placeholder:'Write your primary goal for this period in one clear sentence.'},
     {id:'goal1',label:'Goal 1',type:'text',placeholder:'Specific, measurable, dated.'},
     {id:'steps1',label:'Action Steps for Goal 1',type:'textarea',placeholder:'What are the concrete steps to achieve this?'},
     {id:'goal2',label:'Goal 2',type:'text',placeholder:'Specific, measurable, dated.'},
     {id:'steps2',label:'Action Steps for Goal 2',type:'textarea',placeholder:'What are the concrete steps to achieve this?'},
     {id:'goal3',label:'Goal 3',type:'text',placeholder:'Specific, measurable, dated.'},
     {id:'steps3',label:'Action Steps for Goal 3',type:'textarea',placeholder:'What are the concrete steps to achieve this?'},
     {id:'goal4',label:'Goal 4',type:'text',placeholder:'Specific, measurable, dated.'},
     {id:'steps4',label:'Action Steps for Goal 4',type:'textarea',placeholder:'What are the concrete steps to achieve this?'},
   ]},
  {id:'ws-kpi',title:'KPI Checklist',dept:'business',type:'checklist',
   fields:[
     {id:'period',label:'Review Period',type:'text',placeholder:'e.g. March 2026'},
     {id:'audience',label:'Audience & Demand — Notes',type:'textarea',placeholder:'Monthly listeners trending? Songs being saved? Fans returning?'},
     {id:'engagement',label:'Engagement — Notes',type:'textarea',placeholder:'Real comments? Shares? DMs?'},
     {id:'content',label:'Content Performance — Notes',type:'textarea',placeholder:'Retention rate? Skip rate? Watch time?'},
     {id:'brand',label:'Brand & Marketability — Notes',type:'textarea',placeholder:'Can a stranger describe you? Visual consistency?'},
     {id:'financial',label:'Financial Health — Notes',type:'textarea',placeholder:'Revenue from fans? Merch selling? Shows profitable?'},
     {id:'live',label:'Live Performance — Notes',type:'textarea',placeholder:'Tickets sold? Promoters rebook? Fans return?'},
     {id:'takeaway',label:'One Metric to Improve Next Cycle',type:'textarea',placeholder:'Pick one. Not five.'},
   ]},
  {id:'ws-release-calendar',title:'Release Calendar',dept:'artist',type:'planning',
   fields:[
     {id:'year',label:'Year',type:'text',placeholder:'e.g. 2026'},
     {id:'q1',label:'Q1 (Jan-Mar) — Planned Drops',type:'textarea',placeholder:'Song title — Target date — Status'},
     {id:'q2',label:'Q2 (Apr-Jun) — Planned Drops',type:'textarea',placeholder:'Song title — Target date — Status'},
     {id:'q3',label:'Q3 (Jul-Sep) — Planned Drops',type:'textarea',placeholder:'Song title — Target date — Status'},
     {id:'q4',label:'Q4 (Oct-Dec) — Planned Drops',type:'textarea',placeholder:'Song title — Target date — Status'},
     {id:'events',label:'Planned Events / Shows',type:'textarea',placeholder:'Event name — Date — City — Status'},
     {id:'notes',label:'Notes',type:'textarea',placeholder:'Constraints, dependencies, contingencies...'},
   ]},
  {id:'ws-deliverables',title:'Music Deliverables Checklist',dept:'artist',type:'checklist',
   fields:[
     {id:'project',label:'Project Name',type:'text',placeholder:'Song or project title'},
     {id:'audio',label:'Audio Files — Status',type:'textarea',placeholder:'Mastered stereo WAV — \nInstrumental WAV — \nRadio edit (if applicable) — \nStems — '},
     {id:'artwork',label:'Artwork — Status',type:'textarea',placeholder:'3000x3000 JPG/PNG — \nLayered source file — \nBanner/header versions — '},
     {id:'metadata',label:'Metadata — Status',type:'textarea',placeholder:'Song title, featured artists — \nISRC — \nUPC — \nBPM and key — \nLyrics — '},
     {id:'legal',label:'Legal — Status',type:'textarea',placeholder:'Split sheet signed — \nProducer agreement — \nSample clearances — '},
     {id:'rollout',label:'Rollout — Status',type:'textarea',placeholder:'Release date confirmed — \nRollout calendar built — \nContent planned — \nPitching submitted — '},
   ]},
  {id:'ws-video-storyboard',title:'Music Video Storyboard',dept:'artist',type:'creative',
   fields:[
     {id:'song',label:'Song Title',type:'text',placeholder:''},
     {id:'concept',label:'Core Idea / Vibe (1 sentence)',type:'textarea',placeholder:'What is this video about? What should the viewer feel?'},
     {id:'visuals',label:'3-5 Non-Negotiable Visual Elements',type:'textarea',placeholder:'Colors, locations, wardrobe, props, mood words...'},
     {id:'structure',label:'Visual Structure (map to song)',type:'textarea',placeholder:'Intro — \nVerse 1 — \nHook — \nVerse 2 — \nHook — \nBridge — \nOutro — '},
     {id:'locations',label:'Location(s)',type:'textarea',placeholder:'Primary location — \nBackup location — \nPermission needed? — '},
     {id:'budget',label:'Production Budget',type:'textarea',placeholder:'Camera/crew — $\nLocations — $\nWardrobe — $\nEditing — $\nTotal — $'},
     {id:'timeline',label:'Production Timeline',type:'textarea',placeholder:'Shoot date — \nRough cut by — \nFinal cut by — \nRelease date — '},
   ]},
  {id:'ws-aesthetic-rules',title:'Aesthetic Rules (Visual Identity)',dept:'artist',type:'system',
   fields:[
     {id:'adjectives',label:'3-5 Adjectives That Must Always Describe Your Visual Image',type:'textarea',placeholder:'e.g. Raw, cinematic, dark, textured, intentional'},
     {id:'colors',label:'Primary Color Palette',type:'textarea',placeholder:'List your brand colors — hex codes if possible'},
     {id:'fonts',label:'Typography Direction',type:'textarea',placeholder:'Font families or style direction for graphics and content'},
     {id:'photography',label:'Photography / Visual Style',type:'textarea',placeholder:'Lighting style, framing, location types, mood...'},
     {id:'never',label:'What Your Visuals Should NEVER Look Like',type:'textarea',placeholder:'What aesthetic is off-brand for you?'},
     {id:'references',label:'Reference Artists / Visual Inspirations',type:'textarea',placeholder:'Artists or brands whose visual approach inspires you'},
   ]},
  {id:'ws-content-pillars',title:'Content Pillars Sheet',dept:'growth',type:'planning',
   fields:[
     {id:'pillar1',label:'Content Pillar 1 — Name & Description',type:'textarea',placeholder:'e.g. Music & Promo — Behind the music, clips, announcements'},
     {id:'pct1',label:'Pillar 1 — % of Content Output',type:'text',placeholder:'e.g. 40%'},
     {id:'pillar2',label:'Content Pillar 2 — Name & Description',type:'textarea',placeholder:'e.g. Process & BTS — Studio sessions, creative process, day in the life'},
     {id:'pct2',label:'Pillar 2 — % of Content Output',type:'text',placeholder:'e.g. 30%'},
     {id:'pillar3',label:'Content Pillar 3 — Name & Description',type:'textarea',placeholder:'e.g. Personality / Lifestyle — Who you are off stage'},
     {id:'pct3',label:'Pillar 3 — % of Content Output',type:'text',placeholder:'e.g. 30%'},
     {id:'cadence',label:'Output Cadence Per Platform',type:'textarea',placeholder:'Instagram — \nTikTok — \nYouTube — \nOther — '},
     {id:'batch',label:'Batch Schedule',type:'textarea',placeholder:'When will you batch content each week/month?'},
   ]},
];

// Search index — combine SOPs and worksheets
const SEARCH_INDEX = [
  ...SOPS.map(s => ({id:s.id, title:s.title, dept:s.dept, type:'SOP', tags:s.tags})),
  ...WORKSHEETS.map(w => ({id:w.id, title:w.title, dept:w.dept, type:'Worksheet', tags:[w.type, w.dept, ...w.title.toLowerCase().split(' ')]})),
];

// ════════════════════════════════════════
// STATE
// ════════════════════════════════════════
const STATE = {
  user: null,
  fullMode: false,
  completed: {},
  sopProgress: {},
  wsData: {},
  goals: [],
  recentActivity: [],
  prefs: { prompt: true, dept: true },
};

function loadState() {
  const raw = localStorage.getItem('rc_state');
  if (raw) {
    try { Object.assign(STATE, JSON.parse(raw)); } catch(e) {}
  }
}

function saveStateData() {
  const toSave = {
    user: STATE.user,
    fullMode: STATE.fullMode,
    completed: STATE.completed,
    sopProgress: STATE.sopProgress,
    wsData: STATE.wsData,
    goals: STATE.goals,
    recentActivity: STATE.recentActivity.slice(0,20),
    prefs: STATE.prefs,
  };
  localStorage.setItem('rc_state', JSON.stringify(toSave));
}

function logActivity(label, color) {
  STATE.recentActivity.unshift({ label, color, time: Date.now() });
  if (STATE.recentActivity.length > 20) STATE.recentActivity = STATE.recentActivity.slice(0, 20);
  saveStateData();
}

// ════════════════════════════════════════
// AUTH STUBS — restore these on deployment
// ════════════════════════════════════════
// TO ADD AUTH LATER:
//   1. Remove the direct init() call at the bottom of this section
//   2. Restore a login gate (Google OAuth, email, etc.)
//   3. Call init() only after successful auth
//   4. Restore handleSignOut() to clear session + redirect

function handleGoogleAuth() {
  // TODO: wire to Google OAuth on deployment
  // window.location.href = '/auth/google';
}

function handleSignOut() {
  // TODO: clear server session on deployment
  // window.location.href = '/auth/logout';
  STATE.user = { name: 'Artist', avatar: 'A' };
  saveStateData();
}

// ════════════════════════════════════════
// BOOT — no gate, direct entry
// ════════════════════════════════════════
function init() {
  // State already loaded before this call

  // Set a default user if none exists (no auth required)
  if (!STATE.user) {
    STATE.user = { name: 'Artist', avatar: 'A' };
  }

  // Populate UI with user info
  const name = STATE.user.name || 'Artist';
  const avatar = STATE.user.avatar || name[0].toUpperCase();
  document.getElementById('userName').textContent = name;
  document.getElementById('userAvatar').textContent = avatar;
  document.getElementById('settingsName').textContent = name;
  document.getElementById('settingsEmail').textContent = '— Profile available after auth is enabled —';

  // Apply saved mode preference
  if (STATE.fullMode) {
    document.body.classList.add('full-mode');
    document.getElementById('modeBadge').textContent = 'Full Mode';
    document.getElementById('modeBadge').classList.add('full');
    document.getElementById('modeToggle').classList.add('on');
  }

  // Render all sections
  renderHome();
  renderSOPGrid('all');
  renderWSGrid('all');
  renderGoals();
  renderProgress();
  renderLearnGrid();
  buildTimeline();
}

// ════════════════════════════════════════
// VIEW ROUTING
// ════════════════════════════════════════
function showView(id) {
  document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
  const v = document.getElementById('view-' + id);
  if (v) v.classList.add('active');
  const n = document.getElementById('nav-' + id);
  if (n) n.classList.add('active');
  if (id === 'home') renderHome();
  if (id === 'progress') renderProgress();
  if (id === 'goals') renderGoals();
}

// ════════════════════════════════════════
// HOME
// ════════════════════════════════════════
function renderHome() {
  const hour = new Date().getHours();
  const greeting = hour < 12 ? 'GOOD MORNING' : hour < 17 ? 'WELCOME BACK' : 'BACK AT IT';
  document.getElementById('homeGreeting').textContent = greeting + ', ' + (STATE.user?.name || 'ARTIST').toUpperCase();
  document.getElementById('homeDate').textContent = new Date().toLocaleDateString('en-US', {weekday:'long', month:'long', day:'numeric'});

  if (STATE.prefs.prompt) {
    document.getElementById('dailyPrompt').textContent = PROMPTS[new Date().getDate() % PROMPTS.length];
  }

  const sopsDone = Object.values(STATE.sopProgress).filter(p => p.done).length;
  const wsDone = Object.keys(STATE.wsData).length;
  const goalsDone = STATE.goals.length;
  const lessDone = Object.values(STATE.completed).filter(Boolean).length;
  const pct = Math.round((lessDone / 168) * 100);

  document.getElementById('homeStat1').textContent = sopsDone;
  document.getElementById('homeStat2').textContent = wsDone;
  document.getElementById('homeStat3').textContent = goalsDone;
  document.getElementById('homeStat4').textContent = pct + '%';

  // dept progress
  const deptTotals = [42, 42, 42, 42, 42]; // ~equal split (168 total / 5)
  // simplified: count by system id prefix
  [0,1,2,3,4].forEach(i => {
    const prefix = i + '.';
    const done = Object.keys(STATE.completed).filter(k => k.startsWith(prefix) && STATE.completed[k]).length;
    const total = 42;
    const pct = Math.round((done/total)*100);
    document.getElementById('dp'+i).textContent = pct+'%';
    document.getElementById('df'+i).style.width = pct+'%';
    document.getElementById('dc'+i).textContent = done+'/'+total;
  });
}

// ════════════════════════════════════════
// SEARCH
// ════════════════════════════════════════
function handleSearch(val) {
  const q = val.trim().toLowerCase();
  const res = document.getElementById('searchResults');
  if (!q) { res.classList.remove('open'); return; }
  const matches = SEARCH_INDEX.filter(item =>
    item.title.toLowerCase().includes(q) ||
    item.tags.some(t => t.includes(q)) ||
    item.dept.includes(q)
  ).slice(0, 8);
  if (!matches.length) { res.classList.remove('open'); return; }
  res.innerHTML = matches.map(m => `
    <div class="sr-item" onclick="openFromSearch('${m.type}','${m.id}')">
      <div class="sr-tag">${m.type}</div>
      <div class="sr-title">${m.title}</div>
      <div class="sr-dept" style="color:${DEPT_COLORS[m.dept]||'var(--cn)'};">${DEPT_NAMES[m.dept]||''}</div>
    </div>
  `).join('');
  res.classList.add('open');
}

function openSearch() {
  if (document.getElementById('searchInput').value.trim()) {
    document.getElementById('searchResults').classList.add('open');
  }
}

function closeSearch() {
  document.getElementById('searchResults').classList.remove('open');
}

function openFromSearch(type, id) {
  closeSearch();
  document.getElementById('searchInput').value = '';
  if (type === 'SOP') {
    showView('sops');
    openSOP(id);
  } else {
    showView('worksheets');
    openWS(id);
  }
}

function searchFor(keyword) {
  showView('search');
  document.getElementById('searchViewInput').value = keyword;
  renderSearchView(keyword);
}

function renderSearchView(q) {
  const el = document.getElementById('searchViewResults');
  if (!q.trim()) { el.innerHTML = '<div style="font-family:DM Mono,monospace;font-size:10px;color:var(--muted);">Type to search SOPs and worksheets</div>'; return; }
  const matches = SEARCH_INDEX.filter(item =>
    item.title.toLowerCase().includes(q.toLowerCase()) ||
    item.tags.some(t => t.includes(q.toLowerCase())) ||
    item.dept.includes(q.toLowerCase())
  );
  if (!matches.length) {
    el.innerHTML = '<div style="font-family:DM Mono,monospace;font-size:10px;color:var(--muted);">No results for "'+q+'"</div>';
    return;
  }
  el.innerHTML = `<div style="font-family:DM Mono,monospace;font-size:9px;color:var(--muted);margin-bottom:12px;letter-spacing:1px;text-transform:uppercase;">${matches.length} result${matches.length!==1?'s':''}</div>` +
    matches.map(m => `
    <div class="sop-row" onclick="openFromSearch('${m.type}','${m.id}')">
      <div class="sop-dept-pip" style="background:${DEPT_COLORS[m.dept]||'var(--cn)'};"></div>
      <div class="sop-title">${m.title}</div>
      <div class="sop-dept-tag" style="color:${DEPT_COLORS[m.dept]||'var(--cn)'};">${m.dept}</div>
      <div class="sop-type">${m.type}</div>
      <div class="sop-arrow">→</div>
    </div>`).join('');
}

// ════════════════════════════════════════
// SOP LIBRARY
// ════════════════════════════════════════
function renderSOPGrid(filter) {
  const grid = document.getElementById('sopGrid');
  const filtered = filter === 'all' ? SOPS : SOPS.filter(s => s.dept === filter);
  grid.innerHTML = filtered.map(s => {
    const c = DEPT_COLORS[s.dept] || 'var(--cn)';
    const prog = STATE.sopProgress[s.id];
    const done = prog?.done;
    return `<div class="sop-row" onclick="openSOP('${s.id}')">
      <div class="sop-dept-pip" style="background:${c};"></div>
      <div class="sop-title">${s.title}</div>
      <div class="sop-dept-tag" style="color:${c};">${s.dept}</div>
      <div class="sop-type">${done ? '✓ Done' : s.steps.length+' steps'}</div>
      <div class="sop-arrow">→</div>
    </div>`;
  }).join('');
}

function filterSOPs(dept) {
  showView('sops');
  document.getElementById('sopViewer').classList.remove('active');
  document.getElementById('sopLibrary').style.display = '';
  document.querySelectorAll('#sopFilters .filter-btn').forEach(b => b.classList.remove('active'));
  const btns = document.querySelectorAll('#sopFilters .filter-btn');
  const idx = ['all','human','business','growth','artist'].indexOf(dept);
  if (btns[idx]) btns[idx].classList.add('active');
  renderSOPGrid(dept);
}

function openSOP(id) {
  const sop = SOPS.find(s => s.id === id);
  if (!sop) return;
  const c = DEPT_COLORS[sop.dept] || 'var(--cn)';
  if (!STATE.sopProgress[id]) STATE.sopProgress[id] = { steps: [], done: false };
  const prog = STATE.sopProgress[id];

  document.getElementById('sopLibrary').style.display = 'none';
  const viewer = document.getElementById('sopViewer');
  viewer.classList.add('active');

  const done = prog.steps.filter(Boolean).length;
  const pct = Math.round((done / sop.steps.length) * 100);

  document.getElementById('sopViewerContent').innerHTML = `
    <div style="margin-bottom:6px;">
      <span class="tag" style="color:${c};border-color:${c};">${sop.dept}</span>
    </div>
    <h1 style="font-family:'Bebas Neue',sans-serif;font-size:36px;letter-spacing:3px;margin-bottom:16px;color:${c};">${sop.title}</h1>
    <div class="sop-progress-bar"><div class="sop-progress-fill" id="sopProgBar" style="background:${c};width:${pct}%;"></div></div>
    <div class="sop-progress-txt" id="sopProgTxt">${done} of ${sop.steps.length} steps complete${prog.done?' — SOP Complete':''}</div>
    <div class="sop-step-list">
      ${sop.steps.map((step, i) => {
        const chk = prog.steps[i] || false;
        return `<div class="sop-step ${chk?'done':''}" onclick="toggleSOPStep('${id}',${i})">
          <div class="sop-step-num" style="color:${c};">${String(i+1).padStart(2,'0')}</div>
          <div class="sop-step-body">
            <div class="sop-step-title">${step.t}</div>
            ${step.n ? `<div class="sop-step-note">${step.n}</div>` : ''}
          </div>
          <div class="sop-step-chk ${chk?'done':''}" style="${chk?`background:${c};border-color:${c};`:''}">
            ${chk ? '✓' : ''}
          </div>
        </div>`;
      }).join('')}
    </div>
    <div style="margin-top:20px;display:flex;gap:10px;">
      <button class="btn btn-primary" style="border-color:${c};color:${c};" onclick="completeSOP('${id}')">Mark SOP Complete</button>
      <button class="btn btn-outline" onclick="resetSOP('${id}')">Reset</button>
    </div>`;
}

function toggleSOPStep(id, idx) {
  if (!STATE.sopProgress[id]) STATE.sopProgress[id] = { steps: [], done: false };
  const prog = STATE.sopProgress[id];
  if (!prog.steps) prog.steps = [];
  prog.steps[idx] = !prog.steps[idx];
  saveStateData();
  openSOP(id); // re-render
}

function completeSOP(id) {
  if (!STATE.sopProgress[id]) STATE.sopProgress[id] = { steps: [], done: false };
  STATE.sopProgress[id].done = true;
  const sop = SOPS.find(s => s.id === id);
  logActivity('Completed SOP: ' + sop.title, DEPT_COLORS[sop.dept]);
  saveStateData();
  showToast('SOP marked complete');
  openSOP(id);
}

function resetSOP(id) {
  STATE.sopProgress[id] = { steps: [], done: false };
  saveStateData();
  openSOP(id);
}

function closeSopViewer() {
  document.getElementById('sopViewer').classList.remove('active');
  document.getElementById('sopLibrary').style.display = '';
}

// ════════════════════════════════════════
// WORKSHEETS
// ════════════════════════════════════════
function renderWSGrid(filter) {
  const grid = document.getElementById('wsGrid');
  const filtered = filter === 'all' ? WORKSHEETS : WORKSHEETS.filter(w => w.dept === filter);
  grid.innerHTML = filtered.map(w => {
    const c = DEPT_COLORS[w.dept] || 'var(--cn)';
    const hasData = !!STATE.wsData[w.id];
    return `<div class="ws-card" onclick="openWS('${w.id}')">
      <div class="ws-card-title">${w.title}</div>
      <div class="ws-card-meta">
        <div class="ws-card-dept" style="color:${c};">${w.dept}</div>
        <div class="ws-card-type">${w.type}</div>
      </div>
      <div class="ws-card-status">
        <div class="ws-dot ${hasData?'started':''}"></div>
        ${hasData ? 'In progress' : 'Not started'}
      </div>
    </div>`;
  }).join('');
}

function filterWS(dept) {
  document.getElementById('wsViewer').style.display = 'none';
  document.getElementById('wsLibrary').style.display = '';
  document.querySelectorAll('#wsTabs .ws-tab').forEach(b => b.classList.remove('active'));
  const tabs = document.querySelectorAll('#wsTabs .ws-tab');
  const idx = ['all','human','business','growth','artist'].indexOf(dept);
  if (tabs[idx]) tabs[idx].classList.add('active');
  renderWSGrid(dept);
}

function openWS(id) {
  const ws = WORKSHEETS.find(w => w.id === id);
  if (!ws) return;
  const c = DEPT_COLORS[ws.dept] || 'var(--cn)';
  const saved = STATE.wsData[id] || {};

  document.getElementById('wsLibrary').style.display = 'none';
  document.getElementById('wsViewer').style.display = '';

  document.getElementById('wsViewerContent').innerHTML = `
    <div style="margin-bottom:6px;">
      <span class="tag" style="color:${c};border-color:${c};">${ws.dept} — ${ws.type}</span>
    </div>
    <h1 style="font-family:'Bebas Neue',sans-serif;font-size:36px;letter-spacing:3px;margin-bottom:24px;color:${c};">${ws.title}</h1>
    <div class="ws-form">
      ${ws.fields.map(f => `
        <div class="ws-field-group">
          <label class="ws-field-lbl" for="wsf_${f.id}">${f.label}</label>
          ${f.type === 'textarea' ?
            `<textarea class="ws-input" id="wsf_${f.id}" rows="4" placeholder="${f.placeholder||''}">${saved[f.id]||''}</textarea>` :
          f.type === 'select' ?
            `<select class="ws-select" id="wsf_${f.id}">
              ${(f.opts||[]).map(o => `<option ${saved[f.id]===o?'selected':''}>${o}</option>`).join('')}
            </select>` :
            `<input class="ws-input" id="wsf_${f.id}" type="text" placeholder="${f.placeholder||''}" value="${saved[f.id]||''}">`
          }
        </div>`).join('')}
      <div class="ws-save-row">
        <button class="btn btn-primary" style="border-color:${c};color:${c};" onclick="saveWS('${id}')">Save</button>
        <button class="btn btn-outline" onclick="clearWS('${id}')">Clear</button>
        <span class="ws-save-msg" id="wsSaveMsg"></span>
      </div>
    </div>`;
}

function saveWS(id) {
  const ws = WORKSHEETS.find(w => w.id === id);
  if (!ws) return;
  const data = {};
  ws.fields.forEach(f => {
    const el = document.getElementById('wsf_' + f.id);
    if (el) data[f.id] = el.value;
  });
  STATE.wsData[id] = data;
  logActivity('Updated worksheet: ' + ws.title, DEPT_COLORS[ws.dept]);
  saveStateData();
  document.getElementById('wsSaveMsg').textContent = 'Saved';
  setTimeout(() => { const m = document.getElementById('wsSaveMsg'); if(m) m.textContent=''; }, 2000);
  showToast('Worksheet saved');
}

function clearWS(id) {
  if (!confirm('Clear all data in this worksheet?')) return;
  delete STATE.wsData[id];
  saveStateData();
  openWS(id);
}

function closeWsViewer() {
  document.getElementById('wsViewer').style.display = 'none';
  document.getElementById('wsLibrary').style.display = '';
}

// ════════════════════════════════════════
// GOAL TRACKER
// ════════════════════════════════════════
function renderGoals() {
  const total = STATE.goals.length;
  const done = STATE.goals.filter(g => g.done).length;
  const active = total - done;
  document.getElementById('gTotal').textContent = total;
  document.getElementById('gDone').textContent = done;
  document.getElementById('gActive').textContent = active;

  const list = document.getElementById('goalList');
  if (!STATE.goals.length) {
    list.innerHTML = '<div style="font-family:DM Mono,monospace;font-size:10px;color:var(--muted);padding:16px;">No goals yet. Add your first goal above.</div>';
    return;
  }
  list.innerHTML = STATE.goals.map((g, i) => {
    const c = DEPT_COLORS[g.dept] || 'var(--cn)';
    const dueStr = g.date ? new Date(g.date).toLocaleDateString('en-US',{month:'short',day:'numeric',year:'numeric'}) : 'No date';
    return `<div class="goal-item ${g.done?'done':''}">
      <div class="goal-chk" onclick="toggleGoal(${i})" style="${g.done?`background:${c};border-color:${c};color:#000;`:'border-color:'+c}">
        ${g.done ? '✓' : ''}
      </div>
      <div class="goal-body">
        <div class="goal-title-txt">${g.title}</div>
        <div class="goal-meta">
          <span class="goal-meta-item" style="color:${c};">${g.dept}</span>
          <span class="goal-meta-item">Due: ${dueStr}</span>
        </div>
      </div>
      <div class="goal-actions">
        <button class="goal-del" onclick="deleteGoal(${i})">✕</button>
      </div>
    </div>`;
  }).join('');

  buildTimeline();
}

function addGoal() {
  const title = document.getElementById('goalTitle').value.trim();
  const dept = document.getElementById('goalDept').value;
  const date = document.getElementById('goalDate').value;
  if (!title) { showToast('Enter a goal first'); return; }
  STATE.goals.push({ title, dept, date, done: false, created: Date.now() });
  document.getElementById('goalTitle').value = '';
  document.getElementById('goalDate').value = '';
  logActivity('Added goal: ' + title, DEPT_COLORS[dept]);
  saveStateData();
  renderGoals();
  showToast('Goal added');
}

function toggleGoal(i) {
  STATE.goals[i].done = !STATE.goals[i].done;
  if (STATE.goals[i].done) logActivity('Completed goal: ' + STATE.goals[i].title, DEPT_COLORS[STATE.goals[i].dept]);
  saveStateData();
  renderGoals();
}

function deleteGoal(i) {
  STATE.goals.splice(i, 1);
  saveStateData();
  renderGoals();
}

function buildTimeline() {
  const months = ['JAN','FEB','MAR','APR','MAY','JUN','JUL','AUG','SEP','OCT','NOV','DEC'];
  const grid = document.getElementById('timelineGrid');
  if (!grid) return;
  grid.innerHTML = months.map((m, mi) => {
    const count = STATE.goals.filter(g => {
      if (!g.date) return false;
      const d = new Date(g.date);
      return d.getMonth() === mi;
    }).length;
    return `<div class="month-cell ${count>0?'has-goal':''}">
      <div class="month-name">${m}</div>
      <div class="month-count" style="color:${count>0?'var(--c1)':'var(--muted)'};">${count||'—'}</div>
    </div>`;
  }).join('');
}

// ════════════════════════════════════════
// PROGRESS
// ════════════════════════════════════════
function renderProgress() {
  const lesDone = Object.values(STATE.completed).filter(Boolean).length;
  let modDone = 0;
  // count modules: a module has 7 lessons. 24 modules total
  for (let s=0; s<5; s++) {
    for (let m=1; m<=6; m++) {
      const modId = s + '.' + m;
      const allDone = [1,2,3,4,5,6,7].every(l => STATE.completed[`${modId}.${l}`]);
      if (allDone) modDone++;
    }
  }
  const sopsDone = Object.values(STATE.sopProgress).filter(p => p.done).length;
  const pct = Math.round((lesDone/168)*100);

  document.getElementById('progLessons').textContent = lesDone;
  document.getElementById('progMods').textContent = modDone;
  document.getElementById('progSOPs').textContent = sopsDone;
  document.getElementById('progPct').textContent = pct+'%';

  // streak
  const streak = calcStreak();
  document.getElementById('streakNum').textContent = streak;
  document.getElementById('streakTxt').textContent = streak > 0
    ? `${streak} consecutive days with activity. Keep it going.`
    : 'Complete a lesson or run a SOP to start your streak.';

  // dept breakdown
  const names = ['Main Framework','Human System','Business System','Growth System','Artistry System'];
  const colors = ['var(--c0)','var(--c1)','var(--c2)','var(--c3)','var(--c4)'];
  const deptEl = document.getElementById('deptProgFull');
  deptEl.innerHTML = [0,1,2,3,4].map(i => {
    const done = Object.keys(STATE.completed).filter(k => k.startsWith(i+'.') && STATE.completed[k]).length;
    const total = 42;
    const p = Math.round((done/total)*100);
    return `<div class="dept-prog-row">
      <div class="dept-prog-hdr">
        <div class="dept-prog-name" style="color:${colors[i]};">${names[i]}</div>
        <div class="dept-prog-pct" style="color:${colors[i]};">${done}/${total} — ${p}%</div>
      </div>
      <div class="dept-prog-bar"><div class="dept-prog-fill" style="background:${colors[i]};width:${p}%;"></div></div>
    </div>`;
  }).join('');

  // recent
  const recEl = document.getElementById('recentList');
  if (!STATE.recentActivity.length) {
    recEl.innerHTML = '<div style="font-family:DM Mono,monospace;font-size:10px;color:var(--muted);padding:12px;">No recent activity yet.</div>';
    return;
  }
  recEl.innerHTML = STATE.recentActivity.slice(0,10).map(a => {
    const t = new Date(a.time);
    const timeStr = t.toLocaleDateString('en-US',{month:'short',day:'numeric'}) + ' ' + t.toLocaleTimeString('en-US',{hour:'2-digit',minute:'2-digit'});
    return `<div class="recent-item">
      <div class="recent-pip" style="background:${a.color||'var(--c0)'}"></div>
      ${a.label}
      <div class="recent-time">${timeStr}</div>
    </div>`;
  }).join('');
}

function calcStreak() {
  const dates = STATE.recentActivity.map(a => new Date(a.time).toDateString());
  const unique = [...new Set(dates)];
  let streak = 0;
  const today = new Date();
  for (let i=0; i<unique.length; i++) {
    const d = new Date(today);
    d.setDate(d.getDate() - i);
    if (unique.includes(d.toDateString())) streak++;
    else break;
  }
  return streak;
}

// ════════════════════════════════════════
// CURRICULUM (learn mode)
// ════════════════════════════════════════
const CURRICULUM = [
  {id:'0',label:'0.0',title:'MAIN FRAMEWORK',ci:0,desc:'The operating model behind every independent music career.',
   modules:[
    {id:'0.1',title:'Career Foundations',desc:'Why most artists fail and how to build foundational thinking.',lessons:['Why Most Independent Artists Fail','The 4 Department Model','Hobby vs Career Thinking','Systems vs Motivation','The Independent Artist Flywheel','The Discipline Loop','Career Infrastructure']},
    {id:'0.2',title:'Bottleneck Diagnosis',desc:'Identify what is actually blocking your career.',lessons:['Identifying Artist Bottlenecks','Creative Identity vs Business Identity','Chaos vs Structure','Why Talent Alone Doesn\'t Win','The Compounding Effect','Short-Term vs Long-Term Moves','Career Sustainability']},
    {id:'0.3',title:'Systems Thinking',desc:'Build systems, not routines.',lessons:['Creative Systems','Business Systems','Marketing Systems','Personal Systems','System Failure Examples','System Alignment','Systems That Scale']},
    {id:'0.4',title:'Execution Systems',desc:'Translate strategy into output.',lessons:['Execution vs Ideas','Release Infrastructure','Creative Momentum','Audience Compounding','Strategic Output','Career Velocity','Long-Term Positioning']},
    {id:'0.5',title:'Scaling Systems',desc:'Build infrastructure that grows.',lessons:['Building a Team','Delegation','Operational Standards','Protecting Intellectual Property','Financial Flywheel','Scaling Audience','Scaling Output']},
    {id:'0.6',title:'Legacy Systems',desc:'Build something that outlasts you.',lessons:['Artist Longevity','Catalog Value','Cultural Positioning','Ownership vs Fame','Artist Institutions','Creative Legacy','Building Something That Outlasts You']},
   ]},
  {id:'1',label:'1.0',title:'HUMAN SYSTEM',ci:1,desc:'Personal stability, time management, financial literacy, and emotional discipline.',
   modules:[
    {id:'1.1',title:'Personal Stability',desc:'Your inner state determines your output.',lessons:['Why Personal Chaos Kills Careers','Artist Discipline vs Motivation','The Energy Economy','Sleep, Health, and Creative Output','Artist Burnout','Focus vs Distraction','Personal Responsibility']},
    {id:'1.2',title:'Time Systems',desc:'Time is your only non-renewable resource.',lessons:['Time = Energy Framework','Weekly Capacity Planning','Artist Scheduling Systems','Creative Time Blocks','Work vs Art Balance','Weekend Scheduling','Time Leak Diagnosis']},
    {id:'1.3',title:'Financial Literacy',desc:'Artists go broke not because they earn too little.',lessons:['Artist Money Reality','Personal Expense Categories','Budget Systems for Artists','Financial Stress Management','Spending Discipline','Savings Systems','Investing in Your Career']},
    {id:'1.4',title:'Emotional Discipline',desc:'Emotional reactions kill careers.',lessons:['Emotional Control Under Pressure','Handling Criticism','Social Media Psychology','Managing Ego','Artist Comparison Trap','Creative Resilience','Staying Consistent']},
    {id:'1.5',title:'Decision Systems',desc:'Every bad career decision was an emotional one.',lessons:['Decision Speed Framework','Avoiding Overthinking','Strategic vs Emotional Decisions','Risk Assessment','Choosing Collaborators','Saying No','Career Priority Decisions']},
    {id:'1.6',title:'Personal Mastery',desc:'Become the CEO of yourself.',lessons:['Identity vs Image','Artist Self-Awareness','Lifestyle Alignment','Mental Toughness','Creative Longevity','Personal Integrity','Becoming the CEO of Yourself']},
   ]},
  {id:'2',label:'2.0',title:'BUSINESS SYSTEM',ci:2,desc:'Legal infrastructure, financial systems, and operations.',
   modules:[
    {id:'2.1',title:'Business Foundations',desc:'Define your business identity.',lessons:['Hobby vs Business','The Artist as CEO','Building Career Infrastructure','Mission Vision Values','Brand Philosophy','Business Systems','Long-Term Career Strategy']},
    {id:'2.2',title:'Legal Infrastructure',desc:'Protect your work before it has value.',lessons:['Why Artists Need Contracts','Split Sheets Explained','Producer Agreements','Vocalist Releases','Sample Clearance','Distribution Agreements','Copyright Ownership']},
    {id:'2.3',title:'Business Structure',desc:'Set up the legal shell that protects your creative assets.',lessons:['LLC for Artists','Asset Protection','Business Bank Accounts','Tax Basics','Self-Employment Taxes','EIN and Professional Identity','Advanced Structures']},
    {id:'2.4',title:'Financial Systems',desc:'Track money in, track money out.',lessons:['Business Expenses','ROI Tracking','Music Investment Strategy','Revenue Streams','Show Profitability','Merch Margins','Budgeting Campaigns']},
    {id:'2.5',title:'Operations Systems',desc:'How you handle files is a business decision.',lessons:['File Systems','Deliverables Protocol','Metadata Management','Studio Documentation','Cloud Backup Systems','Vendor Relationships','Release Readiness']},
    {id:'2.6',title:'Performance Metrics',desc:'If you are not tracking it, you are guessing.',lessons:['KPI Tracking','Audience Metrics','Engagement Metrics','Content Performance','Financial Health','Momentum Indicators','Reality Check Framework']},
   ]},
  {id:'3',label:'3.0',title:'GROWTH SYSTEM',ci:3,desc:'Audience building, content strategy, marketing, and networking.',
   modules:[
    {id:'3.1',title:'Audience Foundations',desc:'Understand what a real fanbase is.',lessons:['What a Real Fanbase Is','The Attention Economy','Platform Selection','Audience Geography','Niche Positioning','Cultural Relevance','Audience Psychology']},
    {id:'3.2',title:'Content Systems',desc:'Build a content operation that runs consistently.',lessons:['Content Pillars','Platform Fit','Short-Form Strategy','Long-Form Strategy','Content Batching','Content Repurposing','Content Consistency']},
    {id:'3.3',title:'Social Growth',desc:'Platform-specific strategy.',lessons:['Instagram Strategy','TikTok Strategy','YouTube Strategy','Streaming Platforms','Community Building','Comment Strategy','Fan Interaction']},
    {id:'3.4',title:'Marketing Systems',desc:'Release marketing and promotion done right.',lessons:['Release Marketing','Paid vs Organic Promotion','Press Outreach','Blog Placements','Playlist Strategy','Collaborations','Influencer Strategy']},
    {id:'3.5',title:'Networking Systems',desc:'Build industry relationships that create leverage.',lessons:['Industry Relationships','Artist Collaborations','Producer Networks','DJ Relationships','Media Contacts','Event Promoters','Brand Partnerships']},
    {id:'3.6',title:'Momentum Systems',desc:'Convert attention to fans. Convert fans to customers.',lessons:['Converting Attention to Fans','Converting Fans to Customers','Fan Retention','Fan Community','Cultural Positioning','Scaling Visibility','Long-Term Growth']},
   ]},
  {id:'4',label:'4.0',title:'ARTISTRY SYSTEM',ci:4,desc:'Creative identity, song creation, production, visuals, and catalog.',
   modules:[
    {id:'4.1',title:'Creative Identity',desc:'Know who you are artistically.',lessons:['Artistic Vision','Cultural Study','Creative Influence','Genre Positioning','Artist Identity','Message and Themes','Creative Direction']},
    {id:'4.2',title:'Song Creation',desc:'Songwriting systems and structure.',lessons:['Songwriting Systems','Beat Selection','Vocal Performance','Recording Quality','Arrangement','Hook Writing','Song Structure']},
    {id:'4.3',title:'Production Systems',desc:'Build a studio workflow that produces consistently.',lessons:['Beatmaking','Mixing Basics','Mastering Basics','Studio Workflow','DAW Templates','Recording Efficiency','Production Standards']},
    {id:'4.4',title:'Visual Identity',desc:'Your visuals communicate before your music does.',lessons:['Aesthetic Rules','Brand Visuals','Album Art','Music Video Concepts','Storyboarding','Visual Consistency','Fashion and Style']},
    {id:'4.5',title:'Release Execution',desc:'Execute the release, not just the record.',lessons:['Release Calendar','Project Planning','Singles vs Albums','Music Video Strategy','Rollout Campaigns','Creative Deadlines','Content Around Releases']},
    {id:'4.6',title:'Catalog Strategy',desc:'Build a catalog that compounds over time.',lessons:['Building a Catalog','Artistic Evolution','Quality Control','Creative Legacy','Cultural Impact','Signature Sound','Timeless Art']},
   ]},
];

const LESSON_INDEX = [];
CURRICULUM.forEach(sys => sys.modules.forEach(mod => mod.lessons.forEach((les,li) => {
  LESSON_INDEX.push({sysId:sys.id,sysLabel:sys.label,sysTitle:sys.title,ci:sys.ci,modId:mod.id,modTitle:mod.title,lessonId:`${mod.id}.${li+1}`,lessonTitle:les,index:LESSON_INDEX.length});
})));

const COLS = ['var(--c0)','var(--c1)','var(--c2)','var(--c3)','var(--c4)'];

function renderLearnGrid() {
  const grid = document.getElementById('learnGrid');
  if (!grid) return;
  grid.innerHTML = CURRICULUM.map(sys => {
    let done=0; const total=42;
    sys.modules.forEach(mod => mod.lessons.forEach((_,li) => { if(STATE.completed[`${mod.id}.${li+1}`]) done++; }));
    const pct = Math.round((done/total)*100);
    const c = COLS[sys.ci];
    return `<div class="learn-card" onclick="openLearnSys('${sys.id}')" style="box-shadow:inset 0 2px 0 ${c};">
      <div class="learn-bg-num" style="color:${c};">${sys.label}</div>
      <div class="learn-lbl" style="color:${c};">${sys.label}</div>
      <div class="learn-title" style="color:${c};">${sys.title}</div>
      <div class="learn-desc">${sys.desc}</div>
      <div class="learn-bar"><div class="learn-fill" style="background:${c};width:${pct}%;"></div></div>
      <div class="learn-count">${done}/${total} lessons — ${pct}%</div>
    </div>`;
  }).join('');
}

let _curSys=null, _curMod=null, _curLes=null, _lesCache={};

function openLearnSys(sysId) {
  _curSys = CURRICULUM.find(s => s.id===sysId);
  const c = COLS[_curSys.ci];
  document.getElementById('learnHome').style.display='none';
  document.getElementById('learnModule').style.display='block';
  document.getElementById('learnLesson').style.display='none';
  document.getElementById('learnModuleContent').innerHTML = `
    <span class="tag" style="color:${c};border-color:${c};">${_curSys.label} ${_curSys.title}</span>
    <h1 style="font-family:'Bebas Neue',sans-serif;font-size:40px;letter-spacing:3px;margin:10px 0 24px;color:${c};">${_curSys.title}</h1>
    <div class="module-list">
      ${_curSys.modules.map(mod => {
        const done = mod.lessons.filter((_,li) => STATE.completed[`${mod.id}.${li+1}`]).length;
        return `<div class="module-row" onclick="openLearnMod('${_curSys.id}','${mod.id}')">
          <div class="mod-id" style="color:${c};">${mod.id}</div>
          <div class="mod-name">${mod.title}</div>
          <div class="mod-prog">${done}/${mod.lessons.length}</div>
        </div>`;
      }).join('')}
    </div>`;
}

function openLearnMod(sysId, modId) {
  _curSys = CURRICULUM.find(s => s.id===sysId);
  _curMod = _curSys.modules.find(m => m.id===modId);
  const c = COLS[_curSys.ci];
  document.getElementById('learnHome').style.display='none';
  document.getElementById('learnModule').style.display='block';
  document.getElementById('learnLesson').style.display='none';
  document.getElementById('learnModuleContent').innerHTML = `
    <div class="back-nav" onclick="openLearnSys('${sysId}')">← Back</div>
    <span class="tag" style="color:${c};border-color:${c};">${_curMod.id} — ${_curMod.title}</span>
    <h1 style="font-family:'Bebas Neue',sans-serif;font-size:36px;letter-spacing:3px;margin:10px 0 8px;color:${c};">${_curMod.title}</h1>
    <p style="color:var(--muted);font-size:12px;margin-bottom:24px;">${_curMod.desc}</p>
    <div class="lesson-list">
      ${_curMod.lessons.map((les,li) => {
        const lid=`${modId}.${li+1}`;
        const done=STATE.completed[lid];
        return `<div class="lesson-row" onclick="openLearnLesson('${lid}')">
          <div class="les-id" style="color:${c};">${lid}</div>
          <div class="les-name">${les}</div>
          <div class="les-status"><div class="les-dot" style="${done?`background:${c};`:''}"></div>${done?'Done':'Open'}</div>
        </div>`;
      }).join('')}
    </div>`;
}

async function openLearnLesson(lesId) {
  const entry = LESSON_INDEX.find(l => l.lessonId===lesId);
  if(!entry) return;
  _curLes = entry;
  const c = COLS[entry.ci];
  document.getElementById('learnHome').style.display='none';
  document.getElementById('learnModule').style.display='none';
  document.getElementById('learnLesson').style.display='block';

  const done = STATE.completed[lesId];
  document.getElementById('learnLessonContent').innerHTML = `
    <span class="tag" style="color:${c};border-color:${c};">${entry.sysLabel} ${entry.sysTitle}</span>
    <h1 style="font-family:'Bebas Neue',sans-serif;font-size:36px;letter-spacing:3px;margin:10px 0 4px;color:${c};">${entry.lessonTitle}</h1>
    <div style="font-family:'DM Mono',monospace;font-size:9px;color:var(--muted);margin-bottom:24px;">${entry.lessonId}</div>
    <div class="lesson-view-content">
      <div>
        <div class="content-box">
          <div class="box-lbl" style="color:${c};">Lesson</div>
          <div class="les-txt" id="lesContent"><div class="skeleton-stack"><div class="skel"></div><div class="skel m"></div><div class="skel"></div><div class="skel s"></div><div class="skel m"></div></div></div>
        </div>
        <div class="content-box" id="takeBox" style="display:none;">
          <div class="box-lbl" style="color:${c};">Key Takeaways</div>
          <div id="takeList"></div>
        </div>
      </div>
      <div>
        <div class="content-box" id="chkBox" style="display:none;">
          <div class="box-lbl" style="color:${c};">Action Items</div>
          <div id="chkList"></div>
        </div>
        <button class="complete-btn" id="compBtn" onclick="markLessonDone('${lesId}')" style="border-color:${c};color:${c};${done?`background:${c};color:#000;`:''}'">${done?'✓ Completed':'Mark as Complete'}</button>
        <div class="nav-btns">
          <button class="nav-btn" onclick="navLesson(-1)" ${entry.index===0?'style=visibility:hidden;':''}>← Prev</button>
          <button class="nav-btn" onclick="navLesson(1)" ${entry.index===LESSON_INDEX.length-1?'style=visibility:hidden;':''}>Next →</button>
        </div>
      </div>
    </div>`;

  await loadLessonContent(entry);
}

async function loadLessonContent(entry) {
  const lid = entry.lessonId;
  if (_lesCache[lid]) { renderLessonContent(_lesCache[lid], entry); return; }
  try {
    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method:'POST', headers:{'Content-Type':'application/json'},
      body: JSON.stringify({
        model:'claude-sonnet-4-20250514', max_tokens:1000,
        messages:[{role:'user',content:`You are an expert music industry educator for RAPCLASS. Direct, no-filler, practical. No motivational fluff.\n\nLesson ${lid}: ${entry.lessonTitle}\nSystem: ${entry.sysTitle} | Module: ${entry.modTitle}\n\nRespond ONLY with valid JSON, no markdown:\n{"content":"3-4 paragraphs. Direct, practical, for independent artists. Each paragraph separated by \\n.","takeaways":["takeaway 1","takeaway 2","takeaway 3","takeaway 4"],"checklist":["action 1","action 2","action 3"]}`}]
      })
    });
    const data = await res.json();
    const parsed = JSON.parse((data.content||[]).map(b=>b.text||'').join('').replace(/```json|```/g,'').trim());
    _lesCache[lid] = parsed;
    renderLessonContent(parsed, entry);
  } catch(e) {
    const el = document.getElementById('lesContent');
    if(el) el.innerHTML = '<div style="color:#e05050;font-family:DM Mono,monospace;font-size:11px;">Failed to load lesson content.</div>';
  }
}

function renderLessonContent(data, entry) {
  const c = COLS[entry.ci];
  const el = document.getElementById('lesContent');
  if(el) el.innerHTML = data.content.split('\n').filter(p=>p.trim()).map(p=>`<p>${p}</p>`).join('');
  const tb = document.getElementById('takeList');
  if(tb) { tb.innerHTML = data.takeaways.map(t=>`<div class="takeaway-row"><span class="t-arr" style="color:${c};">→</span><span>${t}</span></div>`).join(''); document.getElementById('takeBox').style.display='block'; }
  const lid = entry.lessonId;
  const saved = _curLes?._checklists?.[lid] || [];
  const cb = document.getElementById('chkList');
  if(cb) {
    cb.innerHTML = data.checklist.map((item,i) => {
      const chk = saved[i]||false;
      return `<div class="chk-item" onclick="toggleLesChk(${i})"><div class="chkbox" id="lchk${i}" style="${chk?`background:${c};border-color:${c};color:#000;`:''}">${chk?'✓':''}</div><div class="chktxt ${chk?'done':''}" id="lct${i}">${item}</div></div>`;
    }).join('');
    document.getElementById('chkBox').style.display='block';
  }
}

function toggleLesChk(i) {
  if(!_curLes) return;
  if(!_curLes._checklists) _curLes._checklists={};
  const lid=_curLes.lessonId;
  if(!_curLes._checklists[lid]) _curLes._checklists[lid]=[];
  _curLes._checklists[lid][i]=!(_curLes._checklists[lid][i]);
  const chk=_curLes._checklists[lid][i];
  const c=COLS[_curLes.ci];
  const cb=document.getElementById('lchk'+i); if(cb){cb.style.cssText=chk?`background:${c};border-color:${c};color:#000;`:'';cb.textContent=chk?'✓':'';}
  const ct=document.getElementById('lct'+i); if(ct) ct.className='chktxt'+(chk?' done':'');
}

function markLessonDone(lid) {
  STATE.completed[lid]=true;
  const entry=LESSON_INDEX.find(l=>l.lessonId===lid);
  if(entry) logActivity('Completed lesson: '+entry.lessonTitle, COLS[entry.ci]);
  saveStateData();
  const btn=document.getElementById('compBtn');
  if(btn){btn.textContent='✓ Completed';btn.classList.add('done');btn.style.background=COLS[_curLes?.ci||0];}
  showToast('Lesson complete');
  renderLearnGrid();
}

function navLesson(dir) {
  if(!_curLes) return;
  const n=LESSON_INDEX[_curLes.index+dir];
  if(n) openLearnLesson(n.lessonId);
}

function closeLearnModule() {
  document.getElementById('learnHome').style.display='';
  document.getElementById('learnModule').style.display='none';
}
function closeLearnLesson() {
  document.getElementById('learnModule').style.display='block';
  document.getElementById('learnLesson').style.display='none';
  if(_curMod && _curSys) openLearnMod(_curSys.id, _curMod.id);
}

// ════════════════════════════════════════
// SETTINGS
// ════════════════════════════════════════
function toggleMode() {
  STATE.fullMode = !STATE.fullMode;
  const badge = document.getElementById('modeBadge');
  const toggle = document.getElementById('modeToggle');
  if (STATE.fullMode) {
    document.body.classList.add('full-mode');
    badge.textContent = 'Full Mode'; badge.classList.add('full');
    toggle.classList.add('on');
    showToast('Full mode enabled — Curriculum unlocked');
  } else {
    document.body.classList.remove('full-mode');
    badge.textContent = 'Stripped Mode'; badge.classList.remove('full');
    toggle.classList.remove('on');
    showToast('Stripped mode — essentials only');
  }
  saveStateData();
}

function togglePref(key, el) {
  STATE.prefs[key] = !STATE.prefs[key];
  el.classList.toggle('on', STATE.prefs[key]);
  saveStateData();
}

function clearProgress() {
  if (!confirm('Clear all lesson completions, SOP history, and streaks? Worksheets and goals are preserved.')) return;
  STATE.completed = {}; STATE.sopProgress = {}; STATE.recentActivity = [];
  saveStateData(); showToast('Progress cleared'); renderHome();
}

function exportData() {
  const data = JSON.stringify({ completed: STATE.completed, goals: STATE.goals, wsData: STATE.wsData, exportDate: new Date().toISOString() }, null, 2);
  const blob = new Blob([data], {type:'application/json'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a'); a.href=url; a.download='rapclass_data.json'; a.click();
  URL.revokeObjectURL(url);
  showToast('Data exported');
}

// ════════════════════════════════════════
// TOAST
// ════════════════════════════════════════
let _toastTimer;
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg; t.classList.add('show');
  clearTimeout(_toastTimer);
  _toastTimer = setTimeout(() => t.classList.remove('show'), 2400);
}

// ════════════════════════════════════════
// INIT
// ════════════════════════════════════════
// Set today's date

// ════════════════════════════════════════
// BOOT — runs after all functions defined
// ════════════════════════════════════════
loadState();
init();

</script>
</body>
</html>
