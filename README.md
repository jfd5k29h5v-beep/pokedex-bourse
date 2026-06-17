[pokemon_tracker_iphone.html](https://github.com/user-attachments/files/29059716/pokemon_tracker_iphone.html)
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="theme-color" content="#0d0d14">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Pokédex Bourse">
<title>Pokédex Bourse — Gaspard</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Mono:ital,wght@0,300;0,400;0,500;1,300&display=swap" rel="stylesheet">
<style>
:root {
  --ink: #0d0d14;
  --paper: #13131f;
  --lift: #1c1c2e;
  --rim: #2a2a42;
  --fog: #44446a;
  --dim: #7777aa;
  --txt: #e8e8f8;
  --gold: #f5c842;
  --gold2: #ffd966;
  --blue: #4466ff;
  --teal: #00d4aa;
  --pink: #ff4488;
  --radius: 10px;
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html { scroll-behavior: smooth; }

body {
  background: var(--ink);
  color: var(--txt);
  font-family: 'DM Mono', monospace;
  font-size: 13px;
  min-height: 100vh;
  overflow-x: hidden;
}

/* Subtle scanline texture */
body::after {
  content: '';
  position: fixed; inset: 0; z-index: 9999; pointer-events: none;
  background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.015) 2px, rgba(0,0,0,0.015) 4px);
}

/* ── LAYOUT ─────────────────────────────────────────── */
.app { display: flex; min-height: 100vh; }

/* SIDEBAR */
.sidebar {
  width: 220px; flex-shrink: 0;
  background: var(--paper);
  border-right: 1px solid var(--rim);
  display: flex; flex-direction: column;
  position: sticky; top: 0; height: 100vh;
  overflow-y: auto;
}

.sidebar-logo {
  padding: 24px 20px 20px;
  border-bottom: 1px solid var(--rim);
}
.sidebar-logo h1 {
  font-family: 'Bebas Neue', sans-serif;
  font-size: 26px; letter-spacing: 3px;
  color: var(--gold);
  text-shadow: 0 0 30px rgba(245,200,66,0.4);
  line-height: 1;
}
.sidebar-logo p { font-size: 9px; color: var(--dim); letter-spacing: 2px; margin-top: 4px; text-transform: uppercase; }

.nav { padding: 16px 0; flex: 1; }
.nav-item {
  display: flex; align-items: center; gap: 10px;
  padding: 11px 20px; cursor: pointer;
  color: var(--dim); font-size: 11px; letter-spacing: 1px;
  transition: all 0.15s; border-left: 2px solid transparent;
  text-transform: uppercase;
}
.nav-item:hover { color: var(--txt); background: rgba(68,102,255,0.06); }
.nav-item.active { color: var(--gold); border-left-color: var(--gold); background: rgba(245,200,66,0.05); }
.nav-item .ico { font-size: 15px; width: 20px; text-align: center; }

.sidebar-stats {
  padding: 16px 20px;
  border-top: 1px solid var(--rim);
}
.ss-row { display: flex; justify-content: space-between; align-items: baseline; margin-bottom: 8px; }
.ss-lbl { font-size: 9px; color: var(--dim); letter-spacing: 1px; text-transform: uppercase; }
.ss-val { font-family: 'Bebas Neue', sans-serif; font-size: 18px; color: var(--gold); letter-spacing: 1px; }
.ss-pnl { font-size: 11px; font-weight: 500; }

/* MAIN */
.main { flex: 1; min-width: 0; display: flex; flex-direction: column; }

.topbar {
  background: var(--paper); border-bottom: 1px solid var(--rim);
  padding: 14px 24px; display: flex; align-items: center; gap: 12px;
  flex-wrap: wrap; position: sticky; top: 0; z-index: 10;
}
.topbar-title { font-family: 'Bebas Neue', sans-serif; font-size: 22px; letter-spacing: 2px; color: var(--txt); margin-right: auto; }

/* SEARCH */
.search-wrap { position: relative; }
.search-ico { position: absolute; left: 10px; top: 50%; transform: translateY(-50%); color: var(--dim); font-size: 13px; pointer-events: none; }
.search-inp {
  background: var(--lift); border: 1px solid var(--rim);
  color: var(--txt); padding: 8px 12px 8px 32px;
  font-family: 'DM Mono', monospace; font-size: 11px;
  border-radius: var(--radius); outline: none; width: 200px;
  transition: border-color 0.2s, width 0.3s;
}
.search-inp:focus { border-color: var(--gold); width: 260px; }
.search-inp::placeholder { color: var(--fog); }

/* BUTTONS */
.btn {
  border: none; cursor: pointer; border-radius: var(--radius);
  font-family: 'DM Mono', monospace; font-size: 11px; font-weight: 500;
  padding: 8px 16px; display: inline-flex; align-items: center; gap: 6px;
  transition: all 0.15s; letter-spacing: 0.5px; white-space: nowrap;
}
.btn-gold { background: var(--gold); color: #000; font-weight: 700; }
.btn-gold:hover { background: var(--gold2); transform: translateY(-1px); box-shadow: 0 4px 16px rgba(245,200,66,0.3); }
.btn-blue { background: rgba(68,102,255,0.15); color: #7799ff; border: 1px solid rgba(68,102,255,0.3); }
.btn-blue:hover { background: rgba(68,102,255,0.25); border-color: var(--blue); }
.btn-ghost { background: var(--lift); color: var(--dim); border: 1px solid var(--rim); }
.btn-ghost:hover { border-color: var(--dim); color: var(--txt); }
.btn-danger { background: rgba(255,68,136,0.1); color: var(--pink); border: 1px solid rgba(255,68,136,0.2); }
.btn-danger:hover { background: rgba(255,68,136,0.2); }
.btn.spin-anim .spin { animation: rotate 1s linear infinite; display: inline-block; }
@keyframes rotate { to { transform: rotate(360deg); } }

/* FILTER CHIPS */
.chips { display: flex; gap: 6px; flex-wrap: wrap; padding: 12px 24px; border-bottom: 1px solid var(--rim); background: var(--paper); }
.chip {
  background: var(--lift); border: 1px solid var(--rim); color: var(--dim);
  padding: 5px 12px; border-radius: 20px; font-size: 10px; cursor: pointer;
  transition: all 0.15s; letter-spacing: 0.5px;
}
.chip:hover { border-color: var(--fog); color: var(--txt); }
.chip.on { background: rgba(245,200,66,0.12); border-color: var(--gold); color: var(--gold); }

/* SORT BAR */
.sortbar { display: flex; align-items: center; gap: 10px; padding: 10px 24px; background: var(--paper); border-bottom: 1px solid var(--rim); }
.sortbar-count { font-size: 10px; color: var(--dim); margin-right: auto; }
.sort-select {
  background: var(--lift); border: 1px solid var(--rim); color: var(--txt);
  padding: 6px 10px; font-family: 'DM Mono', monospace; font-size: 10px;
  border-radius: 6px; outline: none; cursor: pointer;
}
.view-toggle { display: flex; gap: 4px; }
.vtbtn {
  background: var(--lift); border: 1px solid var(--rim); color: var(--fog);
  width: 30px; height: 30px; border-radius: 6px; cursor: pointer; font-size: 13px;
  display: flex; align-items: center; justify-content: center; transition: all 0.15s;
}
.vtbtn.on { border-color: var(--gold); color: var(--gold); }

/* SCROLL AREA */
.scroll-area { flex: 1; overflow-y: auto; padding: 24px; }

/* ── CARD GRID ──────────────────────────────────────── */
.grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(175px, 1fr)); gap: 14px; }

.card-tile {
  background: var(--paper); border: 1px solid var(--rim);
  border-radius: 12px; overflow: hidden; cursor: pointer;
  transition: transform 0.2s, border-color 0.2s, box-shadow 0.2s;
  position: relative;
}
.card-tile:hover {
  transform: translateY(-5px);
  border-color: var(--gold);
  box-shadow: 0 10px 40px rgba(0,0,0,0.5), 0 0 20px rgba(245,200,66,0.12);
}

/* Holographic shimmer on hover */
.card-tile::before {
  content: '';
  position: absolute; inset: 0; z-index: 2;
  background: linear-gradient(135deg, rgba(255,255,255,0) 0%, rgba(255,255,255,0.04) 50%, rgba(255,255,255,0) 100%);
  opacity: 0; transition: opacity 0.3s;
}
.card-tile:hover::before { opacity: 1; }

.card-img-wrap {
  width: 100%; aspect-ratio: 3/4.1;
  background: linear-gradient(135deg, var(--lift), var(--paper));
  overflow: hidden; position: relative;
}
.card-img-wrap img {
  width: 100%; height: 100%; object-fit: cover;
  transition: transform 0.4s;
  display: block;
}
.card-tile:hover .card-img-wrap img { transform: scale(1.04); }

.card-img-placeholder {
  position: absolute; inset: 0;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  gap: 8px; color: var(--fog); font-size: 10px; text-align: center; padding: 10px;
}
.card-img-placeholder .ball { font-size: 38px; opacity: 0.25; animation: bob 3s ease-in-out infinite; }
@keyframes bob { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-5px)} }

/* Badge */
.tile-badge {
  position: absolute; top: 8px; left: 8px; z-index: 3;
  font-family: 'Bebas Neue', sans-serif; font-size: 11px; letter-spacing: 1px;
  padding: 2px 8px; border-radius: 4px;
}
.badge-pack { background: var(--gold); color: #000; }
.badge-grade { background: var(--blue); color: #fff; }

/* Delta badge top-right */
.tile-delta {
  position: absolute; top: 8px; right: 8px; z-index: 3;
  font-size: 10px; font-weight: 700; padding: 2px 7px; border-radius: 20px;
}
.delta-up { background: rgba(0,212,170,0.2); color: var(--teal); }
.delta-dn { background: rgba(255,68,136,0.2); color: var(--pink); }
.delta-eq { background: rgba(68,68,106,0.4); color: var(--dim); }

.card-foot { padding: 10px 11px 11px; }
.card-name { font-size: 11px; font-weight: 500; color: var(--txt); line-height: 1.25; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.card-ref { font-size: 9px; color: var(--fog); margin: 2px 0 8px; }
.card-price-row { display: flex; align-items: baseline; justify-content: space-between; }
.card-price { font-family: 'Bebas Neue', sans-serif; font-size: 20px; letter-spacing: 1px; color: var(--gold); }
.card-roi { font-size: 9px; }

/* ── TABLE VIEW ─────────────────────────────────────── */
.tbl-wrap { background: var(--paper); border: 1px solid var(--rim); border-radius: var(--radius); overflow: hidden; }
table { width: 100%; border-collapse: collapse; }
thead { background: var(--lift); }
th {
  padding: 10px 14px; font-size: 9px; color: var(--dim);
  letter-spacing: 2px; text-transform: uppercase; text-align: left;
  border-bottom: 1px solid var(--rim); cursor: pointer; user-select: none;
  white-space: nowrap;
}
th:hover { color: var(--gold); }
tr { border-bottom: 1px solid rgba(42,42,66,0.5); transition: background 0.12s; }
tr:last-child { border-bottom: none; }
tbody tr:hover { background: rgba(68,102,255,0.05); cursor: pointer; }
td { padding: 8px 14px; vertical-align: middle; }
.t-thumb { width: 36px; height: 50px; object-fit: cover; border-radius: 4px; background: var(--lift); display: block; }
.t-name { font-weight: 500; font-size: 11px; }
.t-sub { font-size: 9px; color: var(--fog); margin-top: 1px; }
.t-price { font-family: 'Bebas Neue', sans-serif; font-size: 17px; color: var(--gold); letter-spacing: 1px; }
.pill { display: inline-flex; align-items: center; padding: 2px 8px; border-radius: 20px; font-size: 9px; font-weight: 700; letter-spacing: 0.5px; }
.pill-up { background: rgba(0,212,170,0.12); color: var(--teal); }
.pill-dn { background: rgba(255,68,136,0.12); color: var(--pink); }
.pill-eq { background: rgba(68,68,106,0.2); color: var(--dim); }
.pill-gold { background: rgba(245,200,66,0.12); color: var(--gold); }

/* ── CHART TAB ──────────────────────────────────────── */
.chart-section { display: none; padding: 24px; }
.chart-card { background: var(--paper); border: 1px solid var(--rim); border-radius: var(--radius); padding: 20px; margin-bottom: 20px; }
.chart-head { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 16px; flex-wrap: wrap; gap: 8px; }
.chart-title { font-family: 'Bebas Neue', sans-serif; font-size: 18px; letter-spacing: 2px; color: var(--gold); }
.chart-sub { font-size: 9px; color: var(--dim); margin-top: 2px; letter-spacing: 1px; text-transform: uppercase; }
.chart-wrap { position: relative; height: 300px; }
.chart-btns { display: flex; gap: 6px; }

/* ── ADD / EDIT FORM ───────────────────────────────── */
.form-section { display: none; padding: 24px; }
.form-card { background: var(--paper); border: 1px solid var(--rim); border-radius: var(--radius); padding: 24px; max-width: 680px; }
.form-card h2 { font-family: 'Bebas Neue', sans-serif; font-size: 22px; letter-spacing: 3px; color: var(--gold); margin-bottom: 20px; }
.form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
.fg { display: flex; flex-direction: column; gap: 5px; }
.fg.full { grid-column: 1 / -1; }
.fg label { font-size: 9px; color: var(--dim); letter-spacing: 2px; text-transform: uppercase; }
.finp {
  background: var(--lift); border: 1px solid var(--rim); color: var(--txt);
  padding: 9px 12px; font-family: 'DM Mono', monospace; font-size: 12px;
  border-radius: 6px; outline: none; transition: border-color 0.2s; width: 100%;
}
.finp:focus { border-color: var(--gold); }
.finp::placeholder { color: var(--fog); }
select.finp { cursor: pointer; }
.form-actions { display: flex; gap: 10px; margin-top: 20px; flex-wrap: wrap; }
.form-preview { margin-top: 20px; display: none; }
.form-preview h3 { font-size: 9px; color: var(--dim); letter-spacing: 2px; text-transform: uppercase; margin-bottom: 10px; }
.preview-img-wrap { display: flex; gap: 16px; align-items: flex-start; }
.preview-img { width: 100px; height: 140px; object-fit: cover; border-radius: 8px; background: var(--lift); border: 1px solid var(--rim); }
.preview-info { flex: 1; }
.preview-name { font-family: 'Bebas Neue', sans-serif; font-size: 18px; color: var(--gold); letter-spacing: 1px; }
.preview-sub { font-size: 10px; color: var(--fog); margin-top: 3px; }
.preview-price { font-family: 'Bebas Neue', sans-serif; font-size: 28px; color: var(--teal); margin-top: 10px; }
.alert-ok {
  background: rgba(0,212,170,0.1); border: 1px solid rgba(0,212,170,0.3);
  color: var(--teal); border-radius: 6px; padding: 10px 14px;
  font-size: 11px; margin-top: 14px; display: none;
}

/* ── MODAL ──────────────────────────────────────────── */
.overlay {
  position: fixed; inset: 0; z-index: 100;
  background: rgba(5,5,15,0.88);
  backdrop-filter: blur(8px);
  display: flex; align-items: center; justify-content: center; padding: 16px;
  opacity: 0; pointer-events: none; transition: opacity 0.25s;
}
.overlay.open { opacity: 1; pointer-events: all; }

.modal {
  background: var(--paper); border: 1px solid var(--rim); border-radius: 16px;
  width: 100%; max-width: 760px; max-height: 92vh; overflow-y: auto;
  transform: scale(0.94) translateY(10px);
  transition: transform 0.25s;
}
.overlay.open .modal { transform: scale(1) translateY(0); }

.modal-layout { display: flex; gap: 0; }

/* Left image column */
.modal-img-col {
  width: 240px; flex-shrink: 0;
  background: linear-gradient(160deg, var(--lift), var(--ink));
  border-right: 1px solid var(--rim);
  display: flex; flex-direction: column; align-items: center;
  padding: 24px 16px; gap: 12px; border-radius: 16px 0 0 16px;
}
.modal-card-img {
  width: 180px; height: 252px; object-fit: cover; border-radius: 10px;
  box-shadow: 0 8px 32px rgba(0,0,0,0.6);
  background: var(--lift);
}
.modal-img-placeholder {
  width: 180px; height: 252px; border-radius: 10px;
  background: var(--lift); border: 1px solid var(--rim);
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  gap: 10px; color: var(--fog); font-size: 10px;
}
.modal-img-placeholder .ball2 { font-size: 50px; opacity: 0.2; }
.img-actions { display: flex; flex-direction: column; gap: 6px; width: 100%; }
.img-inp { font-size: 9px; text-align: center; }

/* Right content */
.modal-content { flex: 1; min-width: 0; padding: 22px; display: flex; flex-direction: column; gap: 16px; }

.modal-header { display: flex; justify-content: space-between; align-items: flex-start; }
.modal-title { font-family: 'Bebas Neue', sans-serif; font-size: 24px; letter-spacing: 2px; color: var(--gold); line-height: 1.1; }
.modal-ref { font-size: 10px; color: var(--fog); margin-top: 3px; }
.modal-close {
  background: none; border: 1px solid var(--rim); color: var(--dim);
  width: 30px; height: 30px; border-radius: 50%; cursor: pointer; font-size: 14px;
  display: flex; align-items: center; justify-content: center; flex-shrink: 0;
  transition: all 0.15s;
}
.modal-close:hover { border-color: var(--pink); color: var(--pink); }

/* Price history */
.ph-label { font-size: 9px; color: var(--dim); letter-spacing: 2px; text-transform: uppercase; margin-bottom: 8px; }
.ph-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(110px, 1fr)); gap: 8px; }
.ph-item {
  background: var(--lift); border: 1px solid var(--rim); border-radius: 6px;
  padding: 8px 10px; cursor: pointer; transition: border-color 0.15s;
  position: relative;
}
.ph-item:hover { border-color: var(--dim); }
.ph-item.editable:hover { border-color: var(--gold); }
.ph-date { font-size: 9px; color: var(--dim); }
.ph-val { font-family: 'Bebas Neue', sans-serif; font-size: 18px; color: var(--txt); letter-spacing: 1px; margin: 2px 0; }
.ph-delta { font-size: 9px; }
.ph-edit-hint { font-size: 8px; color: var(--fog); margin-top: 2px; }
.ph-item.editing { border-color: var(--gold); background: rgba(245,200,66,0.05); }
.ph-edit-inp {
  width: 100%; background: none; border: none; outline: none;
  font-family: 'Bebas Neue', sans-serif; font-size: 18px; color: var(--gold);
  letter-spacing: 1px;
}

/* Mini chart */
.mini-chart-wrap { height: 160px; }

/* Actions bar */
.modal-actions { display: flex; gap: 8px; flex-wrap: wrap; padding-top: 8px; border-top: 1px solid var(--rim); margin-top: auto; }

/* Edit price inline */
.add-price-row { display: flex; gap: 8px; align-items: flex-end; flex-wrap: wrap; }
.add-price-row .fg { flex: 1; min-width: 100px; }

/* ── PAGINATION ─────────────────────────────────────── */
.pagination { display: flex; align-items: center; gap: 6px; padding: 16px 24px; justify-content: flex-end; flex-wrap: wrap; }
.ppage {
  background: var(--lift); border: 1px solid var(--rim); color: var(--dim);
  min-width: 30px; height: 30px; border-radius: 6px; cursor: pointer;
  font-family: 'DM Mono', monospace; font-size: 10px;
  display: flex; align-items: center; justify-content: center; transition: all 0.15s; padding: 0 6px;
}
.ppage:hover, .ppage.on { border-color: var(--gold); color: var(--gold); }
.pinfo { font-size: 10px; color: var(--dim); margin-right: auto; }

/* ── TOAST ──────────────────────────────────────────── */
.toast {
  position: fixed; bottom: 24px; right: 24px; z-index: 999;
  background: var(--lift); border: 1px solid var(--gold);
  color: var(--gold); padding: 11px 18px; border-radius: 8px;
  font-size: 11px; max-width: 300px; line-height: 1.5;
  transform: translateY(16px); opacity: 0; transition: all 0.3s; pointer-events: none;
}
.toast.show { transform: translateY(0); opacity: 1; }

/* ── NOTICE ─────────────────────────────────────────── */
.notice {
  background: rgba(68,102,255,0.06); border: 1px solid rgba(68,102,255,0.2);
  color: #8899cc; border-radius: 8px; padding: 11px 16px;
  font-size: 10px; line-height: 1.7; margin-bottom: 20px;
}
.notice a { color: var(--gold); text-decoration: none; }
.notice strong { color: #aabbff; }

/* ── EMPTY ──────────────────────────────────────────── */
.empty-state { text-align: center; padding: 60px 20px; color: var(--fog); }
.empty-state .e-ico { font-size: 48px; opacity: 0.2; margin-bottom: 12px; }
.empty-state p { font-size: 11px; }

/* ── BOTTOM NAV (mobile only) ───────────────────────── */
.bottom-nav {
  display: none; /* hidden on desktop */
}

/* ── MOBILE ─────────────────────────────────────────── */
@media (max-width: 900px) {
  /* Hide desktop sidebar */
  .sidebar { display: none; }

  /* Bottom tab bar — iPhone style */
  .bottom-nav {
    display: flex;
    position: fixed; bottom: 0; left: 0; right: 0; z-index: 50;
    background: rgba(13,13,20,0.96);
    backdrop-filter: blur(16px) saturate(180%);
    -webkit-backdrop-filter: blur(16px) saturate(180%);
    border-top: 1px solid var(--rim);
    padding: 6px 0 max(8px, env(safe-area-inset-bottom));
    gap: 0;
  }
  .bnav-item {
    flex: 1; display: flex; flex-direction: column;
    align-items: center; justify-content: center;
    gap: 3px; padding: 4px 0;
    color: var(--dim); font-size: 9px; letter-spacing: 0.5px;
    text-transform: uppercase; cursor: pointer;
    transition: color 0.15s;
    -webkit-tap-highlight-color: transparent;
    user-select: none;
  }
  .bnav-item .bnav-ico { font-size: 20px; line-height: 1; }
  .bnav-item.active { color: var(--gold); }
  .bnav-item.active .bnav-ico { filter: drop-shadow(0 0 6px rgba(245,200,66,0.5)); }

  /* Add safe-area padding so content isn't under the bottom nav */
  .main {
    padding-bottom: calc(60px + env(safe-area-inset-bottom));
  }

  /* Topbar: compact, single line */
  .topbar {
    padding: 10px 12px;
    gap: 8px;
    flex-wrap: nowrap;
  }
  .topbar-title {
    font-size: 16px;
    letter-spacing: 1px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    min-width: 0;
  }
  /* Hide refresh and add buttons from topbar on mobile — actions moved to bottom nav */
  .topbar .btn-blue,
  .topbar .btn-gold { display: none; }

  /* Search full width */
  .search-inp { width: 100% !important; }
  .search-wrap { flex: 1; min-width: 0; }

  /* Chips: horizontal scroll (no wrap) */
  .chips {
    padding: 8px 12px;
    flex-direction: column !important;
    gap: 6px;
  }
  .chips-row {
    display: flex;
    gap: 6px;
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
    scrollbar-width: none;
    flex-wrap: nowrap !important;
    padding-bottom: 2px;
  }
  .chips-row::-webkit-scrollbar { display: none; }
  .chip { flex-shrink: 0; padding: 6px 13px; font-size: 11px; }

  /* Sort bar */
  .sortbar { padding: 8px 12px; gap: 6px; }
  .sortbar-count { font-size: 10px; }

  /* Grid: 2 columns on iPhone */
  .grid { grid-template-columns: repeat(2, 1fr); gap: 10px; }
  .scroll-area { padding: 12px; }

  /* Card tile: tighter */
  .card-foot { padding: 8px 9px 9px; }
  .card-name { font-size: 10px; }
  .card-ref  { font-size: 8px; }
  .card-price { font-size: 17px; }

  /* Modal: full screen on mobile */
  .overlay { padding: 0; align-items: flex-end; }
  .modal {
    max-width: 100%;
    max-height: 94vh;
    border-radius: 20px 20px 0 0;
    border-bottom: none;
    transform: translateY(40px) scale(1) !important; /* override desktop scale */
  }
  .overlay.open .modal { transform: translateY(0) scale(1) !important; }
  .modal-layout { flex-direction: column; }
  .modal-img-col {
    width: 100%;
    flex-direction: row;
    border-right: none;
    border-bottom: 1px solid var(--rim);
    border-radius: 20px 20px 0 0;
    padding: 14px 16px;
    gap: 14px;
    align-items: center;
  }
  .modal-card-img,
  .modal-img-placeholder { width: 90px; height: 126px; border-radius: 8px; flex-shrink: 0; }
  .img-actions { flex: 1; }
  .img-actions .btn { font-size: 10px; padding: 5px 10px; }
  .modal-content { padding: 16px; gap: 14px; }
  .modal-title { font-size: 20px; }
  .ph-grid { grid-template-columns: repeat(auto-fill, minmax(90px, 1fr)); gap: 6px; }
  .ph-val { font-size: 15px; }
  .add-price-row { flex-direction: column; gap: 8px; }
  .add-price-row .fg { width: 100%; }
  .add-price-row .btn { width: 100%; justify-content: center; }
  .move-row { flex-direction: column; gap: 8px; }
  .move-row .fg { width: 100%; }
  .move-row .btn { width: 100%; justify-content: center; }
  .modal-actions {
    flex-wrap: wrap;
    gap: 8px;
  }
  .modal-actions .btn,
  .modal-actions a.btn {
    flex: 1 1 calc(50% - 4px);
    justify-content: center;
    min-width: 0;
    font-size: 10px;
    padding: 8px 10px;
  }

  /* Form section */
  .form-section { padding: 0; }
  .form-card { border-radius: 0; border-left: none; border-right: none; padding: 16px; max-width: 100%; }
  .form-card h2 { font-size: 18px; margin-bottom: 14px; }
  .form-grid { grid-template-columns: 1fr; gap: 12px; }
  .finp { font-size: 16px !important; /* prevents iOS zoom on focus */ padding: 11px 12px; }
  select.finp { font-size: 16px !important; }
  .form-actions { flex-direction: column; }
  .form-actions .btn { width: 100%; justify-content: center; padding: 13px; font-size: 13px; }
  .notice { font-size: 11px; }

  /* Chart section */
  .chart-section { padding: 12px; }
  .chart-card { padding: 14px; }
  .chart-wrap { height: 220px; }
  .chart-btns { gap: 4px; }

  /* Pagination */
  .pagination { padding: 12px; gap: 4px; justify-content: center; }
  .ppage { width: 34px; height: 34px; font-size: 11px; }

  /* Hide table columns that don't fit */
  .hm { display: none; }

  /* Toast above bottom nav */
  .toast {
    bottom: calc(70px + env(safe-area-inset-bottom));
    left: 12px; right: 12px;
    max-width: none;
    text-align: center;
  }

  /* Enlarge tap targets */
  .chip, .btn, .vtbtn, .ppage { min-height: 36px; }
  .vtbtn { width: 36px; height: 36px; }
}

/* Very small phones */
@media (max-width: 390px) {
  .grid { grid-template-columns: repeat(2, 1fr); gap: 8px; }
  .scroll-area { padding: 10px; }
  .card-price { font-size: 15px; }
  .topbar-title { font-size: 14px; }
}

/* ── ANIMATIONS ─────────────────────────────────────── */
@keyframes cardIn {
  from { opacity: 0; transform: translateY(16px) scale(0.96); }
  to   { opacity: 1; transform: translateY(0) scale(1); }
}
.card-tile { animation: cardIn 0.45s cubic-bezier(.22,1,.36,1) both; }
.card-tile:active { transform: scale(0.98); }

@keyframes rowIn {
  from { opacity: 0; transform: translateX(-8px); }
  to   { opacity: 1; transform: translateX(0); }
}
tbody tr { animation: rowIn 0.35s ease both; }

@keyframes toastIn {
  0%   { transform: translateY(20px) scale(0.92); opacity: 0; }
  60%  { transform: translateY(-3px) scale(1.02); opacity: 1; }
  100% { transform: translateY(0) scale(1); }
}
.toast.show { animation: toastIn 0.4s ease; }

@keyframes flashGold {
  0%   { background: rgba(245,200,66,0.35); }
  100% { background: transparent; }
}
.flash { animation: flashGold 0.9s ease; }

@keyframes popIn {
  from { opacity: 0; transform: scale(0.85); }
  to   { opacity: 1; transform: scale(1); }
}
.chip { animation: popIn 0.3s ease both; }
.chip:active { transform: scale(0.94); }

.btn:active { transform: scale(0.96); }

@keyframes pulseGlow {
  0%,100% { box-shadow: 0 0 0 rgba(245,200,66,0); }
  50% { box-shadow: 0 0 14px rgba(245,200,66,0.35); }
}
.chase-tile { border-style: dashed !important; animation: pulseGlow 2.4s ease-in-out infinite; }

/* ── LOCATION PILL ──────────────────────────────────── */
.loc-pill {
  position: absolute; bottom: 8px; right: 8px; z-index: 3;
  font-size: 12px; background: rgba(13,13,20,0.7); border-radius: 50%;
  width: 22px; height: 22px; display: flex; align-items: center; justify-content: center;
  backdrop-filter: blur(4px);
}

/* ── CHIP ADD BUTTON ────────────────────────────────── */
.chip-add {
  background: transparent; border: 1px dashed var(--fog); color: var(--dim);
}
.chip-add:hover { border-color: var(--gold); color: var(--gold); }

/* ── MOVE CARD SECTION ──────────────────────────────── */
.move-row { display: flex; gap: 8px; align-items: flex-end; flex-wrap: wrap; }
.move-row .fg { flex: 1; min-width: 120px; }
.chips-row { display: flex; gap: 6px; flex-wrap: wrap; }
.chips-row + .chips-row { margin-top: 8px; }
</style>
</head>
<body>
<div class="app">

<!-- ── SIDEBAR ── -->
<aside class="sidebar">
  <div class="sidebar-logo">
    <h1>POKÉDEX<br>BOURSE</h1>
    <p>Collection Gaspard</p>
  </div>
  <nav class="nav">
    <div class="nav-item active" onclick="showSection('collection')" id="nav-collection">
      <span class="ico">🃏</span> Collection
    </div>
    <div class="nav-item" onclick="showSection('charts')" id="nav-charts">
      <span class="ico">📈</span> Graphiques
    </div>
    <div class="nav-item" onclick="showSection('add')" id="nav-add">
      <span class="ico">＋</span> Ajouter une carte
    </div>
  </nav>
  <div class="sidebar-stats" id="sidebarStats"></div>
</aside>

<!-- ── MAIN ── -->
<div class="main">

  <!-- Topbar -->
  <div class="topbar">
    <div class="topbar-title" id="topbarTitle">COLLECTION</div>
    <div class="search-wrap">
      <span class="search-ico">🔍</span>
      <input class="search-inp" id="searchInp" placeholder="Rechercher..." oninput="applyFilters()">
    </div>
    <button class="btn btn-blue" id="refreshBtn" onclick="doRefresh()">
      <span class="spin">↻</span> Actualiser les prix
    </button>
    <button class="btn btn-gold" onclick="showSection('add')">＋ Ajouter</button>
  </div>

  <!-- Filter chips -->
  <div class="chips" id="chipsBar" style="flex-direction:column;align-items:stretch">
    <div class="chips-row" id="chipsRowCat"></div>
    <div class="chips-row" id="chipsRowLoc"></div>
  </div>

  <!-- Sort bar -->
  <div class="sortbar">
    <span class="sortbar-count" id="countLbl"></span>
    <select class="sort-select" id="sortSel" onchange="applyFilters()">
      <option value="val_d">Valeur ↓</option>
      <option value="val_a">Valeur ↑</option>
      <option value="roi_d">ROI ↓</option>
      <option value="chg_d">Hausse totale ↓</option>
      <option value="name">Nom A→Z</option>
    </select>
    <div class="view-toggle">
      <button class="vtbtn on" id="vg" onclick="setView('grid')" title="Grille">⊞</button>
      <button class="vtbtn" id="vl" onclick="setView('list')" title="Liste">☰</button>
    </div>
  </div>

  <!-- Collection section -->
  <div id="collectionSection">
    <div class="scroll-area">
      <div id="gridView" class="grid"></div>
      <div id="listView" style="display:none">
        <div class="tbl-wrap">
          <table>
            <thead>
              <tr>
                <th style="width:46px">IMG</th>
                <th>CARTE</th>
                <th>INVESTI</th>
                <th>PRIX NM (FR)</th>
                <th class="hm">ROI</th>
                <th class="hm">Δ TOTAL</th>
                <th class="hm">GRADING</th>
                <th>CM</th>
              </tr>
            </thead>
            <tbody id="tblBody"></tbody>
          </table>
        </div>
      </div>
    </div>
    <div class="pagination" id="pagBar"></div>
  </div>

  <!-- Charts section -->
  <div class="chart-section" id="chartSection">
    <div class="chart-card">
      <div class="chart-head">
        <div>
          <div class="chart-title">ÉVOLUTION GLOBALE</div>
          <div class="chart-sub">Valeur totale de la collection par date (€)</div>
        </div>
        <div class="chart-btns">
          <button class="chip on" onclick="switchChart('total',this)">TOTAL</button>
          <button class="chip" onclick="switchChart('top10',this)">TOP 10</button>
          <button class="chip" onclick="switchChart('roi',this)">ROI %</button>
        </div>
      </div>
      <div class="chart-wrap"><canvas id="mainChart"></canvas></div>
    </div>
    <div class="chart-card">
      <div class="chart-title">TOP 10 — VALEUR ACTUELLE</div>
      <div class="chart-wrap" style="height:220px"><canvas id="top10Chart"></canvas></div>
    </div>
  </div>

  <!-- Add card section -->
  <div class="form-section" id="addSection">
    <div class="scroll-area">
    <div class="form-card">
      <h2>＋ AJOUTER UNE CARTE</h2>
      <div class="notice">
        💡 Renseigne le nom <strong>exact en français</strong>, la référence (ex: 199/165) et le set.
        Le <strong>prix NM France</strong> correspond à la 1ère offre de vente en état NM, vendeur France, sur Cardmarket.<br>
        L'image sera récupérée sur <strong>Pokécardex</strong> — ou colle directement une URL d'image.<br>
        🎯 Choisis la catégorie <strong>Chase Card</strong> pour une carte que tu vises pour un futur achat : elle apparaîtra dans son propre onglet et ne sera <strong>pas comptée</strong> dans la valeur de ta collection.
      </div>
      <div class="form-grid">
        <div class="fg">
          <label>Nom de la carte (FR) *</label>
          <input class="finp" id="f-name" placeholder="ex: DRACAUFEU EX" oninput="onFormChange()">
        </div>
        <div class="fg">
          <label>Référence (N°/Total)</label>
          <input class="finp" id="f-ref" placeholder="ex: 199/165" oninput="onFormChange()">
        </div>
        <div class="fg">
          <label>Set / Série</label>
          <input class="finp" id="f-set" placeholder="ex: 151 FR" oninput="onFormChange()">
        </div>
        <div class="fg">
          <label>Catégorie</label>
          <select class="finp" id="f-cat" onchange="onFormChange()"></select>
        </div>
        <div class="fg">
          <label>Localisation</label>
          <select class="finp" id="f-loc">
            <option value="maison">🏠 Chez moi</option>
            <option value="banque">🏦 En banque</option>
          </select>
        </div>
        <div class="fg">
          <label>Prix d'achat (€) — vide si packé</label>
          <input class="finp" id="f-invest" type="number" step="0.01" placeholder="45.00" oninput="onFormChange()">
        </div>
        <div class="fg">
          <label id="f-price-label">Prix NM France actuel (€) *</label>
          <input class="finp" id="f-price" type="number" step="0.01" placeholder="85.00" oninput="onFormChange()">
        </div>
        <div class="fg">
          <label>Grading (ex: CA 10, PCA 9.5)</label>
          <input class="finp" id="f-grade" placeholder="Laisser vide si non gradée">
        </div>
        <div class="fg">
          <label>Date de la cote</label>
          <input class="finp" id="f-date" placeholder="ex: 06/06/26">
        </div>
        <div class="fg full">
          <label>URL image (Pokécardex ou autre) — optionnel</label>
          <input class="finp" id="f-imgurl" placeholder="https://..." oninput="onFormChange()">
        </div>
      </div>
      <div class="form-preview" id="formPreview">
        <h3>Aperçu</h3>
        <div class="preview-img-wrap">
          <img class="preview-img" id="previewImg" src="" alt="">
          <div class="preview-info">
            <div class="preview-name" id="previewName">—</div>
            <div class="preview-sub" id="previewSub">—</div>
            <div class="preview-price" id="previewPrice">—</div>
          </div>
        </div>
      </div>
      <div class="alert-ok" id="addOk">✓ Carte ajoutée avec succès !</div>
      <div class="form-actions">
        <button class="btn btn-gold" onclick="addCard()">Ajouter à la collection</button>
        <button class="btn btn-ghost" onclick="clearAddForm()">Effacer</button>
      </div>
    </div>
    </div>
  </div>

</div><!-- /main -->
</div><!-- /app -->

<!-- ── BOTTOM NAV (mobile) ── -->
<nav class="bottom-nav" id="bottomNav">
  <div class="bnav-item active" id="bnav-collection" onclick="showSection('collection');setActiveBottomNav('collection')">
    <span class="bnav-ico">🃏</span>
    <span>Collection</span>
  </div>
  <div class="bnav-item" id="bnav-charts" onclick="showSection('charts');setActiveBottomNav('charts')">
    <span class="bnav-ico">📈</span>
    <span>Graphiques</span>
  </div>
  <div class="bnav-item" id="bnav-add" onclick="showSection('add');setActiveBottomNav('add')" style="color:var(--gold)">
    <span class="bnav-ico" style="font-size:26px;margin-top:-2px">＋</span>
    <span>Ajouter</span>
  </div>
  <div class="bnav-item" id="bnav-refresh" onclick="doRefresh()">
    <span class="bnav-ico" id="refreshIco">↻</span>
    <span>Actualiser</span>
  </div>
</nav>

<!-- ── MODAL ── -->
<div class="overlay" id="overlay" onclick="maybeCloseModal(event)">
  <div class="modal" id="modal">
    <div class="modal-layout">

      <!-- Left: image -->
      <div class="modal-img-col" id="modalImgCol">
        <div id="modalImgWrap">
          <img class="modal-card-img" id="modalImg" src="" alt="" style="display:none" onerror="showImgPlaceholder()">
          <div class="modal-img-placeholder" id="modalImgPlaceholder">
            <div class="ball2">⚡</div>
            <div id="modalImgPlaceholderName" style="font-size:10px;text-align:center;padding:0 8px;color:var(--fog)"></div>
          </div>
        </div>
        <div class="img-actions">
          <div style="font-size:9px;color:var(--dim);letter-spacing:1px;text-transform:uppercase;margin-bottom:2px">Changer l'image</div>
          <input class="finp img-inp" id="imgUrlInp" placeholder="URL Pokécardex..." style="font-size:10px;padding:6px 8px">
          <button class="btn btn-ghost" style="font-size:10px;padding:6px 12px;width:100%;justify-content:center" onclick="applyImgUrl()">Appliquer</button>
        </div>
      </div>

      <!-- Right: content -->
      <div class="modal-content">
        <div class="modal-header">
          <div>
            <div class="modal-title" id="mTitle"></div>
            <div class="modal-ref" id="mRef"></div>
          </div>
          <button class="modal-close" onclick="closeModal()">✕</button>
        </div>

        <!-- Price history + edit -->
        <div>
          <div class="ph-label">Historique des prix (cliquer pour modifier)</div>
          <div class="ph-grid" id="mPrices"></div>
        </div>

        <!-- Add new price entry -->
        <div>
          <div class="ph-label">Ajouter / Mettre à jour un prix</div>
          <div class="add-price-row">
            <div class="fg">
              <label>Date</label>
              <input class="finp" id="newPriceDate" placeholder="06/06/26" style="padding:7px 10px;font-size:11px">
            </div>
            <div class="fg">
              <label>Prix NM France (€)</label>
              <input class="finp" id="newPriceVal" type="number" step="0.01" placeholder="0.00" style="padding:7px 10px;font-size:11px">
            </div>
            <button class="btn btn-blue" onclick="addPriceEntry()" style="margin-bottom:1px">Enregistrer</button>
          </div>
        </div>

        <!-- Mini chart -->
        <div id="miniChartSection">
          <div class="ph-label">Évolution du prix</div>
          <div class="mini-chart-wrap"><canvas id="miniChart"></canvas></div>
        </div>

        <!-- Move card -->
        <div>
          <div class="ph-label">Déplacer la carte</div>
          <div class="move-row">
            <div class="fg">
              <label>Catégorie</label>
              <select class="finp" id="moveCat"></select>
            </div>
            <div class="fg">
              <label>Localisation</label>
              <select class="finp" id="moveLoc">
                <option value="maison">🏠 Chez moi</option>
                <option value="banque">🏦 En banque</option>
              </select>
            </div>
            <button class="btn btn-blue" onclick="applyMove()" style="margin-bottom:1px">Déplacer</button>
          </div>
        </div>

        <!-- Actions -->
        <div class="modal-actions" id="mActions"></div>
      </div>

    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
// ════════════════════════════════════════════════════
// DATA — Collection Gaspard (prix = 1ère offre NM France CM)
// ════════════════════════════════════════════════════
// Prix indexés : première offre de vente NM, vendeur France, Cardmarket FR
// Format prices : { "JJ/MM/AA": valeur }
// invest : null = packé (pas d'achat)
// imgUrl : URL image Pokécardex (peut être modifiée par l'utilisateur)

const DEFAULT_DATA = [
  // ── Les grosses pièces gradées ──
  {id:1,name:"LUGIA V",ref:"186/195",set:"SIT FR",cat:"SAR",invest:185,prices:{"30/03/24":450,"23/02/25":500,"25/03/26":1000},graded:"CA 10",
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SIT/186.jpg"},
  {id:2,name:"RAYQUAZA VMAX",ref:"218/203",set:"SIT FR",cat:"SAR",invest:700,prices:{"23/02/25":1000,"25/03/26":2000},graded:"CA 10",
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SIT/218.jpg"},
  {id:3,name:"GIRATINA VSTAR",ref:"GG69/GG70",set:"CRZ FR",cat:"TG",invest:56,prices:{"30/03/24":110,"23/02/25":250,"25/03/26":280},graded:"CA 9.5",
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG69.jpg"},
  {id:4,name:"MEWTWO VSTAR",ref:"GG44/GG70",set:"CRZ FR",cat:"TG",invest:139,prices:{"30/03/24":150,"23/02/25":155,"25/03/26":230},graded:"CA 9.5",
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG44.jpg"},
  {id:5,name:"RAYQUAZA VMAX TG",ref:"TG20/TG30",set:"BRS FR",cat:"TG",invest:65,prices:{"30/03/24":100,"23/02/25":140,"25/03/26":350},graded:"CA 10",
   imgUrl:"https://www.pokecardex.com/assets/img/sets/BRS/TG20.jpg"},
  {id:6,name:"RAIKOU V",ref:"GG41/GG70",set:"CRZ FR",cat:"TG",invest:41,prices:{"30/03/24":32,"23/02/25":45,"25/03/26":100},graded:"CA 10",
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG41.jpg"},
  {id:7,name:"SUICUNE V",ref:"GG38/GG70",set:"CRZ FR",cat:"TG",invest:42,prices:{"23/02/25":75,"25/03/26":170},graded:"CA 9.5",
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG38.jpg"},
  {id:8,name:"MEW V",ref:"251/264",set:"FST FR",cat:"SAR",invest:55,prices:{"30/03/24":80,"23/02/25":95,"25/03/26":130},graded:"CA 8",
   imgUrl:"https://www.pokecardex.com/assets/img/sets/FST/251.jpg"},
  {id:9,name:"PTÉRA V",ref:"180/196",set:"PAL FR",cat:"SAR",invest:185,prices:{"30/03/24":210,"23/02/25":210,"25/03/26":250},graded:"CA 7",
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/180.jpg"},
  {id:10,name:"GARDEVOIR EX",ref:"233/091",set:"DES PAL FR",cat:"SAR",invest:null,prices:{"25/03/26":185},graded:"CA 9.5",
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/233.jpg"},
  // ── SAR / VSTAR / VMAX ──
  {id:11,name:"LÉVIATOR EX",ref:"123/122",set:"TEF FR",cat:"SAR",invest:20,prices:{"30/03/24":40,"23/02/25":80,"25/03/26":175},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/TEF/123.jpg"},
  {id:12,name:"DRATTAK EX",ref:"187/159",set:"JTG FR",cat:"SAR",invest:140,prices:{"25/03/26":250},graded:"CA 10",
   imgUrl:"https://www.pokecardex.com/assets/img/sets/JTG/187.jpg"},
  {id:13,name:"ALAKAZAM EX",ref:"125/124",set:"TEF FR",cat:"SAR",invest:80,prices:{"25/03/26":300},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/TEF/125.jpg"},
  {id:14,name:"MEWTWO EX",ref:"163/162",set:"TEF FR",cat:"SAR",invest:80,prices:{"25/03/26":300},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/TEF/163.jpg"},
  {id:15,name:"ARCEUS VSTAR",ref:"GG70/GG70",set:"CRZ FR",cat:"TG",invest:55,prices:{"30/03/24":56,"23/02/25":130,"25/03/26":135},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG70.jpg"},
  {id:16,name:"DIALGA ORIGINEL VSTAR",ref:"GG68/GG70",set:"CRZ FR",cat:"TG",invest:25,prices:{"30/03/24":35,"23/02/25":90,"25/03/26":100},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG68.jpg"},
  {id:17,name:"PALKIA ORIGINEL VSTAR",ref:"GG67/GG70",set:"CRZ FR",cat:"TG",invest:38,prices:{"30/03/24":35,"23/02/25":90,"25/03/26":90},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG67.jpg"},
  {id:18,name:"DARKRAI VSTAR",ref:"GG50/GG70",set:"CRZ FR",cat:"TG",invest:30,prices:{"23/02/25":55,"25/03/26":80},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG50.jpg"},
  {id:19,name:"DEOXYS VSTAR",ref:"GG46/GG70",set:"CRZ FR",cat:"TG",invest:22,prices:{"30/03/24":25,"23/02/25":40,"25/03/26":80},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG46.jpg"},
  {id:20,name:"TYRANOCIF V",ref:"155/163",set:"AST FR",cat:"SAR",invest:79,prices:{"30/03/24":130,"23/02/25":130,"25/03/26":138},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/AST/155.jpg"},
  {id:21,name:"DRACAUFEU EX SAR",ref:"223/197",set:"OBF FR",cat:"SAR",invest:80,prices:{"30/03/24":72,"23/02/25":90,"25/03/26":70},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/OBF/223.jpg"},
  {id:22,name:"DRACAUFEU VSTAR PROMO",ref:"SWSH262",set:"PROMO FR",cat:"SAR",invest:58,prices:{"30/03/24":45,"23/02/25":140,"25/03/26":105},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PR/SWSH262.jpg"},
  {id:23,name:"AQUALI VMAX PROMO",ref:"SWSH182",set:"PROMO FR",cat:"SAR",invest:38,prices:{"30/03/24":50,"23/02/25":120,"25/03/26":95},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PR/SWSH182.jpg"},
  {id:24,name:"PALKIA ORIGINEL V",ref:"178/189",set:"AST FR",cat:"SAR",invest:60,prices:{"30/03/24":60,"23/02/25":70,"25/03/26":90},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/AST/178.jpg"},
  {id:25,name:"PINGOLÉON V",ref:"146/163",set:"AST FR",cat:"SAR",invest:45,prices:{"30/03/24":57,"23/02/25":70,"25/03/26":90},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/AST/146.jpg"},
  {id:26,name:"DRACAUFEU EX CLINIQUE",ref:"234/091",set:"DES PAL FR",cat:"SAR",invest:200,prices:{"25/03/26":210},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/234.jpg"},
  {id:27,name:"DRACAUFEU V CLINIQUE",ref:"154/172",set:"BRS FR",cat:"SAR",invest:245,prices:{"25/03/26":260},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/BRS/154.jpg"},
  {id:28,name:"MILOBELLUS EX CLINIQUE",ref:"237/191",set:"SSP FR",cat:"SAR",invest:200,prices:{"25/03/26":200},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SSP/237.jpg"},
  {id:29,name:"CELEBI V",ref:"245/264",set:"FST FR",cat:"SAR",invest:57,prices:{"30/03/24":65,"23/02/25":79,"25/03/26":85},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/FST/245.jpg"},
  {id:30,name:"BRUYVERNE V",ref:"196/203",set:"SIT FR",cat:"SAR",invest:35.5,prices:{"30/03/24":54,"23/02/25":47,"25/03/26":60},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SIT/196.jpg"},
  {id:31,name:"DIALGA ORIGINEL V",ref:"177/189",set:"AST FR",cat:"SAR",invest:60,prices:{"30/03/24":60,"23/02/25":60,"25/03/26":62},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/AST/177.jpg"},
  {id:32,name:"ARCEUS V",ref:"166/172",set:"BRS FR",cat:"SAR",invest:65,prices:{"30/03/24":50,"23/02/25":50,"25/03/26":65},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/BRS/166.jpg"},
  {id:33,name:"FEU PERÇANT EX",ref:"204/162",set:"TEF FR",cat:"SAR",invest:90,prices:{"23/02/25":89,"25/03/26":80},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/TEF/204.jpg"},
  {id:34,name:"VERT-DE-FER EX",ref:"203/162",set:"TEF FR",cat:"SAR",invest:70,prices:{"23/02/25":76,"25/03/26":65},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/TEF/203.jpg"},
  {id:35,name:"CHEF-DE-FER EX",ref:"206/162",set:"TEF FR",cat:"SAR",invest:70,prices:{"23/02/25":95,"25/03/26":70},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/TEF/206.jpg"},
  {id:36,name:"GARDE-DE-FER EX",ref:"249/182",set:"PAR FR",cat:"SAR",invest:73,prices:{"23/02/25":73,"25/03/26":45},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAR/249.jpg"},
  {id:37,name:"ALTARIA EX",ref:"253/182",set:"PAR FR",cat:"SAR",invest:90,prices:{"23/02/25":100,"25/03/26":80},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAR/253.jpg"},
  {id:38,name:"FARFUREX EX",ref:"175/189",set:"AST FR",cat:"SAR",invest:20,prices:{"25/03/26":50},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/AST/175.jpg"},
  {id:39,name:"DARDARGNAN V",ref:"167/189",set:"AST FR",cat:"SAR",invest:40,prices:{"23/02/25":50,"25/03/26":50},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/AST/167.jpg"},
  {id:40,name:"MYGAVOLT EX",ref:"168/142",set:"SCR FR",cat:"SAR",invest:null,prices:{"23/02/25":45,"25/03/26":40},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SCR/168.jpg"},
  {id:41,name:"KOREAIDON EX",ref:"247/198",set:"SVI FR",cat:"SAR",invest:40,prices:{"23/02/25":36,"25/03/26":30},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SVI/247.jpg"},
  {id:42,name:"MIRAIDON EX",ref:"244/198",set:"SVI FR",cat:"SAR",invest:30,prices:{"23/02/25":35,"25/03/26":30},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SVI/244.jpg"},
  {id:43,name:"GARDEVOIR EX",ref:"245/198",set:"SVI FR",cat:"SAR",invest:60,prices:{"25/03/26":70},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SVI/245.jpg"},
  {id:44,name:"MIASCARADE EX",ref:"256/193",set:"PAL FR",cat:"SAR",invest:20,prices:{"30/03/24":22,"23/02/25":18,"25/03/26":20},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/256.jpg"},
  {id:45,name:"CARCHACROK EX",ref:"245/182",set:"PAR FR",cat:"SAR",invest:52,prices:{"30/03/24":44,"23/02/25":45,"25/03/26":40},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAR/245.jpg"},
  {id:46,name:"YUYU EX",ref:"259/193",set:"PAL FR",cat:"SAR",invest:45,prices:{"30/03/24":38,"23/02/25":38,"25/03/26":36},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/259.jpg"},
  {id:47,name:"FLAMIGATOR EX",ref:"258/193",set:"PAL FR",cat:"SAR",invest:20,prices:{"23/02/25":20,"25/03/26":20},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/258.jpg"},
  {id:48,name:"DINGLU EX",ref:"263/193",set:"PAL FR",cat:"SAR",invest:12,prices:{"25/03/26":12},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/263.jpg"},
  {id:49,name:"GENESECT EX",ref:"169/086",set:"SFA FR",cat:"SAR",invest:20,prices:{"25/03/26":35},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SFA/169.jpg"},
  {id:50,name:"GENESECT V",ref:"255/264",set:"FST FR",cat:"SAR",invest:20,prices:{"25/03/26":25},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/FST/255.jpg"},
  {id:51,name:"ZACIAN EX",ref:"186/159",set:"JTG FR",cat:"SAR",invest:null,prices:{"25/03/26":55},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/JTG/186.jpg"},
  {id:52,name:"MÉGA DIANCIE EX",ref:"282/217",set:"TWM FR",cat:"SAR",invest:null,prices:{"25/03/26":110},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/TWM/282.jpg"},
  {id:53,name:"MÉGA OHMASSACRE",ref:"278/217",set:"TWM FR",cat:"SAR",invest:null,prices:{"25/03/26":45},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/TWM/278.jpg"},
  {id:54,name:"URSAKING LUNE V.EX",ref:"216/217",set:"TWM FR",cat:"SAR",invest:40,prices:{"25/03/26":35},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/TWM/216.jpg"},
  {id:55,name:"CORBOSS V",ref:"162/172",set:"BRS FR",cat:"SAR",invest:25,prices:{"30/03/24":20,"23/02/25":22,"25/03/26":19},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/BRS/162.jpg"},
  {id:56,name:"HOOPA V",ref:"GG53/GG70",set:"CRZ FR",cat:"TG",invest:7,prices:{"30/03/24":14,"23/02/25":15,"25/03/26":18},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG53.jpg"},
  {id:57,name:"ZAMAZENTA V GG",ref:"GG54/GG70",set:"CRZ FR",cat:"TG",invest:30,prices:{"23/02/25":30,"25/03/26":35},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG54.jpg"},
  {id:58,name:"LUMINÉON V",ref:"156/172",set:"BRS FR",cat:"SAR",invest:27,prices:{"23/02/25":25,"25/03/26":23},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/BRS/156.jpg"},
  {id:59,name:"LUMINÉON V GG",ref:"GG39/GG70",set:"CRZ FR",cat:"TG",invest:15,prices:{"23/02/25":15,"25/03/26":20},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG39.jpg"},
  {id:60,name:"ZARBI V",ref:"177/195",set:"SIT FR",cat:"SAR",invest:35,prices:{"23/02/25":40,"25/03/26":37},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SIT/177.jpg"},
  {id:61,name:"BERSEKATT DE GALAR V",ref:"184/196",set:"SIT FR",cat:"SAR",invest:47,prices:{"23/02/25":38,"25/03/26":30},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SIT/184.jpg"},
  {id:62,name:"MOUFFLAIR V",ref:"181/195",set:"SIT FR",cat:"SAR",invest:15,prices:{"23/02/25":20,"25/03/26":16},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SIT/181.jpg"},
  {id:63,name:"TAPATOÈS EX",ref:"264/193",set:"PAL FR",cat:"SAR",invest:10,prices:{"23/02/25":15,"25/03/26":12},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/264.jpg"},
  {id:64,name:"ROC-DE-FER EX",ref:"207/162",set:"TEF FR",cat:"SAR",invest:43,prices:{"23/02/25":50,"25/03/26":40},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/TEF/207.jpg"},
  {id:65,name:"FORT IVOIRE EX",ref:"246/198",set:"SVI FR",cat:"SAR",invest:15,prices:{"25/03/26":25},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SVI/246.jpg"},
  {id:66,name:"REGIDRAG O V",ref:"184/195",set:"SIT FR",cat:"SAR",invest:15,prices:{"25/03/26":25},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SIT/184.jpg"},
  {id:67,name:"PALMVAL EX",ref:"260/193",set:"PAL FR",cat:"SAR",invest:15,prices:{"25/03/26":15},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/260.jpg"},
  {id:68,name:"PIKACHU VMAX TG",ref:"TG17/TG30",set:"BRS FR",cat:"TG",invest:54,prices:{"23/02/25":65,"25/03/26":67},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/BRS/TG17.jpg"},
  {id:69,name:"BRASÉGALI VMAX TG",ref:"TG15/TG30",set:"AST FR",cat:"TG",invest:20,prices:{"30/03/24":15,"23/02/25":30,"25/03/26":26},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/AST/TG15.jpg"},
  {id:70,name:"MEW GG",ref:"GG10/GG20",set:"CRZ FR",cat:"TG",invest:7,prices:{"30/03/24":7.5,"25/03/26":31},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG10.jpg"},
  {id:71,name:"DEOXYS GG",ref:"GG12/GG20",set:"CRZ FR",cat:"TG",invest:4,prices:{"30/03/24":3.5,"25/03/26":16},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CRZ/GG12.jpg"},
  {id:72,name:"DRACAUFEU TG",ref:"TG03/TG30",set:"BRS FR",cat:"TG",invest:9,prices:{"30/03/24":10,"25/03/26":20},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/BRS/TG03.jpg"},
  {id:73,name:"CORVAILLUS VMAX TG",ref:"TG19/TG30",set:"BRS FR",cat:"TG",invest:11,prices:{"30/03/24":12,"23/02/25":12,"25/03/26":13},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/BRS/TG19.jpg"},
  {id:74,name:"ÉTHERNATOS VMAX TG",ref:"TG22/TG30",set:"BRS FR",cat:"TG",invest:8,prices:{"30/03/24":15,"23/02/25":15,"25/03/26":13},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/BRS/TG22.jpg"},
  {id:75,name:"SHIFOURS VMAX TG",ref:"TG21/TG30",set:"BRS FR",cat:"TG",invest:10,prices:{"30/03/24":14,"23/02/25":13,"25/03/26":15},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/BRS/TG21.jpg"},
  // ── AR ──
  {id:76,name:"GROUDON AR",ref:"199/182",set:"PAR FR",cat:"AR",invest:null,prices:{"30/03/24":90,"25/03/26":140},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAR/199.jpg"},
  {id:77,name:"ARTIKODIN AR",ref:"161/159",set:"JTG FR",cat:"AR",invest:null,prices:{"25/03/26":72},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/JTG/161.jpg"},
  {id:78,name:"TYRANOCIF AR",ref:"222/193",set:"PAL FR",cat:"AR",invest:40,prices:{"30/03/24":35,"25/03/26":60},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/222.jpg"},
  {id:79,name:"FEUNARD AR",ref:"199/197",set:"OBF FR",cat:"AR",invest:19.5,prices:{"30/03/24":17,"25/03/26":35},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/OBF/199.jpg"},
  {id:80,name:"YVELTAL AR",ref:"205/182",set:"PAR FR",cat:"AR",invest:null,prices:{"30/03/24":13,"25/03/26":32},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAR/205.jpg"},
  {id:81,name:"MAGIRÊVE AR",ref:"212/193",set:"PAL FR",cat:"AR",invest:10,prices:{"25/03/26":27},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/212.jpg"},
  {id:82,name:"SIMULARBR AR",ref:"219/193",set:"PAL FR",cat:"AR",invest:9,prices:{"30/03/24":9.5,"25/03/26":25},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/219.jpg"},
  {id:83,name:"INCISACHE AR",ref:"077/064",set:"SFA FR",cat:"AR",invest:11,prices:{"25/03/26":22},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/SFA/077.jpg"},
  {id:84,name:"STEELIX AR",ref:"208/182",set:"PAR FR",cat:"AR",invest:19.9,prices:{"30/03/24":27,"25/03/26":25},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAR/208.jpg"},
  {id:85,name:"BABIMANTA AR",ref:"189/182",set:"PAR FR",cat:"AR",invest:11,prices:{"25/03/26":18},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAR/189.jpg"},
  {id:86,name:"CIZAYOX AR",ref:"205/197",set:"OBF FR",cat:"AR",invest:5,prices:{"30/03/24":7.5,"25/03/26":18},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/OBF/205.jpg"},
  {id:87,name:"SCARABRUTE AR",ref:"168/167",set:"TWN FR",cat:"AR",invest:9,prices:{"25/03/26":14},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/TWN/168.jpg"},
  {id:88,name:"PHIONE AR",ref:"175/167",set:"TWM FR",cat:"AR",invest:8,prices:{"25/03/26":14},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/TWM/175.jpg"},
  {id:89,name:"TAUROS DE PALDEA AR",ref:"218/193",set:"PAL FR",cat:"AR",invest:13.5,prices:{"30/03/24":15,"25/03/26":25},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/218.jpg"},
  {id:90,name:"SHAOFOUINE AR",ref:"200/182",set:"PAR FR",cat:"AR",invest:9,prices:{"30/03/24":10,"25/03/26":14},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAR/200.jpg"},
  {id:91,name:"MATOURGEON AR",ref:"197/193",set:"PAL FR",cat:"AR",invest:8,prices:{"30/03/24":10,"25/03/26":11},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/197.jpg"},
  {id:92,name:"FAMIGNOL AR",ref:"226/193",set:"PAL FR",cat:"AR",invest:null,prices:{"30/03/24":14,"25/03/26":28},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/PAL/226.jpg"},
  // ── 151 ──
  {id:93,name:"SALAMÈCHE AR",ref:"168/165",set:"151 FR",cat:"151",invest:null,prices:{"30/03/24":46,"30/03/25":60,"26/02/26":85,"25/03/26":150},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/168.jpg"},
  {id:94,name:"REPTINCEL AR",ref:"169/165",set:"151 FR",cat:"151",invest:null,prices:{"30/03/24":52,"30/03/25":40,"26/02/26":55,"25/03/26":105},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/169.jpg"},
  {id:95,name:"TÊTARTE AR",ref:"176/165",set:"151 FR",cat:"151",invest:null,prices:{"30/03/24":24,"30/03/25":30,"26/02/26":27,"25/03/26":95},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/176.jpg"},
  {id:96,name:"BULBIZARRE AR",ref:"166/165",set:"151 FR",cat:"151",invest:null,prices:{"30/03/24":44,"30/03/25":40,"26/02/26":55,"25/03/26":95},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/166.jpg"},
  {id:97,name:"PIKACHU AR",ref:"173/165",set:"151 FR",cat:"151",invest:null,prices:{"30/03/24":35,"30/03/25":50,"26/02/26":62,"25/03/26":90},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/173.jpg"},
  {id:98,name:"CARAPUCE AR",ref:"170/165",set:"151 FR",cat:"151",invest:null,prices:{"30/03/24":47,"30/03/25":55,"26/02/26":60,"25/03/26":83},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/170.jpg"},
  {id:99,name:"HERBIZARRE AR",ref:"167/165",set:"151 FR",cat:"151",invest:37,prices:{"30/03/24":40,"30/03/25":43,"26/02/26":45,"25/03/26":83},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/167.jpg"},
  {id:100,name:"CARABAFFE AR",ref:"171/165",set:"151 FR",cat:"151",invest:null,prices:{"30/03/24":39,"30/03/25":44,"26/02/26":55,"25/03/26":88},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/171.jpg"},
  {id:101,name:"DRACO AR",ref:"181/165",set:"151 FR",cat:"151",invest:30,prices:{"30/03/24":40,"30/03/25":45,"26/02/26":60,"25/03/26":83},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/181.jpg"},
  {id:102,name:"FLORIZARRE EX SAR",ref:"198/165",set:"151 FR",cat:"151",invest:45,prices:{"30/03/24":75,"30/03/25":80,"26/02/26":100,"25/03/26":180},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/198.jpg"},
  {id:103,name:"ÉLECTHOR EX SAR",ref:"202/165",set:"151 FR",cat:"151",invest:65,prices:{"30/03/24":76,"30/03/25":80,"26/02/26":135,"25/03/26":190},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/202.jpg"},
  {id:104,name:"TORTANK EX SAR",ref:"200/165",set:"151 FR",cat:"151",invest:null,prices:{"30/03/24":69,"30/03/25":80,"26/02/26":120,"25/03/26":175},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/200.jpg"},
  {id:105,name:"ALAKAZAM EX SAR",ref:"201/165",set:"151 FR",cat:"151",invest:null,prices:{"30/03/24":62,"30/03/25":67,"26/02/26":85,"25/03/26":180},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/201.jpg"},
  {id:106,name:"MEW EX SAR PROMO",ref:"053",set:"151 FR PROMO",cat:"151",invest:null,prices:{"30/03/24":28,"30/03/25":50,"26/02/26":95,"25/03/26":100},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/053.jpg"},
  {id:107,name:"MEWTWO AR PROMO",ref:"052",set:"151 FR PROMO",cat:"151",invest:null,prices:{"30/03/24":17,"30/03/25":30,"26/02/26":70,"25/03/26":65},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/052.jpg"},
  {id:108,name:"PSYKOKWAK AR",ref:"175/165",set:"151 FR",cat:"151",invest:null,prices:{"30/03/24":20,"30/03/25":26,"26/02/26":30,"25/03/26":44},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/175.jpg"},
  {id:109,name:"DRACAUFEU EX FA",ref:"183/165",set:"151 FR",cat:"151",invest:null,prices:{"30/03/24":115,"30/03/25":95,"26/02/26":60,"25/03/26":70},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/183.jpg"},
  {id:110,name:"MEW EX FA",ref:"193/165",set:"151 FR",cat:"151",invest:null,prices:{"30/03/24":50,"30/03/25":50,"26/02/26":35,"25/03/26":37},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/MEW/193.jpg"},
  // ── EX ancienne ère ──
  {id:111,name:"GARDEVOIR EX XY",ref:"116/114",set:"XY FR",cat:"EX",invest:55,prices:{"25/03/26":110},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/XY1/116.jpg"},
  {id:112,name:"KYOGRE EX",ref:"6/34",set:"XY1 FR",cat:"EX",invest:20,prices:{"25/03/26":200},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/XYPR/6.jpg"},
  {id:113,name:"MÉGA LOCKPIN EX",ref:"128/094",set:"XY3 FR",cat:"EX",invest:25,prices:{"25/03/26":25},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/FLF/128.jpg"},
  {id:114,name:"MÉGA SHARPEDO EX",ref:"127/094",set:"XY3 FR",cat:"EX",invest:30,prices:{"25/03/26":40},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/FLF/127.jpg"},
  {id:115,name:"M RAYQUAZA EX 25ANS",ref:"76/108",set:"XY6 25ANS FR",cat:"EX",invest:34.9,prices:{"30/03/24":35,"23/02/25":40,"25/03/26":110},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/RSK/76.jpg"},
  {id:116,name:"VOLCANION EX",ref:"182/159",set:"JTG FR",cat:"EX",invest:30,prices:{"25/03/26":40},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/JTG/182.jpg"},
  {id:117,name:"DRACAUFEU GX",ref:"20/147",set:"SM3 FR",cat:"GX",invest:16.9,prices:{"30/03/24":16,"23/02/25":16,"25/03/26":19},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/BS/20.jpg"},
  // ── Vintage / Légende ──
  {id:118,name:"LUGIA LÉGENDE COMPLET",ref:"—",set:"HS FR",cat:"VINTAGE",invest:50,prices:{"25/03/26":170},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/HS/113.jpg"},
  {id:119,name:"HO-OH LÉGENDE COMPLET",ref:"—",set:"HS FR",cat:"VINTAGE",invest:50,prices:{"25/03/26":170},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/HS/111.jpg"},
  {id:120,name:"SUICUNE ENTEI LÉGENDE",ref:"—",set:"HS FR",cat:"VINTAGE",invest:50,prices:{"25/03/26":100},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/HS/115.jpg"},
  {id:121,name:"KYOGRE & GROUDON LÉGENDE",ref:"—",set:"HS FR",cat:"VINTAGE",invest:25,prices:{"25/03/26":80},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/HS/87.jpg"},
  {id:122,name:"RAIKOU & SUICUNE LÉGENDE",ref:"—",set:"HS FR",cat:"VINTAGE",invest:25,prices:{"25/03/26":70},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/HS/117.jpg"},
  {id:123,name:"STELIX PRIME",ref:"87/95",set:"HS FR",cat:"VINTAGE",invest:40,prices:{"30/03/24":40,"23/02/25":40,"25/03/26":50},graded:"PCA 7",
   imgUrl:"https://www.pokecardex.com/assets/img/sets/HS/87.jpg"},
  {id:124,name:"FLORIZARRE 25ANS",ref:"15/102",set:"B&W 25ANS FR",cat:"VINTAGE",invest:19.9,prices:{"30/03/24":17,"23/02/25":17,"25/03/26":30},graded:null,
   imgUrl:"https://www.pokecardex.com/assets/img/sets/CEL25/15.jpg"},
];

// ════════════════════════════════════════════════════
// Cardmarket URL — lien direct vers la carte en NM + vendeur France
// ════════════════════════════════════════════════════
function cmUrl(card) {
  // URL Cardmarket avec filtres : langue FR, état NM (4), vendeur France
  // Format : /fr/Pokemon/Products/Singles/{set-slug}/{card-slug}
  // On construit une URL de recherche filtrée NM + FR + vendeur France
  const name = encodeURIComponent(card.name);
  const base = 'https://www.cardmarket.com/fr/Pokemon/Products/Search';
  // Paramètres : idLanguage=2 (FR), minCondition=4 (NM), sellerCountry=6 (France)
  return `${base}?searchString=${name}&idLanguage=2&minCondition=4&sellerCountry=6`;
}

// ════════════════════════════════════════════════════
// PERSISTENCE — localStorage
// ════════════════════════════════════════════════════
const LS_KEY = 'pokedex_gaspard_v3';
let COLLECTION = [];
let nextId = 200;

// ── CATEGORIES (extensible) ──
const DEFAULT_CATEGORIES = [
  {id:'SAR',label:'SAR / V / VMAX / VSTAR'},
  {id:'AR',label:'AR'},
  {id:'TG',label:'TG / GG'},
  {id:'151',label:'151'},
  {id:'EX',label:'EX (ancienne ère)'},
  {id:'GX',label:'GX'},
  {id:'VINTAGE',label:'Vintage / Légende'},
  {id:'CHASE',label:'🎯 Chase Card'},
];
let CATEGORIES = [];

function saveData() {
  try { localStorage.setItem(LS_KEY, JSON.stringify({ cards: COLLECTION, nextId, categories: CATEGORIES })); } catch(e){}
}

function loadData() {
  try {
    const raw = localStorage.getItem(LS_KEY);
    if (raw) {
      const d = JSON.parse(raw);
      COLLECTION = d.cards || DEFAULT_DATA;
      nextId = d.nextId || 200;
      CATEGORIES = d.categories || JSON.parse(JSON.stringify(DEFAULT_CATEGORIES));
    } else {
      COLLECTION = JSON.parse(JSON.stringify(DEFAULT_DATA));
      nextId = 200;
      CATEGORIES = JSON.parse(JSON.stringify(DEFAULT_CATEGORIES));
    }
  } catch(e){
    COLLECTION = JSON.parse(JSON.stringify(DEFAULT_DATA));
    nextId = 200;
    CATEGORIES = JSON.parse(JSON.stringify(DEFAULT_CATEGORIES));
  }
  // Ensure every card has a location and known category
  COLLECTION.forEach(c => {
    if (!c.loc) c.loc = 'maison';
    if (!CATEGORIES.find(cat=>cat.id===c.cat)) {
      CATEGORIES.push({id:c.cat, label:c.cat});
    }
  });
  // Ensure CHASE category always exists
  if (!CATEGORIES.find(cat=>cat.id==='CHASE')) CATEGORIES.push({id:'CHASE',label:'🎯 Chase Card'});
}

function catLabel(id) {
  const c = CATEGORIES.find(x=>x.id===id);
  return c ? c.label : id;
}

// ════════════════════════════════════════════════════
// HELPERS
// ════════════════════════════════════════════════════
function sortedDates(prices) {
  return Object.keys(prices).sort((a, b) => {
    const [da,ma,ya] = a.split('/'); const [db,mb,yb] = b.split('/');
    return new Date(+('20'+ya), +ma-1, +da) - new Date(+('20'+yb), +mb-1, +db);
  });
}
function latestP(c) { const k = sortedDates(c.prices); return k.length ? c.prices[k[k.length-1]] : null; }
function firstP(c)  { const k = sortedDates(c.prices); return k.length ? c.prices[k[0]] : null; }
function fEur(v)    { if (v==null||isNaN(v)) return '—'; return Math.round(v*100)/100 + ' €'; }
function roi(c)     { const p=latestP(c); if(!c.invest||!p) return null; return (p-c.invest)/c.invest*100; }
function chgTotal(c){ const f=firstP(c),l=latestP(c); if(!f||!l) return null; return (l-f)/f*100; }
function fPct(v)    { if(v==null) return '—'; return (v>0?'+':'') + v.toFixed(1) + '%'; }
function pillCls(v) { return v>0?'pill-up':v<0?'pill-dn':'pill-eq'; }
function deltaCls(v){ return v>0?'delta-up':v<0?'delta-dn':'delta-eq'; }
function deltaIcon(v){ return v>0?'▲':v<0?'▼':'—'; }

// ════════════════════════════════════════════════════
// STATE
// ════════════════════════════════════════════════════
let filtered = [];
let activeCat = 'all';
let activeLoc = 'all';
let curPage = 1;
const PER = 24;
let viewMode = 'grid';
let openCardId = null;
let mainChartObj = null;
let miniChartObj = null;
let top10ChartObj = null;
let chartMode = 'total';

// ════════════════════════════════════════════════════
// SIDEBAR STATS
// ════════════════════════════════════════════════════
function renderSidebarStats() {
  let tv=0, ti=0;
  const owned = COLLECTION.filter(c=>c.cat!=='CHASE');
  const chase = COLLECTION.filter(c=>c.cat==='CHASE');
  owned.forEach(c => {
    const p = latestP(c); if(p) tv += p;
    if(c.invest) ti += c.invest;
  });
  const pnl = tv - ti;
  const r = ti > 0 ? pnl/ti*100 : 0;
  const banque = owned.filter(c=>c.loc==='banque').length;
  const maison = owned.filter(c=>c.loc!=='banque').length;
  document.getElementById('sidebarStats').innerHTML = `
    <div class="ss-row"><span class="ss-lbl">Valeur totale</span></div>
    <div class="ss-val">${Math.round(tv).toLocaleString('fr-FR')} €</div>
    <div style="height:8px"></div>
    <div class="ss-row"><span class="ss-lbl">Investi</span></div>
    <div style="font-size:14px;color:var(--dim);font-family:'Bebas Neue';letter-spacing:1px">${Math.round(ti).toLocaleString('fr-FR')} €</div>
    <div style="height:8px"></div>
    <div class="ss-row"><span class="ss-lbl">Plus-value</span></div>
    <div class="ss-pnl" style="color:${pnl>=0?'var(--teal)':'var(--pink)'}">${pnl>=0?'▲':'▼'} ${Math.abs(Math.round(pnl)).toLocaleString('fr-FR')} € (${fPct(r)})</div>
    <div style="height:8px"></div>
    <div class="ss-row">
      <span class="ss-lbl">${owned.length} cartes</span>
      <span class="ss-lbl">${owned.filter(c=>c.graded).length} gradées</span>
    </div>
    <div class="ss-row">
      <span class="ss-lbl">🏠 ${maison}</span>
      <span class="ss-lbl">🏦 ${banque}</span>
    </div>
    ${chase.length ? `<div style="height:8px"></div><div class="ss-row"><span class="ss-lbl">🎯 Chase list</span><span class="ss-lbl">${chase.length}</span></div>` : ''}
  `;
}

// ════════════════════════════════════════════════════
// FILTER + SORT
// ════════════════════════════════════════════════════
function applyFilters() {
  const q = (document.getElementById('searchInp').value||'').toLowerCase();
  const srt = document.getElementById('sortSel').value;
  filtered = COLLECTION.filter(c => {
    const ms = !q || c.name.toLowerCase().includes(q) || c.ref.toLowerCase().includes(q) || c.set.toLowerCase().includes(q);
    let mf = true;
    if (activeCat==='all') mf = c.cat!=='CHASE';
    else if (activeCat==='profit') { const x=chgTotal(c); mf=x!==null&&x>0 && c.cat!=='CHASE'; }
    else if (activeCat==='loss') { const x=chgTotal(c); mf=x!==null&&x<0 && c.cat!=='CHASE'; }
    else if (activeCat==='graded') mf = !!c.graded && c.cat!=='CHASE';
    else mf = c.cat===activeCat;
    let ml = true;
    if (activeLoc==='maison') ml = c.loc!=='banque';
    else if (activeLoc==='banque') ml = c.loc==='banque';
    return ms && mf && ml;
  });
  filtered.sort((a,b) => {
    if(srt==='val_d') return (latestP(b)||0)-(latestP(a)||0);
    if(srt==='val_a') return (latestP(a)||0)-(latestP(b)||0);
    if(srt==='roi_d') return (roi(b)||-999)-(roi(a)||-999);
    if(srt==='chg_d') return (chgTotal(b)||-999)-(chgTotal(a)||-999);
    if(srt==='name') return a.name.localeCompare(b.name,'fr');
    return 0;
  });
  curPage = 1;
  document.getElementById('countLbl').textContent = `${filtered.length} carte${filtered.length>1?'s':''}`;
  renderView();
}

function setCat(c, el) {
  activeCat = c;
  document.querySelectorAll('#chipsRowCat .chip').forEach(b => b.classList.remove('on'));
  el.classList.add('on');
  applyFilters();
}

function setLoc(l, el) {
  activeLoc = l;
  document.querySelectorAll('#chipsRowLoc .chip').forEach(b => b.classList.remove('on'));
  el.classList.add('on');
  applyFilters();
}

// ════════════════════════════════════════════════════
// CHIPS (catégories + localisation, dynamiques)
// ════════════════════════════════════════════════════
function renderChips() {
  const catRow = document.getElementById('chipsRowCat');
  let h = `<span class="chip${activeCat==='all'?' on':''}" onclick="setCat('all',this)">Toutes</span>`;
  CATEGORIES.filter(c=>c.id!=='CHASE').forEach(c => {
    h += `<span class="chip${activeCat===c.id?' on':''}" onclick="setCat('${c.id}',this)">${c.label}</span>`;
  });
  h += `<span class="chip${activeCat==='profit'?' on':''}" onclick="setCat('profit',this)">📈 En hausse</span>`;
  h += `<span class="chip${activeCat==='loss'?' on':''}" onclick="setCat('loss',this)">📉 En baisse</span>`;
  h += `<span class="chip${activeCat==='graded'?' on':''}" onclick="setCat('graded',this)">🏆 Gradées</span>`;
  h += `<span class="chip${activeCat==='CHASE'?' on':''}" style="border-color:var(--gold);color:var(--gold)" onclick="setCat('CHASE',this)">🎯 Chase Cards</span>`;
  h += `<span class="chip chip-add" onclick="addCategory()">＋ Catégorie</span>`;
  catRow.innerHTML = h;

  const locRow = document.getElementById('chipsRowLoc');
  locRow.innerHTML = `
    <span class="chip${activeLoc==='all'?' on':''}" onclick="setLoc('all',this)">Toutes localisations</span>
    <span class="chip${activeLoc==='maison'?' on':''}" onclick="setLoc('maison',this)">🏠 Chez moi</span>
    <span class="chip${activeLoc==='banque'?' on':''}" onclick="setLoc('banque',this)">🏦 En banque</span>
  `;
}

function addCategory() {
  const name = prompt('Nom de la nouvelle catégorie (ex: "Promo Center", "Chromatiques"...) :');
  if (!name || !name.trim()) return;
  const label = name.trim();
  let id = label.toUpperCase().normalize('NFD').replace(/[̀-ͯ]/g,'').replace(/[^A-Z0-9]+/g,'_').replace(/^_+|_+$/g,'');
  if (!id) id = 'CAT_' + Date.now();
  if (CATEGORIES.find(c=>c.id===id)) { toast('⚠ Cette catégorie existe déjà'); return; }
  CATEGORIES.push({id, label});
  saveData();
  renderChips();
  populateCategorySelects();
  toast(`✓ Catégorie "${label}" ajoutée`);
}

function populateCategorySelects() {
  const opts = CATEGORIES.map(c=>`<option value="${c.id}">${c.label}</option>`).join('');
  ['f-cat','moveCat'].forEach(id => {
    const sel = document.getElementById(id);
    if (!sel) return;
    const cur = sel.value;
    sel.innerHTML = opts;
    if (cur && CATEGORIES.find(c=>c.id===cur)) sel.value = cur;
  });
}

// ════════════════════════════════════════════════════
// VIEW TOGGLE
// ════════════════════════════════════════════════════
function setView(v) {
  viewMode = v;
  document.getElementById('vg').classList.toggle('on', v==='grid');
  document.getElementById('vl').classList.toggle('on', v==='list');
  document.getElementById('gridView').style.display = v==='grid' ? 'grid' : 'none';
  document.getElementById('listView').style.display = v==='list' ? 'block' : 'none';
  renderView();
}

function renderView() {
  if (viewMode==='grid') renderGrid();
  else renderList();
  renderPag();
}

// ════════════════════════════════════════════════════
// GRID
// ════════════════════════════════════════════════════
function renderGrid() {
  const page = filtered.slice((curPage-1)*PER, curPage*PER);
  const el = document.getElementById('gridView');
  if (!page.length) { el.innerHTML = '<div class="empty-state" style="grid-column:1/-1"><div class="e-ico">🔍</div><p>Aucune carte trouvée</p></div>'; return; }
  el.innerHTML = page.map((c,i) => {
    const p = latestP(c), ch = chgTotal(c), r = roi(c);
    const isChase = c.cat==='CHASE';
    const deltaStr = (ch!==null && !isChase) ? `<span class="tile-delta ${deltaCls(ch)}">${deltaIcon(ch)} ${Math.abs(ch).toFixed(0)}%</span>` : '';
    const badge = isChase ? '<span class="tile-badge" style="background:var(--gold);color:#000">🎯 CHASE</span>' : c.invest===null ? '<span class="tile-badge badge-pack">PACKÉ</span>' : c.graded ? `<span class="tile-badge badge-grade">${c.graded}</span>` : '';
    const roiStr = (r!==null && !isChase) ? `<span class="card-roi ${pillCls(r)}" style="font-size:9px">${fPct(r)} ROI</span>` : '';
    const locIcon = c.loc==='banque' ? '🏦' : '🏠';
    return `<div class="card-tile${isChase?' chase-tile':''}" style="animation-delay:${(i%PER)*0.025}s" onclick="openModal(${c.id})">
      <div class="card-img-wrap">
        ${c.imgUrl ? `<img src="${c.imgUrl}" alt="${c.name}" loading="lazy" onerror="this.style.display='none';this.nextElementSibling.style.display='flex'">` : ''}
        <div class="card-img-placeholder" style="${c.imgUrl?'display:none':''}">
          <div class="ball">⚡</div>
          <div style="font-size:9px;padding:0 8px;text-align:center">${c.name}</div>
        </div>
        ${badge}${deltaStr}
        <span class="loc-pill" title="${c.loc==='banque'?'En banque':'Chez moi'}">${locIcon}</span>
      </div>
      <div class="card-foot">
        <div class="card-name">${c.name}</div>
        <div class="card-ref">${c.ref} · ${c.set}</div>
        <div class="card-price-row">
          <span class="card-price">${p ? Math.round(p)+' €' : '—'}</span>
          ${roiStr}
        </div>
      </div>
    </div>`;
  }).join('');
}

// ════════════════════════════════════════════════════
// LIST
// ════════════════════════════════════════════════════
function renderList() {
  const page = filtered.slice((curPage-1)*PER, curPage*PER);
  const el = document.getElementById('tblBody');
  if (!page.length) { el.innerHTML = '<tr><td colspan="8" class="empty-state"><div class="e-ico">🔍</div><p>Aucune carte trouvée</p></td></tr>'; return; }
  el.innerHTML = page.map((c,i) => {
    const p=latestP(c), r=roi(c), ch=chgTotal(c);
    const isChase = c.cat==='CHASE';
    const thumb = c.imgUrl ? `<img class="t-thumb" src="${c.imgUrl}" alt="${c.name}" onerror="this.style.background='var(--lift)';this.src=''">` : `<div class="t-thumb" style="display:flex;align-items:center;justify-content:center;font-size:18px;opacity:.2">⚡</div>`;
    const invest = isChase ? '<span class="pill" style="background:rgba(245,200,66,.15);color:var(--gold)">🎯 CHASE</span>' : c.invest===null ? '<span class="pill pill-gold">PACKÉ</span>' : fEur(c.invest);
    const graded = c.graded ? `<span class="pill" style="background:rgba(68,102,255,.15);color:#88aaff">${c.graded}</span>` : '—';
    const locIcon = c.loc==='banque' ? '🏦 Banque' : '🏠 Maison';
    return `<tr style="animation-delay:${(i%PER)*0.02}s" onclick="openModal(${c.id})">
      <td>${thumb}</td>
      <td><div class="t-name">${c.name}</div><div class="t-sub">${c.ref} · ${c.set} · ${catLabel(c.cat)} · ${locIcon}</div></td>
      <td>${invest}</td>
      <td class="t-price">${p?Math.round(p)+' €':'—'}</td>
      <td class="hm">${(r!==null&&!isChase)?`<span class="pill ${pillCls(r)}">${fPct(r)}</span>`:'—'}</td>
      <td class="hm">${(ch!==null&&!isChase)?`<span class="pill ${pillCls(ch)}">${fPct(ch)}</span>`:'—'}</td>
      <td class="hm">${graded}</td>
      <td><a href="${cmUrl(c)}" target="_blank" onclick="event.stopPropagation()" style="color:var(--blue);font-size:10px;text-decoration:none;white-space:nowrap">CM NM FR ↗</a></td>
    </tr>`;
  }).join('');
}

// ════════════════════════════════════════════════════
// PAGINATION
// ════════════════════════════════════════════════════
function renderPag() {
  const total = filtered.length, pages = Math.ceil(total/PER);
  const el = document.getElementById('pagBar');
  if (pages<=1) { el.innerHTML=''; return; }
  const start = (curPage-1)*PER;
  let h = `<span class="pinfo">${start+1}–${Math.min(curPage*PER,total)} / ${total}</span>`;
  for (let i=1;i<=pages;i++) h+=`<button class="ppage${i===curPage?' on':''}" onclick="goPage(${i})">${i}</button>`;
  el.innerHTML = h;
}
function goPage(p) { curPage=p; renderView(); document.querySelector('.main').scrollTo({top:0,behavior:'smooth'}); }

// ════════════════════════════════════════════════════
// MODAL
// ════════════════════════════════════════════════════
function openModal(id) {
  openCardId = id;
  const c = COLLECTION.find(x=>x.id===id);
  if (!c) return;
  document.getElementById('mTitle').textContent = c.name + (c.graded ? ` [${c.graded}]` : '');
  const locTxt = c.loc==='banque' ? '🏦 En banque' : '🏠 Chez moi';
  const investTxt = c.cat==='CHASE' ? ' · 🎯 Chase Card (recherche)' : (c.invest?' · Acheté '+fEur(c.invest):' · PACKÉ');
  document.getElementById('mRef').textContent = `${c.ref} · ${c.set} · ${catLabel(c.cat)} · ${locTxt}${investTxt}`;
  // Move selects
  populateCategorySelects();
  document.getElementById('moveCat').value = c.cat;
  document.getElementById('moveLoc').value = c.loc || 'maison';
  // Image
  const img = document.getElementById('modalImg');
  const ph  = document.getElementById('modalImgPlaceholder');
  document.getElementById('modalImgPlaceholderName').textContent = c.name;
  document.getElementById('imgUrlInp').value = c.imgUrl || '';
  if (c.imgUrl) {
    img.src = c.imgUrl; img.style.display='block'; ph.style.display='none';
    img.onerror = () => { img.style.display='none'; ph.style.display='flex'; };
  } else { img.style.display='none'; ph.style.display='flex'; }
  // Prices
  renderModalPrices(c);
  // Chart
  buildMiniChart(c);
  // Actions
  document.getElementById('mActions').innerHTML = `
    <a class="btn btn-blue" href="${cmUrl(c)}" target="_blank">Voir sur Cardmarket (NM · FR) ↗</a>
    ${chgTotal(c)!==null?`<span class="pill ${pillCls(chgTotal(c))}" style="padding:6px 14px;font-size:11px">Δ Total ${fPct(chgTotal(c))}</span>`:''}
    ${roi(c)!==null?`<span class="pill ${pillCls(roi(c))}" style="padding:6px 14px;font-size:11px">ROI ${fPct(roi(c))}</span>`:''}
    <button class="btn btn-danger" onclick="deleteCard(${c.id})" style="margin-left:auto">🗑 Supprimer</button>
  `;
  // Set today date in new price input
  document.getElementById('newPriceDate').value = todayStr();
  document.getElementById('newPriceVal').value = '';
  document.getElementById('overlay').classList.add('open');
  document.body.style.overflow = 'hidden';
}

function renderModalPrices(c, flashDate) {
  const dates = sortedDates(c.prices);
  document.getElementById('mPrices').innerHTML = dates.map((d,i) => {
    const v = c.prices[d];
    const prev = i>0 ? c.prices[dates[i-1]] : null;
    const delta = prev ? (v-prev)/prev*100 : null;
    return `<div class="ph-item editable${d===flashDate?' flash':''}" onclick="editPriceInline(this,'${d}',${v},${c.id})">
      <div class="ph-date">${d}</div>
      <div class="ph-val">${Math.round(v)} €</div>
      ${delta!==null?`<div class="ph-delta" style="color:var(--${delta>=0?'teal':'pink'})">${deltaIcon(delta)} ${Math.abs(delta).toFixed(1)}%</div>`:''}
      <div class="ph-edit-hint">✎ cliquer pour modifier</div>
    </div>`;
  }).join('');
}

// ════════════════════════════════════════════════════
// MOVE CARD (catégorie / localisation)
// ════════════════════════════════════════════════════
function applyMove() {
  const c = COLLECTION.find(x=>x.id===openCardId);
  if (!c) return;
  const newCat = document.getElementById('moveCat').value;
  const newLoc = document.getElementById('moveLoc').value;
  c.cat = newCat;
  c.loc = newLoc;
  saveData();
  applyFilters();
  renderSidebarStats();
  document.getElementById('mRef').textContent = `${c.ref} · ${c.set} · ${catLabel(c.cat)} · ${c.loc==='banque'?'🏦 En banque':'🏠 Chez moi'}${c.cat==='CHASE'?' · 🎯 Chase Card (recherche)':(c.invest?' · Acheté '+fEur(c.invest):' · PACKÉ')}`;
  toast(`✓ ${c.name} déplacée → ${catLabel(newCat)} / ${newLoc==='banque'?'Banque':'Maison'}`);
}

function editPriceInline(el, date, currentVal, cardId) {
  if (el.classList.contains('editing')) return;
  el.classList.add('editing');
  el.innerHTML = `
    <div class="ph-date">${date}</div>
    <input class="ph-edit-inp" type="number" step="0.01" value="${currentVal}" id="ei_${date.replace(/\//g,'_')}"
      onkeydown="if(event.key==='Enter')savePriceEdit('${date}',${cardId},this);if(event.key==='Escape')cancelEdit(this)">
    <div style="font-size:9px;color:var(--dim);margin-top:4px">↵ Enregistrer · Esc Annuler</div>
  `;
  const inp = el.querySelector('input');
  inp.focus(); inp.select();
}

function savePriceEdit(date, cardId, inp) {
  const val = parseFloat(inp.value);
  if (!val || isNaN(val) || val<=0) { toast('⚠ Valeur invalide'); return; }
  const c = COLLECTION.find(x=>x.id===cardId);
  if (!c) return;
  c.prices[date] = val;
  saveData();
  renderModalPrices(c, date);
  buildMiniChart(c);
  renderView();
  renderSidebarStats();
  toast(`✓ Prix du ${date} mis à jour : ${Math.round(val)} €`);
}

function cancelEdit(inp) {
  const c = COLLECTION.find(x=>x.id===openCardId);
  if (c) renderModalPrices(c);
}

function addPriceEntry() {
  const date = document.getElementById('newPriceDate').value.trim();
  const val  = parseFloat(document.getElementById('newPriceVal').value);
  if (!date || !val || isNaN(val) || val<=0) { toast('⚠ Date et prix requis'); return; }
  const c = COLLECTION.find(x=>x.id===openCardId);
  if (!c) return;
  c.prices[date] = val;
  saveData();
  renderModalPrices(c, date);
  buildMiniChart(c);
  renderView();
  renderSidebarStats();
  document.getElementById('newPriceVal').value = '';
  toast(`✓ Prix ${Math.round(val)} € enregistré pour le ${date}`);
}

function showImgPlaceholder() {
  document.getElementById('modalImg').style.display='none';
  document.getElementById('modalImgPlaceholder').style.display='flex';
}

function applyImgUrl() {
  const url = document.getElementById('imgUrlInp').value.trim();
  const c = COLLECTION.find(x=>x.id===openCardId);
  if (!c) return;
  c.imgUrl = url || null;
  saveData();
  const img=document.getElementById('modalImg'), ph=document.getElementById('modalImgPlaceholder');
  if (url) { img.src=url; img.style.display='block'; ph.style.display='none'; img.onerror=()=>{img.style.display='none';ph.style.display='flex';}; }
  else { img.style.display='none'; ph.style.display='flex'; }
  renderView();
  toast('✓ Image mise à jour');
}

function deleteCard(id) {
  if (!confirm('Supprimer cette carte de la collection ?')) return;
  COLLECTION = COLLECTION.filter(x=>x.id!==id);
  saveData(); closeModal();
  applyFilters(); renderSidebarStats();
  toast('Carte supprimée');
}

function buildMiniChart(c) {
  const dates0 = sortedDates(c.prices);
  document.getElementById('miniChartSection').style.display = dates0.length ? 'block' : 'none';
  if (!dates0.length) { if (miniChartObj) { miniChartObj.destroy(); miniChartObj=null; } return; }
  const ctx = document.getElementById('miniChart').getContext('2d');
  if (miniChartObj) miniChartObj.destroy();
  const dates = sortedDates(c.prices);
  const vals  = dates.map(d=>c.prices[d]);
  const color = vals[vals.length-1] >= vals[0] ? '#00d4aa' : '#ff4488';
  miniChartObj = new Chart(ctx, {
    type:'line',
    data:{ labels:dates, datasets:[{ data:vals, borderColor:color,
      backgroundColor: color==='#00d4aa'?'rgba(0,212,170,0.07)':'rgba(255,68,136,0.07)',
      fill:true, tension:0.4, pointBackgroundColor:color, pointRadius:4, borderWidth:2 }]},
    options:{ responsive:true, maintainAspectRatio:false,
      plugins:{ legend:{display:false}, tooltip:{ backgroundColor:'#1c1c2e', borderColor:'#2a2a42', borderWidth:1,
        titleColor:'#f5c842', bodyColor:'#e8e8f8',
        callbacks:{ label: ctx => fEur(ctx.parsed.y) } } },
      scales:{ x:{ ticks:{color:'#44446a',font:{family:'DM Mono',size:9}}, grid:{color:'rgba(42,42,66,.4)'}},
               y:{ ticks:{color:'#44446a',font:{family:'DM Mono',size:9},callback:v=>Math.round(v)+'€'}, grid:{color:'rgba(42,42,66,.4)'}}}}
  });
}

function closeModal() {
  document.getElementById('overlay').classList.remove('open');
  document.body.style.overflow='';
  openCardId=null;
}
function maybeCloseModal(e) { if(e.target===document.getElementById('overlay')) closeModal(); }

// ════════════════════════════════════════════════════
// CHARTS TAB
// ════════════════════════════════════════════════════
function buildMainChart() {
  const ctx = document.getElementById('mainChart').getContext('2d');
  if (mainChartObj) mainChartObj.destroy();
  const allDates = new Set();
  COLLECTION.forEach(c=>Object.keys(c.prices).forEach(d=>allDates.add(d)));
  const dates = Array.from(allDates).sort((a,b)=>{ const [da,ma,ya]=a.split('/');const [db,mb,yb]=b.split('/'); return new Date(+('20'+ya),+ma-1,+da)-new Date(+('20'+yb),+mb-1,+db); });

  let data;
  if (chartMode==='total') {
    const totals = dates.map(d=>{ let s=0; COLLECTION.forEach(c=>{if(c.prices[d])s+=c.prices[d];}); return s; });
    data={labels:dates,datasets:[{label:'Valeur totale (€)',data:totals,borderColor:'#f5c842',backgroundColor:'rgba(245,200,66,0.06)',fill:true,tension:0.4,pointBackgroundColor:'#f5c842',pointRadius:5,borderWidth:2}]};
  } else if (chartMode==='top10') {
    const top=[...COLLECTION].sort((a,b)=>(latestP(b)||0)-(latestP(a)||0)).slice(0,10);
    const cols=['#f5c842','#4466ff','#00d4aa','#ff4488','#ff8c00','#00cfff','#cc44ff','#88ff66','#ff66aa','#66aaff'];
    data={labels:dates,datasets:top.map((c,i)=>({label:c.name,data:dates.map(d=>c.prices[d]||null),borderColor:cols[i],backgroundColor:'transparent',tension:0.4,pointRadius:3,borderWidth:2,spanGaps:true}))};
  } else {
    const wr=[...COLLECTION].filter(c=>c.invest&&latestP(c)).map(c=>({name:c.name.slice(0,14),r:roi(c)})).sort((a,b)=>b.r-a.r).slice(0,20);
    data={labels:wr.map(x=>x.name),datasets:[{label:'ROI %',data:wr.map(x=>x.r),backgroundColor:wr.map(x=>x.r>=0?'rgba(0,212,170,0.55)':'rgba(255,68,136,0.55)'),borderColor:wr.map(x=>x.r>=0?'#00d4aa':'#ff4488'),borderWidth:1}]};
  }
  const isBar = chartMode==='roi';
  mainChartObj = new Chart(ctx,{type:isBar?'bar':'line',data,options:{
    responsive:true,maintainAspectRatio:false,
    plugins:{legend:{labels:{color:'#7777aa',font:{family:'DM Mono',size:10},boxWidth:10},display:chartMode!=='total'},
      tooltip:{backgroundColor:'#1c1c2e',borderColor:'#2a2a42',borderWidth:1,titleColor:'#f5c842',bodyColor:'#e8e8f8',callbacks:{label:ctx=>isBar?fPct(ctx.parsed.y):fEur(ctx.parsed.y)}}},
    scales:{x:{ticks:{color:'#44446a',font:{family:'DM Mono',size:9}},grid:{color:'rgba(42,42,66,.5)'}},
      y:{ticks:{color:'#44446a',font:{family:'DM Mono',size:9},callback:v=>isBar?v.toFixed(0)+'%':Math.round(v).toLocaleString('fr-FR')+'€'},grid:{color:'rgba(42,42,66,.5)'}}}
  }});

  // Top 10 bar
  const ctx2=document.getElementById('top10Chart').getContext('2d');
  if (top10ChartObj) top10ChartObj.destroy();
  const top10=[...COLLECTION].sort((a,b)=>(latestP(b)||0)-(latestP(a)||0)).slice(0,10);
  top10ChartObj=new Chart(ctx2,{type:'bar',data:{
    labels:top10.map(c=>c.name.slice(0,16)),
    datasets:[{label:'Valeur (€)',data:top10.map(c=>latestP(c)||0),backgroundColor:'rgba(245,200,66,0.4)',borderColor:'#f5c842',borderWidth:1}]
  },options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},
    scales:{x:{ticks:{color:'#44446a',font:{family:'DM Mono',size:9}},grid:{color:'rgba(42,42,66,.5)'}},
      y:{ticks:{color:'#44446a',font:{family:'DM Mono',size:9},callback:v=>Math.round(v)+'€'},grid:{color:'rgba(42,42,66,.5)'}}}}});
}

function switchChart(m, el) {
  chartMode=m;
  document.querySelectorAll('.chart-btns .chip').forEach(b=>b.classList.remove('on'));
  el.classList.add('on');
  buildMainChart();
}

// ════════════════════════════════════════════════════
// TABS
// ════════════════════════════════════════════════════
function showSection(s) {
  document.getElementById('collectionSection').style.display='none';
  document.getElementById('chartSection').style.display='none';
  document.getElementById('addSection').style.display='none';
  document.querySelectorAll('.nav-item').forEach(el=>el.classList.remove('active'));
  document.getElementById('chipsBar').style.display='none';
  document.getElementById('sortbar') && (document.querySelector('.sortbar').style.display='none');
  // Sync bottom nav
  setActiveBottomNav(s);
  if (s==='collection') {
    document.getElementById('collectionSection').style.display='block';
    document.getElementById('chipsBar').style.display='flex';
    document.querySelector('.sortbar').style.display='flex';
    document.getElementById('nav-collection').classList.add('active');
    document.getElementById('topbarTitle').textContent='COLLECTION';
    applyFilters();
  } else if (s==='charts') {
    document.getElementById('chartSection').style.display='block';
    document.getElementById('nav-charts').classList.add('active');
    document.getElementById('topbarTitle').textContent='GRAPHIQUES';
    setTimeout(buildMainChart, 80);
  } else if (s==='add') {
    document.getElementById('addSection').style.display='block';
    document.getElementById('nav-add').classList.add('active');
    document.getElementById('topbarTitle').textContent='AJOUTER UNE CARTE';
  }
}

// ════════════════════════════════════════════════════
// ADD CARD FORM
// ════════════════════════════════════════════════════
function todayStr() {
  const d=new Date(); return `${String(d.getDate()).padStart(2,'0')}/${String(d.getMonth()+1).padStart(2,'0')}/${String(d.getFullYear()).slice(2)}`;
}

function onFormChange() {
  const name=document.getElementById('f-name').value.trim();
  const price=parseFloat(document.getElementById('f-price').value)||null;
  const ref=document.getElementById('f-ref').value.trim();
  const set=document.getElementById('f-set').value.trim();
  const imgUrl=document.getElementById('f-imgurl').value.trim();
  const cat=document.getElementById('f-cat').value;
  const lbl=document.getElementById('f-price-label');
  if (lbl) lbl.textContent = cat==='CHASE' ? 'Prix cible / estimé (€) — optionnel' : 'Prix NM France actuel (€) *';
  const preview=document.getElementById('formPreview');
  if (!name) { preview.style.display='none'; return; }
  preview.style.display='block';
  document.getElementById('previewName').textContent=name;
  document.getElementById('previewSub').textContent=`${ref||'—'} · ${set||'—'}`;
  document.getElementById('previewPrice').textContent=price?Math.round(price)+' €':'—';
  const pImg=document.getElementById('previewImg');
  if (imgUrl) { pImg.src=imgUrl; pImg.style.display='block'; pImg.onerror=()=>pImg.style.display='none'; }
  else pImg.style.display='none';
}

function addCard() {
  const name=document.getElementById('f-name').value.trim();
  const ref=document.getElementById('f-ref').value.trim()||'—';
  const set=document.getElementById('f-set').value.trim()||'—';
  const cat=document.getElementById('f-cat').value;
  const loc=document.getElementById('f-loc').value;
  const invest=parseFloat(document.getElementById('f-invest').value)||null;
  const price=parseFloat(document.getElementById('f-price').value)||null;
  const grade=document.getElementById('f-grade').value.trim()||null;
  const date=document.getElementById('f-date').value.trim()||todayStr();
  const imgUrl=document.getElementById('f-imgurl').value.trim()||null;
  if (!name) { toast('⚠ Le nom est requis'); return; }
  if (cat!=='CHASE' && !price) { toast('⚠ Nom et prix actuel requis'); return; }
  const prices = price ? {[date]:price} : {};
  const card={id:nextId++,name,ref,set,cat,loc,invest,prices,graded:grade,imgUrl};
  COLLECTION.unshift(card);
  saveData();
  renderSidebarStats();
  applyFilters();
  document.getElementById('addOk').style.display='block';
  toast(`✓ ${name} ajoutée !`);
  setTimeout(()=>{ document.getElementById('addOk').style.display='none'; clearAddForm(); showSection('collection'); },1800);
}

function clearAddForm() {
  ['f-name','f-ref','f-set','f-invest','f-price','f-grade','f-imgurl'].forEach(id=>document.getElementById(id).value='');
  document.getElementById('f-date').value=todayStr();
  populateCategorySelects();
  document.getElementById('f-cat').value='SAR';
  document.getElementById('f-loc').value='maison';
  document.getElementById('formPreview').style.display='none';
  document.getElementById('addOk').style.display='none';
  onFormChange();
}

// ════════════════════════════════════════════════════
// MOBILE BOTTOM NAV
// ════════════════════════════════════════════════════
function setActiveBottomNav(section) {
  document.querySelectorAll('.bnav-item').forEach(el => el.classList.remove('active'));
  const el = document.getElementById('bnav-' + section);
  if (el) el.classList.add('active');
}

// ════════════════════════════════════════════════════
// REFRESH (simulated — for real CM API needs OAuth)
// ════════════════════════════════════════════════════
function doRefresh() {
  const btn=document.getElementById('refreshBtn');
  const ico=document.getElementById('refreshIco');
  btn.classList.add('spin-anim');
  btn.disabled=true;
  if (ico) { ico.style.display='inline-block'; ico.style.animation='rotate 1s linear infinite'; }
  // Simulate network call (2s)
  setTimeout(()=>{
    btn.classList.remove('spin-anim');
    btn.disabled=false;
    if (ico) { ico.style.animation=''; }
    toast('✓ Pour actualiser les prix en temps réel, connecte l\'API Cardmarket avec tes identifiants développeur. Entre-temps, clique sur une carte → bouton "Enregistrer" pour mettre à jour manuellement.');
  },1500);
}

// ════════════════════════════════════════════════════
// TOAST
// ════════════════════════════════════════════════════
function toast(msg, dur=4000) {
  const el=document.getElementById('toast');
  el.textContent=msg; el.classList.add('show');
  clearTimeout(el._t);
  el._t=setTimeout(()=>el.classList.remove('show'),dur);
}

// ════════════════════════════════════════════════════
// INIT
// ════════════════════════════════════════════════════
document.addEventListener('DOMContentLoaded', () => {
  loadData();
  renderChips();
  populateCategorySelects();
  document.getElementById('f-date').value = todayStr();
  document.getElementById('f-cat').value = 'SAR';
  document.getElementById('f-loc').value = 'maison';
  renderSidebarStats();
  showSection('collection');
});
</script>
</body>
</html>
