<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>لوحة متابعة مهام العملاء</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;500;600;700;800;900&family=Tajawal:wght@400;500;700;900&display=swap" rel="stylesheet">
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  input, button { font-family: inherit; }

  :root {
    --primary: #0c3b5e;
    --primary-light: #14588a;
    --primary-lighter: #1c74b3;
    --teal: #0d9488;
    --success: #16a34a;
    --success-bg: #ecfdf5;
    --warning: #d97706;
    --danger: #dc2626;
    --danger-bg: #fef2f2;
    --neutral: #94a3b8;
    --bg: #eef1f6;
    --card-bg: #ffffff;
    --border: #e2e8f0;
    --text-dark: #1e293b;
    --text-muted: #64748b;
    --text-light: #94a3b8;
    --radius-lg: 18px;
    --radius-md: 12px;
    --radius-sm: 8px;
    --shadow-sm: 0 1px 3px rgba(15,23,42,0.06), 0 1px 2px rgba(15,23,42,0.04);
    --shadow-md: 0 10px 22px -8px rgba(15,23,42,0.14), 0 4px 8px -4px rgba(15,23,42,0.06);
    --shadow-lg: 0 20px 40px -14px rgba(15,23,42,0.22);
  }

  html, body { direction: rtl; }
  body {
    font-family: Tahoma, 'Segoe UI', Arial, sans-serif;
    background: var(--bg);
    color: var(--text-dark);
    min-height: 100vh;
    padding: 28px 24px 60px;
  }

  .container { max-width: 1200px; margin: 0 auto; }

  /* Header */
  .dashboard-header {
    display: flex; justify-content: space-between; align-items: center; gap: 20px;
    background: linear-gradient(135deg, var(--primary) 0%, var(--primary-lighter) 100%);
    color: #fff; padding: 28px 32px; border-radius: var(--radius-lg);
    box-shadow: var(--shadow-lg); margin-bottom: 26px; flex-wrap: wrap;
  }
  .header-text h1 { font-size: 23px; font-weight: 700; margin-bottom: 6px; }
  .header-text p { font-size: 13.5px; opacity: 0.85; }
  .report-date { font-size: 12.5px; opacity: 0.8; margin-top: 8px; }
  .save-note { font-size: 11.5px; opacity: 0.65; margin-top: 2px; }

  .header-brand { display: flex; align-items: center; gap: 18px; }
  .header-logo {
    background: #fff; border-radius: var(--radius-md); padding: 10px 16px;
    display: flex; align-items: center; justify-content: center; flex-shrink: 0;
    box-shadow: 0 4px 14px rgba(0,0,0,0.18);
  }
  .header-logo img { display: block; height: 52px; width: auto; max-width: 170px; object-fit: contain; }

  .download-btn {
    display: flex; align-items: center; gap: 8px; background: #fff; color: var(--primary);
    border: none; padding: 13px 24px; border-radius: 100px; font-size: 14.5px; font-weight: 700;
    cursor: pointer; box-shadow: 0 4px 14px rgba(0,0,0,0.18);
    transition: transform 0.15s ease, box-shadow 0.15s ease; white-space: nowrap;
  }
  .download-btn:hover:not(:disabled) { transform: translateY(-2px); box-shadow: 0 8px 20px rgba(0,0,0,0.24); }
  .download-btn:disabled { opacity: 0.75; cursor: not-allowed; }
  .download-btn svg { width: 18px; height: 18px; flex-shrink: 0; }

  /* Overview */
  .overview {
    display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 16px; margin-bottom: 26px;
  }
  .stat-card {
    background: var(--card-bg); border-radius: var(--radius-md); padding: 20px 22px;
    box-shadow: var(--shadow-sm); border: 1px solid var(--border);
    display: flex; align-items: center; gap: 16px;
  }
  .stat-icon {
    width: 46px; height: 46px; border-radius: 12px; display: flex; align-items: center;
    justify-content: center; flex-shrink: 0;
  }
  .stat-icon svg { width: 23px; height: 23px; }
  .stat-icon.clients { background: #e0f2fe; color: #0369a1; }
  .stat-icon.tasks { background: #ede9fe; color: #6d28d9; }
  .stat-icon.done { background: #dcfce7; color: #15803d; }
  .stat-icon.visits { background: #fce7f3; color: #be185d; }
  .stat-info { display: flex; flex-direction: column; gap: 3px; }
  .stat-value { font-size: 24px; font-weight: 800; line-height: 1; }
  .stat-label { font-size: 12.5px; color: var(--text-muted); font-weight: 600; }

  .stat-card.hero { background: linear-gradient(135deg, var(--teal) 0%, #0891b2 100%); border: none; color: #fff; }
  .hero-title { font-size: 14.5px; font-weight: 700; color: #fff; }
  .hero-subtitle { font-size: 12.5px; color: rgba(255,255,255,0.85); font-weight: 600; margin-top: 3px; display: block; }

  .client-ring, .hero-ring { position: relative; flex-shrink: 0; }
  .client-ring { width: 56px; height: 56px; }
  .hero-ring { width: 64px; height: 64px; }
  .client-ring svg, .hero-ring svg { width: 100%; height: 100%; transform: rotate(-90deg); display: block; }
  .client-ring circle, .hero-ring circle { fill: none; }
  .client-ring .ring-bg { stroke: #eef1f6; }
  .hero-ring .ring-bg { stroke: rgba(255,255,255,0.35); }
  .ring-text {
    position: absolute; inset: 0; display: flex; align-items: center; justify-content: center;
    font-weight: 800;
  }
  .client-ring .ring-text { font-size: 12.5px; }
  .hero-ring .ring-text { font-size: 14px; }

  /* Comparison */
  .comparison-card {
    background: var(--card-bg); border-radius: var(--radius-md); padding: 22px 24px;
    box-shadow: var(--shadow-sm); border: 1px solid var(--border); margin-bottom: 26px;
  }
  .comparison-title { font-size: 15.5px; font-weight: 700; margin-bottom: 16px; }
  .compare-row { display: flex; align-items: center; gap: 14px; margin-bottom: 13px; }
  .compare-row:last-child { margin-bottom: 0; }
  .compare-name {
    width: 190px; flex-shrink: 0; font-size: 13.5px; font-weight: 600;
    white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
  }
  .compare-track { flex: 1; height: 11px; background: #eef1f6; border-radius: 100px; overflow: hidden; }
  .compare-fill { height: 100%; border-radius: 100px; }
  .compare-percent { width: 42px; flex-shrink: 0; text-align: left; font-size: 13px; font-weight: 700; }

  /* Add client */
  .add-client-bar { display: flex; gap: 10px; margin-bottom: 22px; flex-wrap: wrap; }
  .add-client-bar input {
    flex: 1; min-width: 220px; padding: 13px 16px; border-radius: var(--radius-sm);
    border: 1.5px solid var(--border); font-size: 14px; background: #fff; direction: rtl;
  }
  .add-client-bar input:focus { outline: none; border-color: var(--primary-lighter); }
  .add-client-bar button {
    background: var(--primary); color: #fff; border: none; padding: 13px 22px;
    border-radius: var(--radius-sm); font-size: 14px; font-weight: 700; cursor: pointer;
    white-space: nowrap; transition: background 0.15s ease;
  }
  .add-client-bar button:hover { background: var(--primary-light); }

  /* Clients grid */
  .clients-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(330px, 1fr)); gap: 20px; }
  .client-card {
    background: var(--card-bg); border-radius: var(--radius-lg); box-shadow: var(--shadow-sm);
    border: 1px solid var(--border); overflow: hidden; display: flex; flex-direction: column;
    transition: box-shadow 0.2s ease, transform 0.2s ease;
  }
  .client-card:hover { box-shadow: var(--shadow-md); transform: translateY(-3px); }

  .client-card-header {
    display: flex; align-items: center; gap: 14px; padding: 20px 22px; border-bottom: 1px solid var(--border);
  }
  .client-info { flex: 1; min-width: 0; }
  .client-name {
    font-size: 16px; font-weight: 700; margin-bottom: 4px;
    overflow: hidden; text-overflow: ellipsis; white-space: nowrap;
  }
  .client-meta { font-size: 12px; color: var(--text-muted); font-weight: 600; }

  .delete-client-btn {
    background: none; border: none; color: var(--text-light); font-size: 19px; cursor: pointer;
    line-height: 1; padding: 5px 9px; border-radius: 8px; transition: all 0.15s ease; flex-shrink: 0;
  }
  .delete-client-btn:hover { background: var(--danger-bg); color: var(--danger); }
  .delete-client-btn.confirm-delete, .delete-task-btn.confirm-delete {
    background: var(--danger) !important; color: #fff !important; opacity: 1 !important;
  }

  .task-list { list-style: none; padding: 8px 10px; flex: 1; }
  .task-item {
    display: flex; align-items: center; gap: 8px; padding: 9px 10px; border-radius: 8px;
    transition: background 0.15s ease;
  }
  .task-item:nth-child(even) { background: #f8fafc; }
  .task-item:hover { background: #eef2f7; }
  .task-item label {
    display: flex; align-items: center; gap: 8px; cursor: pointer; min-width: 0;
    flex: 0 1 auto; max-width: 62%;
  }
  .task-item input[type=checkbox] { width: 17px; height: 17px; accent-color: var(--teal); cursor: pointer; flex-shrink: 0; }
  .task-name { font-size: 13.5px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; min-width: 0; flex: 1 1 auto; }
  .task-item.done .task-name { text-decoration: line-through; color: var(--text-light); }
  .task-leader {
    flex: 1 1 auto; min-width: 10px; border-bottom: 1.5px dotted #cbd5e1; margin-bottom: 5px; height: 0;
  }

  .delete-task-btn {
    background: none; border: none; color: var(--text-light); font-size: 16px; cursor: pointer;
    opacity: 0.65; padding: 2px 9px; border-radius: 6px; transition: all 0.15s ease; flex-shrink: 0;
  }
  .delete-task-btn:hover { opacity: 1; background: var(--danger-bg); color: var(--danger); }

  .empty-tasks { text-align: center; padding: 18px; color: var(--text-light); font-size: 13px; }

  .add-task-row { display: flex; gap: 8px; padding: 12px 16px 16px; border-top: 1px dashed var(--border); }
  .add-task-row input {
    flex: 1; min-width: 0; padding: 9px 12px; border-radius: 8px; border: 1.5px solid var(--border);
    font-size: 13px; direction: rtl;
  }
  .add-task-row input:focus { outline: none; border-color: var(--teal); }
  .add-task-row button {
    background: var(--teal); color: #fff; border: none; width: 34px; border-radius: 8px;
    font-size: 17px; cursor: pointer; font-weight: 700; flex-shrink: 0;
  }
  .add-task-row button:hover { background: #0c7c72; }
  .add-task-row select {
    padding: 9px 8px; border-radius: 8px; border: 1.5px solid var(--border);
    font-size: 12.5px; background: #fff; direction: rtl; flex-shrink: 0; color: var(--text-dark);
  }
  .add-task-row select:focus { outline: none; border-color: var(--teal); }

  /* Visits */
  .section-divider { border-top: 1px dashed var(--border); margin: 0 16px; }
  .section-label {
    font-size: 12px; font-weight: 700; color: var(--text-muted); padding: 12px 22px 4px;
    display: flex; align-items: center; justify-content: space-between;
  }
  .visit-count-badge {
    background: #fce7f3; color: #be185d; font-size: 11.5px; font-weight: 700;
    padding: 2px 10px; border-radius: 100px; white-space: nowrap;
  }
  .visit-list { list-style: none; padding: 4px 10px 6px; }
  .visit-item {
    display: flex; align-items: center; gap: 8px; padding: 8px 10px; border-radius: 8px;
    transition: background 0.15s ease;
  }
  .visit-item:nth-child(even) { background: #f8fafc; }
  .visit-item:hover { background: #eef2f7; }
  .visit-icon { flex-shrink: 0; width: 15px; height: 15px; color: #be185d; }
  .visit-name { font-size: 13px; flex: 1; min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .visit-date-badge {
    font-size: 11px; font-weight: 700; color: #be185d; background: #fce7f3;
    padding: 2px 8px; border-radius: 100px; white-space: nowrap; flex-shrink: 0;
  }
  .delete-visit-btn {
    background: none; border: none; color: var(--text-light); font-size: 16px; cursor: pointer;
    opacity: 0.65; padding: 2px 9px; border-radius: 6px; transition: all 0.15s ease; flex-shrink: 0;
  }
  .delete-visit-btn:hover { opacity: 1; background: var(--danger-bg); color: var(--danger); }
  .empty-visits { text-align: center; padding: 14px; color: var(--text-light); font-size: 12.5px; }
  .add-visit-row { display: flex; gap: 8px; padding: 8px 16px 8px; flex-wrap: wrap; }
  .add-visit-row input[type=text] {
    flex: 1; min-width: 140px; padding: 9px 12px; border-radius: 8px; border: 1.5px solid var(--border);
    font-size: 13px; direction: rtl;
  }
  .add-visit-row input[type=date] {
    padding: 9px 10px; border-radius: 8px; border: 1.5px solid var(--border);
    font-size: 12.5px; direction: ltr; color: var(--text-dark); flex-shrink: 0; width: 132px;
  }
  .add-visit-row input:focus { outline: none; border-color: #be185d; }
  .add-visit-row button {
    background: #be185d; color: #fff; border: none; width: 34px; border-radius: 8px;
    font-size: 17px; cursor: pointer; font-weight: 700; flex-shrink: 0;
  }
  .add-visit-row button:hover { background: #9d174d; }
  body.pdf-mode .add-visit-row, body.pdf-mode .delete-visit-btn { display: none !important; }
  .visit-warn-banner {
    display: none; margin: 0 16px 12px; background: #fffbeb; color: #b45309; border: 1px solid #fde68a;
    border-radius: 8px; padding: 8px 12px; font-size: 12px; font-weight: 600; line-height: 1.6;
  }
  .visit-warn-banner.show { display: block; }

  .visits-compare-card {
    background: var(--card-bg); border-radius: var(--radius-md); padding: 22px 24px;
    box-shadow: var(--shadow-sm); border: 1px solid var(--border); margin-bottom: 26px;
  }
  .visits-compare-card .compare-fill { background: #be185d; }

  /* View toggle + all-visits view */
  .view-toggle-bar { display: flex; align-items: center; gap: 10px; margin-bottom: 18px; flex-wrap: wrap; }
  body.pdf-mode .view-toggle-bar { display: none !important; }

  .visits-flat-wrap { display: flex; gap: 20px; flex-wrap: wrap; align-items: flex-start; }
  .visits-flat-main {
    flex: 2 1 380px; background: var(--card-bg); border-radius: var(--radius-md); box-shadow: var(--shadow-sm);
    border: 1px solid var(--border); overflow: hidden;
  }
  .visits-flat-side {
    flex: 1 1 260px; background: var(--card-bg); border-radius: var(--radius-md); box-shadow: var(--shadow-sm);
    border: 1px solid var(--border); padding: 18px;
  }
  .visits-flat-header {
    display: flex; align-items: center; justify-content: space-between; padding: 16px 20px;
    border-bottom: 1px solid var(--border);
  }
  .visits-flat-title { font-size: 14.5px; font-weight: 700; }
  .visits-filter-clear {
    background: none; border: 1px solid var(--border); color: var(--text-muted); font-size: 12px;
    font-weight: 700; padding: 6px 14px; border-radius: 100px; cursor: pointer;
  }
  .visits-filter-clear:hover { border-color: #be185d; color: #be185d; }
  .visits-flat-list { list-style: none; }
  .visits-flat-row {
    display: flex; align-items: center; gap: 12px; padding: 12px 20px; border-bottom: 1px solid #f1f5f9;
  }
  .visits-flat-row:last-child { border-bottom: none; }
  .visits-flat-row .visit-icon { color: #be185d; width: 16px; height: 16px; flex-shrink: 0; }
  .vf-date { width: 92px; flex-shrink: 0; font-size: 12.5px; font-weight: 700; color: #be185d; }
  .vf-name { flex: 1; min-width: 0; font-size: 13.5px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .vf-client { font-size: 11.5px; font-weight: 700; color: var(--text-muted); background: #f1f5f9;
    padding: 3px 10px; border-radius: 100px; white-space: nowrap; flex-shrink: 0; }
  .visits-flat-empty { text-align: center; padding: 40px 20px; color: var(--text-light); font-size: 13px; }

  .mini-cal-head { display: flex; align-items: center; justify-content: space-between; margin-bottom: 12px; }
  .mini-cal-title { font-size: 13.5px; font-weight: 700; }
  .mini-cal-nav { display: flex; gap: 6px; }
  .mini-cal-nav button {
    background: #f1f5f9; border: none; width: 26px; height: 26px; border-radius: 7px; cursor: pointer;
    font-size: 13px; font-weight: 700; color: var(--text-dark);
  }
  .mini-cal-nav button:hover { background: #e2e8f0; }
  .mini-cal-weekdays, .mini-cal-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: 4px; }
  .mini-cal-weekdays { margin-bottom: 6px; }
  .mini-cal-weekdays span { text-align: center; font-size: 10.5px; font-weight: 700; color: var(--text-light); }
  .mini-cal-day {
    aspect-ratio: 1; display: flex; align-items: center; justify-content: center; border-radius: 8px;
    font-size: 12px; font-weight: 600; cursor: default; color: var(--text-dark); position: relative;
  }
  .mini-cal-day.empty { visibility: hidden; }
  .mini-cal-day.has-visit { cursor: pointer; background: #fce7f3; color: #be185d; font-weight: 800; }
  .mini-cal-day.has-visit:hover { background: #fbcfe8; }
  .mini-cal-day.selected { background: #be185d !important; color: #fff !important; }
  .mini-cal-hint { font-size: 11.5px; color: var(--text-muted); margin-top: 12px; line-height: 1.7; }


  /* Priority */
  .priority-pill {
    font-size: 13px; font-weight: 700; padding: 4px 14px; border-radius: 100px;
    white-space: nowrap; flex-shrink: 0; direction: rtl; display: inline-block; line-height: 1.4;
  }
  .priority-pill.priority-high { color: var(--danger); background-color: var(--danger-bg); }
  .priority-pill.priority-medium { color: var(--warning); background-color: #fffbeb; }
  .priority-pill.priority-low { color: var(--success); background-color: var(--success-bg); }
  select.priority-pill {
    border: none; cursor: pointer; width: 84px; text-align: center; text-align-last: center;
    -webkit-appearance: none; -moz-appearance: none; appearance: none;
  }
  .priority-badge-print { display: none; }
  body.pdf-mode select.priority-pill { display: none !important; }
  body.pdf-mode .priority-badge-print { display: inline-block !important; width: 84px; text-align: center; }

  .priority-filter-bar {
    display: flex; align-items: center; gap: 10px; margin-bottom: 18px; flex-wrap: wrap;
  }
  .priority-filter-label { font-size: 13px; font-weight: 700; color: var(--text-muted); }
  .priority-filter-btn {
    background: var(--card-bg); border: 1.5px solid var(--border); color: var(--text-muted);
    padding: 8px 18px; border-radius: 100px; font-size: 13px; font-weight: 700; cursor: pointer;
    transition: all 0.15s ease;
  }
  .priority-filter-btn:hover { border-color: var(--primary-lighter); }
  .priority-filter-btn.active { color: #fff; border-color: transparent; }
  .priority-filter-btn.active[data-priority="all"] { background: var(--primary); }
  .priority-filter-btn.active[data-priority="high"] { background: var(--danger); }
  .priority-filter-btn.active[data-priority="medium"] { background: var(--warning); }
  .priority-filter-btn.active[data-priority="low"] { background: var(--success); }

  body.pdf-mode .priority-filter-bar { display: none !important; }

  .empty-state {
    grid-column: 1 / -1; text-align: center; padding: 60px 20px; color: var(--text-muted);
    background: var(--card-bg); border-radius: var(--radius-lg); border: 2px dashed var(--border);
  }
  .empty-state svg { width: 46px; height: 46px; margin-bottom: 14px; opacity: 0.4; }
  .loading-state { grid-column: 1 / -1; text-align: center; padding: 70px 20px; color: var(--text-muted); font-size: 14px; }

  .pdf-error-banner {
    display: none; background: var(--danger-bg); color: var(--danger); border: 1px solid #fecaca;
    border-radius: var(--radius-sm); padding: 12px 16px; font-size: 13px; margin-bottom: 18px;
    line-height: 1.6; word-break: break-word;
  }
  .pdf-error-banner.show { display: block; }

  body.pdf-mode .delete-client-btn,
  body.pdf-mode .delete-task-btn,
  body.pdf-mode .add-task-row,
  body.pdf-mode .add-client-bar,
  body.pdf-mode .download-btn,
  body.pdf-mode .pdf-error-banner { display: none !important; }
  body.pdf-mode .client-card:hover { transform: none; box-shadow: var(--shadow-sm); }
  body.pdf-mode #clientsGrid { grid-template-columns: 1fr; }

  @media (max-width: 640px) {
    body { padding: 18px 14px 50px; }
    .dashboard-header { padding: 22px 20px; }
    .header-text h1 { font-size: 19px; }
    .header-brand { gap: 12px; }
    .header-logo { padding: 7px 10px; }
    .header-logo img { height: 36px; }
    .compare-name { width: 110px; font-size: 12px; }
  }

  /* ==========================================================================
     ENTERPRISE PDF / PRINT REPORT
     A dedicated, print-only report is generated in #printRoot right before
     export. It never affects the live dashboard and is removed right after.
     ========================================================================== */
  #printRoot { display: none; }

  @media print {
    @page { size: A4; margin: 0; }
    html, body { background: #fff !important; }
    body * { visibility: hidden; }
    body.pdf-export-mode #printRoot,
    body.pdf-export-mode #printRoot * { visibility: visible; }
    body.pdf-export-mode #printRoot {
      display: block; position: absolute; inset: 0; margin: 0; padding: 0; width: 210mm;
    }
  }

  #printRoot, #printRoot * { box-sizing: border-box; }
  #printRoot {
    font-family: 'Cairo', 'Tajawal', Tahoma, 'Segoe UI', Arial, sans-serif;
    direction: rtl; color: #1a2942; -webkit-print-color-adjust: exact; print-color-adjust: exact;
  }

  .pp-page {
    position: relative; width: 210mm; min-height: 297mm; background: #fff;
    padding: 26mm 16mm 20mm 16mm; break-after: page; page-break-after: always;
    display: flex; flex-direction: column;
  }
  .pp-page:last-child { break-after: auto; page-break-after: auto; }

  /* Each header/footer is generated once per .pp-page and anchored to that
     specific page box (position:absolute relative to .pp-page), so every
     printed page reliably shows its own correct header/footer and page
     number — this avoids relying on the browser's print engine to repeat
     position:fixed elements per physical page, which is inconsistent across
     browsers/PDF converters and previously caused every page to visually
     show only the last page's footer stacked on top of the others. */
  .pp-header {
    position: absolute; top: 0; right: 16mm; left: 16mm; height: 16mm;
    display: flex; align-items: center; justify-content: space-between;
    padding: 0 0 3mm; border-bottom: 0.6mm solid #0c3b5e;
    background: #fff;
  }
  .pp-header-brand { display: flex; align-items: center; gap: 3mm; }
  .pp-header-brand img { height: 8mm; width: auto; object-fit: contain; }
  .pp-header-brand .pp-brand-text { font-size: 9pt; font-weight: 800; color: #0c3b5e; }
  .pp-header-title { font-size: 8pt; font-weight: 700; color: #64748b; }

  .pp-footer {
    position: absolute; bottom: 0; right: 16mm; left: 16mm; height: 12mm;
    display: flex; align-items: center; justify-content: space-between;
    padding: 3mm 0 0; border-top: 0.4mm solid #e2e8f0;
    font-size: 7.5pt; color: #94a3b8; background: #fff;
  }
  .pp-footer .pp-footer-left { font-weight: 700; color: #64748b; }
  .pp-footer .pp-page-num { font-weight: 800; color: #0c3b5e; }

  /* ---- Cover page ---- */
  .pp-cover { justify-content: flex-start; padding-top: 0; }
  .pp-cover-band {
    background: linear-gradient(135deg, #0c3b5e 0%, #1c74b3 100%);
    color: #fff; border-radius: 3mm; padding: 14mm 12mm; margin-bottom: 10mm;
    box-shadow: 0 4mm 10mm rgba(12,59,94,0.18);
  }
  .pp-cover-eyebrow { font-size: 9pt; font-weight: 700; opacity: 0.82; letter-spacing: 0.3mm; margin-bottom: 4mm; }
  .pp-cover-title { font-size: 22pt; font-weight: 900; margin-bottom: 3mm; line-height: 1.3; }
  .pp-cover-sub { font-size: 10.5pt; font-weight: 500; opacity: 0.9; margin-bottom: 8mm; }
  .pp-cover-meta-row { display: flex; gap: 10mm; font-size: 8.5pt; opacity: 0.92; flex-wrap: wrap; }
  .pp-cover-meta-row b { font-weight: 800; }

  .pp-section-title {
    font-size: 12pt; font-weight: 800; color: #0c3b5e; margin: 0 0 4mm;
    padding-bottom: 2mm; border-bottom: 0.5mm solid #e2e8f0; display: flex; align-items: center; gap: 2.5mm;
  }
  .pp-section-title .pp-dot { width: 3mm; height: 3mm; border-radius: 50%; background: #1c74b3; display: inline-block; }

  .pp-kpi-grid { display: grid; grid-template-columns: repeat(5, 1fr); gap: 4mm; margin-bottom: 8mm; }
  .pp-kpi-card {
    background: #f8fafc; border: 0.3mm solid #e2e8f0; border-radius: 2.5mm; padding: 4.5mm 3mm;
    text-align: center;
  }
  .pp-kpi-value { font-size: 15pt; font-weight: 900; color: #0c3b5e; line-height: 1.1; }
  .pp-kpi-label { font-size: 7.3pt; font-weight: 700; color: #64748b; margin-top: 1.5mm; }
  .pp-kpi-card.pp-kpi-hero { background: linear-gradient(135deg, #0d9488 0%, #0891b2 100%); border: none; }
  .pp-kpi-card.pp-kpi-hero .pp-kpi-value, .pp-kpi-card.pp-kpi-hero .pp-kpi-label { color: #fff; }

  .pp-exec-summary { font-size: 9.3pt; line-height: 1.9; color: #334155; margin-bottom: 8mm; }

  .pp-compare-block { margin-bottom: 8mm; }
  .pp-compare-row { display: flex; align-items: center; gap: 3mm; margin-bottom: 2.6mm; }
  .pp-compare-name {
    width: 42mm; flex-shrink: 0; font-size: 8pt; font-weight: 700;
    overflow: hidden; text-overflow: ellipsis; white-space: nowrap;
  }
  .pp-compare-track { flex: 1; height: 3mm; background: #eef1f6; border-radius: 2mm; overflow: hidden; }
  .pp-compare-fill { height: 100%; border-radius: 2mm; }
  .pp-compare-pct { width: 11mm; flex-shrink: 0; text-align: left; font-size: 8pt; font-weight: 800; }

  /* ---- Client detail pages ---- */
  .pp-clients-pair-grid {
    display: grid; grid-template-columns: 1fr 1fr; gap: 0 6mm; align-items: start;
  }
  .pp-clients-pair-grid .pp-client-card { margin-bottom: 0; }
  .pp-client-card {
    background: #fff; border: 0.3mm solid #dbe3ee; border-radius: 3mm; overflow: hidden;
    margin-bottom: 6mm; break-inside: avoid; page-break-inside: avoid;
  }
  .pp-client-head {
    display: flex; align-items: center; gap: 4mm; padding: 5mm 6mm;
    background: linear-gradient(135deg, #f0f6fc 0%, #f8fafc 100%); border-bottom: 0.3mm solid #dbe3ee;
  }
  .pp-client-ring { width: 15mm; height: 15mm; flex-shrink: 0; position: relative; }
  .pp-client-ring svg { width: 100%; height: 100%; transform: rotate(-90deg); }
  .pp-client-ring circle { fill: none; }
  .pp-client-ring .pp-ring-bg { stroke: #e2e8f0; }
  .pp-client-ring-text {
    position: absolute; inset: 0; display: flex; align-items: center; justify-content: center;
    font-size: 6.6pt; font-weight: 900;
  }
  .pp-client-name { font-size: 12.5pt; font-weight: 800; color: #0c3b5e; }
  .pp-client-meta { font-size: 8pt; font-weight: 600; color: #64748b; margin-top: 0.8mm; }
  .pp-client-badges { display: flex; gap: 2mm; margin-right: auto; }
  .pp-badge {
    font-size: 7.3pt; font-weight: 800; padding: 1.4mm 3.2mm; border-radius: 10mm; white-space: nowrap;
  }
  .pp-badge-visits { background: #fce7f3; color: #be185d; }

  .pp-table { width: 100%; border-collapse: collapse; }
  .pp-table th {
    font-size: 7pt; font-weight: 800; color: #64748b; text-align: right; padding: 2mm 6mm;
    background: #f8fafc; border-bottom: 0.3mm solid #e2e8f0;
  }
  .pp-table td { font-size: 8.4pt; padding: 2mm 6mm; border-bottom: 0.2mm solid #f1f5f9; vertical-align: middle; }
  .pp-table tr:last-child td { border-bottom: none; }
  .pp-task-name-done { text-decoration: line-through; color: #94a3b8; }
  .pp-status-dot { width: 2.4mm; height: 2.4mm; border-radius: 50%; display: inline-block; margin-left: 1.6mm; }
  .pp-status-dot.on { background: #16a34a; } .pp-status-dot.off { background: #cbd5e1; }
  .pp-pill { font-size: 7pt; font-weight: 800; padding: 0.9mm 2.6mm; border-radius: 10mm; white-space: nowrap; }
  .pp-pill.high { color: #dc2626; background: #fef2f2; }
  .pp-pill.medium { color: #d97706; background: #fffbeb; }
  .pp-pill.low { color: #16a34a; background: #ecfdf5; }
  .pp-empty-row td { text-align: center; color: #94a3b8; font-size: 8pt; padding: 4mm; }

  .pp-visits-title {
    font-size: 8pt; font-weight: 800; color: #64748b; padding: 3mm 6mm 1mm; display: flex;
    align-items: center; justify-content: space-between; border-top: 0.3mm dashed #e2e8f0; margin-top: 1mm;
  }
  .pp-visit-row { font-size: 8.2pt; padding: 1.4mm 6mm; color: #334155; }
  .pp-visit-row:before { content: '›'; color: #be185d; font-weight: 900; margin-left: 1.5mm; }

  @media (max-width: 640px) {
    .pp-kpi-grid { grid-template-columns: repeat(2, 1fr); }
  }
</style>
</head>
<body>
  <div class="container" id="pdfCapture">
    <header class="dashboard-header">
      <div class="header-brand">
        <div class="header-logo">
          <img src="data:image/jpeg;base64,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" alt="Assets Business Solutions">
        </div>
        <div class="header-text">
          <h1>لوحة متابعة مهام العملاء</h1>
          <p>متابعة لحظية لنسبة إنجاز المهام الخاصة بكل عميل</p>
          <div class="report-date" id="reportDate"></div>
          <div class="save-note">يتم حفظ أي تعديل تلقائيًا</div>
        </div>
      </div>
      <button class="download-btn" id="downloadBtn">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg>
        <span id="downloadBtnText">تحميل PDF</span>
      </button>
    </header>

    <div class="pdf-error-banner" id="pdfErrorBanner"></div>

    <section class="overview" id="overview"></section>

    <div class="comparison-card">
      <div class="comparison-title">مقارنة نسبة الإنجاز بين العملاء</div>
      <div id="comparisonList"></div>
    </div>

    <div class="visits-compare-card">
      <div class="comparison-title">مقارنة عدد الزيارات بين العملاء</div>
      <div id="visitsComparisonList"></div>
    </div>

    <div class="add-client-bar">
      <input type="text" id="newClientInput" placeholder="اسم عميل جديد...">
      <button id="addClientBtn">+ إضافة عميل</button>
    </div>

    <div class="view-toggle-bar" id="viewToggleBar">
      <span class="priority-filter-label">طريقة العرض:</span>
      <button class="priority-filter-btn active" data-action="set-view" data-view="clients">العملاء والمهام</button>
      <button class="priority-filter-btn" data-action="set-view" data-view="visits">كل الزيارات</button>
    </div>

    <div class="priority-filter-bar" id="priorityFilterBar">
      <span class="priority-filter-label">فلترة حسب الأولوية:</span>
      <button class="priority-filter-btn active" data-action="filter-priority" data-priority="all">الكل</button>
      <button class="priority-filter-btn" data-action="filter-priority" data-priority="high">مرتفعة</button>
      <button class="priority-filter-btn" data-action="filter-priority" data-priority="medium">متوسطة</button>
      <button class="priority-filter-btn" data-action="filter-priority" data-priority="low">منخفضة</button>
    </div>

    <section class="clients-grid" id="clientsGrid">
      <div class="loading-state">جاري تحميل البيانات...</div>
    </section>

    <section class="visits-flat-wrap" id="visitsFlatView" style="display:none"></section>
  </div>

  <div id="printRoot" aria-hidden="true"></div>

<script>
(function () {
  'use strict';

  var ICONS = {
    users: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>',
    tasks: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 11l3 3L22 4"/><path d="M21 12v7a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11"/></svg>',
    check: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="M9 12l2 2 4-4"/></svg>',
    empty: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="7" width="18" height="13" rx="2"/><path d="M16 3v4M8 3v4M3 11h18"/></svg>',
    calendar: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="18" rx="2"/><path d="M16 2v4M8 2v4M3 10h18"/></svg>'
  };

  function getDefaultClients() {
    return [];
  }

  var PRIORITY_META = {
    high: { label: 'مرتفعة', order: 0 },
    medium: { label: 'متوسطة', order: 1 },
    low: { label: 'منخفضة', order: 2 }
  };
  function normalizePriority(p) { return PRIORITY_META[p] ? p : 'medium'; }

  var state = { clients: [], nextClientId: 100, nextTaskId: 1000, nextVisitId: 5000, filterPriority: 'all' };
  var viewMode = 'clients'; // 'clients' | 'visits' — which section of the dashboard is showing
  var calendarMonthOffset = 0; // months from the current month, for the mini calendar in the visits view
  var calendarSelectedDate = null; // yyyy-mm-dd, filters the flat visits list when set
  var pendingDelete = { type: null, id: null, btn: null };
  var pendingDeleteTimer = null;

  function clearArmed() {
    if (pendingDeleteTimer) { clearTimeout(pendingDeleteTimer); pendingDeleteTimer = null; }
    pendingDelete = { type: null, id: null, btn: null };
  }
  function isArmed(type, id) { return pendingDelete.type === type && pendingDelete.id === id; }
  function armDelete(btn, type, id) {
    if (pendingDelete.btn) {
      pendingDelete.btn.textContent = '×';
      pendingDelete.btn.classList.remove('confirm-delete');
    }
    if (pendingDeleteTimer) clearTimeout(pendingDeleteTimer);
    pendingDelete = { type: type, id: id, btn: btn };
    btn.textContent = '✓';
    btn.classList.add('confirm-delete');
    pendingDeleteTimer = setTimeout(function () {
      if (pendingDelete.btn === btn) {
        btn.textContent = '×';
        btn.classList.remove('confirm-delete');
        pendingDelete = { type: null, id: null, btn: null };
      }
    }, 2500);
  }

  var LOCAL_KEY = 'client_tasks_data_v1';

  async function loadState() {
    try {
      if (window.storage) {
        var result = await window.storage.get('client_tasks_data', false);
        if (result && result.value) {
          var parsed = JSON.parse(result.value);
          state.clients = parsed.clients || [];
          state.nextClientId = parsed.nextClientId || 100;
          state.nextTaskId = parsed.nextTaskId || 1000;
          state.nextVisitId = parsed.nextVisitId || 5000;
          state.clients.forEach(function (c) {
            c.tasks.forEach(function (t) { t.priority = normalizePriority(t.priority); });
            if (!Array.isArray(c.visits)) c.visits = [];
          });
        } else {
          state.clients = getDefaultClients();
        }
      } else if (window.localStorage) {
        var raw = localStorage.getItem(LOCAL_KEY);
        if (raw) {
          var parsedLocal = JSON.parse(raw);
          state.clients = parsedLocal.clients || [];
          state.nextClientId = parsedLocal.nextClientId || 100;
          state.nextTaskId = parsedLocal.nextTaskId || 1000;
          state.nextVisitId = parsedLocal.nextVisitId || 5000;
          state.clients.forEach(function (c) {
            c.tasks.forEach(function (t) { t.priority = normalizePriority(t.priority); });
            if (!Array.isArray(c.visits)) c.visits = [];
          });
        } else {
          state.clients = getDefaultClients();
        }
      } else {
        state.clients = getDefaultClients();
      }
    } catch (e) {
      state.clients = getDefaultClients();
    }
    render();
  }

  async function saveState() {
    var payload = JSON.stringify({
      clients: state.clients,
      nextClientId: state.nextClientId,
      nextTaskId: state.nextTaskId,
      nextVisitId: state.nextVisitId
    });
    try {
      if (window.storage) {
        await window.storage.set('client_tasks_data', payload, false);
      } else if (window.localStorage) {
        localStorage.setItem(LOCAL_KEY, payload);
      }
    } catch (e) { console.error('save failed', e); }
  }

  function escapeHtml(str) {
    return String(str)
      .replace(/&/g, '&amp;')
      .replace(/</g, '&lt;')
      .replace(/>/g, '&gt;')
      .replace(/"/g, '&quot;')
      .replace(/'/g, '&#39;');
  }

  var arabicDays = ['الأحد','الاثنين','الثلاثاء','الأربعاء','الخميس','الجمعة','السبت'];
  var arabicMonths = ['يناير','فبراير','مارس','أبريل','مايو','يونيو','يوليو','أغسطس','سبتمبر','أكتوبر','نوفمبر','ديسمبر'];
  function formatArabicDate(d) {
    return 'تاريخ التقرير: ' + arabicDays[d.getDay()] + '، ' + d.getDate() + ' ' + arabicMonths[d.getMonth()] + ' ' + d.getFullYear();
  }

  function getClientStats(client) {
    var total = client.tasks.length;
    var done = client.tasks.filter(function (t) { return t.done; }).length;
    var pct = total === 0 ? 0 : Math.round((done / total) * 100);
    var visitsCount = (client.visits || []).length;
    return { total: total, done: done, pct: pct, visitsCount: visitsCount };
  }

  function getColorForPct(pct, total) {
    if (total === 0) return '#94a3b8';
    if (pct >= 100) return '#16a34a';
    if (pct >= 50) return '#d97706';
    return '#dc2626';
  }

  function renderRingSVG(pct, total, size, strokeWidth, isHero) {
    var clamped = Math.max(0, Math.min(100, Math.round(pct)));
    var r = (size - strokeWidth) / 2;
    var circumference = 2 * Math.PI * r;
    var offset = circumference * (1 - clamped / 100);
    var color = isHero ? '#ffffff' : getColorForPct(clamped, total);
    return '<svg viewBox="0 0 ' + size + ' ' + size + '">' +
      '<circle class="ring-bg" cx="' + (size/2) + '" cy="' + (size/2) + '" r="' + r + '" stroke-width="' + strokeWidth + '"></circle>' +
      '<circle cx="' + (size/2) + '" cy="' + (size/2) + '" r="' + r + '" stroke-width="' + strokeWidth + '" stroke="' + color + '" stroke-linecap="round" stroke-dasharray="' + circumference.toFixed(2) + '" stroke-dashoffset="' + offset.toFixed(2) + '"></circle>' +
      '</svg><div class="ring-text" style="color:' + color + '">' + clamped + '%</div>';
  }

  function renderOverview() {
    var totalClients = state.clients.length;
    var allTasks = state.clients.reduce(function (acc, c) { return acc.concat(c.tasks); }, []);
    var totalTasks = allTasks.length;
    var doneTasks = allTasks.filter(function (t) { return t.done; }).length;
    var overallPct = totalTasks === 0 ? 0 : Math.round((doneTasks / totalTasks) * 100);
    var totalVisits = state.clients.reduce(function (acc, c) { return acc + (c.visits || []).length; }, 0);

    document.getElementById('overview').innerHTML =
      '<div class="stat-card">' +
        '<div class="stat-icon clients">' + ICONS.users + '</div>' +
        '<div class="stat-info"><span class="stat-value">' + totalClients + '</span><span class="stat-label">عدد العملاء</span></div>' +
      '</div>' +
      '<div class="stat-card">' +
        '<div class="stat-icon tasks">' + ICONS.tasks + '</div>' +
        '<div class="stat-info"><span class="stat-value">' + totalTasks + '</span><span class="stat-label">إجمالي المهام</span></div>' +
      '</div>' +
      '<div class="stat-card">' +
        '<div class="stat-icon done">' + ICONS.check + '</div>' +
        '<div class="stat-info"><span class="stat-value">' + doneTasks + '</span><span class="stat-label">المهام المكتملة</span></div>' +
      '</div>' +
      '<div class="stat-card">' +
        '<div class="stat-icon visits">' + ICONS.calendar + '</div>' +
        '<div class="stat-info"><span class="stat-value">' + totalVisits + '</span><span class="stat-label">إجمالي الزيارات</span></div>' +
      '</div>' +
      '<div class="stat-card hero">' +
        '<div class="hero-ring">' + renderRingSVG(overallPct, totalTasks, 64, 8, true) + '</div>' +
        '<div class="stat-info"><span class="hero-title">نسبة الإنجاز الكلية</span><span class="hero-subtitle">' + doneTasks + '/' + totalTasks + ' مهام مكتملة</span></div>' +
      '</div>';
  }

  function renderComparison() {
    var container = document.getElementById('comparisonList');
    if (state.clients.length === 0) {
      container.innerHTML = '<div class="empty-tasks">لا يوجد عملاء لعرض المقارنة</div>';
      return;
    }
    container.innerHTML = state.clients.map(function (client) {
      var s = getClientStats(client);
      var color = getColorForPct(s.pct, s.total);
      var safeName = escapeHtml(client.name);
      return '<div class="compare-row">' +
        '<div class="compare-name" title="' + safeName + '">' + safeName + '</div>' +
        '<div class="compare-track"><div class="compare-fill" style="width:' + s.pct + '%; background:' + color + ';"></div></div>' +
        '<div class="compare-percent" style="color:' + color + '">' + s.pct + '%</div>' +
      '</div>';
    }).join('');
  }

  function renderVisitsComparison() {
    var container = document.getElementById('visitsComparisonList');
    if (state.clients.length === 0) {
      container.innerHTML = '<div class="empty-tasks">لا يوجد عملاء لعرض المقارنة</div>';
      return;
    }
    var maxVisits = Math.max(1, state.clients.reduce(function (m, c) { return Math.max(m, (c.visits || []).length); }, 0));
    container.innerHTML = state.clients.map(function (client) {
      var count = (client.visits || []).length;
      var widthPct = Math.round((count / maxVisits) * 100);
      var safeName = escapeHtml(client.name);
      return '<div class="compare-row">' +
        '<div class="compare-name" title="' + safeName + '">' + safeName + '</div>' +
        '<div class="compare-track"><div class="compare-fill" style="width:' + widthPct + '%; background:#be185d;"></div></div>' +
        '<div class="compare-percent" style="color:#be185d">' + count + '</div>' +
      '</div>';
    }).join('');
  }

  function renderClients() {
    var grid = document.getElementById('clientsGrid');
    if (state.clients.length === 0) {
      grid.innerHTML = '<div class="empty-state">' + ICONS.empty + '<div>لا يوجد عملاء بعد. أضف أول عميل من الأعلى للبدء.</div></div>';
      return;
    }
    grid.innerHTML = state.clients.map(function (client) {
      var s = getClientStats(client);
      var safeClientName = escapeHtml(client.name);
      var metaText = s.total === 0 ? 'لا توجد مهام بعد' : (s.done + '/' + s.total + ' مهام مكتملة');

      var sortedTasks = client.tasks.slice().sort(function (a, b) {
        return PRIORITY_META[normalizePriority(a.priority)].order - PRIORITY_META[normalizePriority(b.priority)].order;
      });
      var visibleTasks = state.filterPriority === 'all'
        ? sortedTasks
        : sortedTasks.filter(function (t) { return normalizePriority(t.priority) === state.filterPriority; });

      var tasksHtml;
      if (client.tasks.length === 0) {
        tasksHtml = '<div class="empty-tasks">لا توجد مهام بعد</div>';
      } else if (visibleTasks.length === 0) {
        tasksHtml = '<div class="empty-tasks">لا توجد مهام بهذه الأولوية</div>';
      } else {
        tasksHtml = visibleTasks.map(function (task) {
          var safeTaskName = escapeHtml(task.name);
          var priority = normalizePriority(task.priority);
          var prioritySelect = '<select class="priority-pill priority-' + priority + '" data-action="change-priority" data-client="' + client.id + '" data-task="' + task.id + '" title="تغيير الأولوية">' +
            Object.keys(PRIORITY_META).map(function (p) {
              return '<option value="' + p + '" ' + (p === priority ? 'selected' : '') + '>' + PRIORITY_META[p].label + '</option>';
            }).join('') +
          '</select>';
          var priorityPrint = '<span class="priority-pill priority-badge-print priority-' + priority + '">' + PRIORITY_META[priority].label + '</span>';
          return '<li class="task-item ' + (task.done ? 'done' : '') + '">' +
            '<label>' +
              '<input type="checkbox" ' + (task.done ? 'checked' : '') + ' data-action="toggle" data-client="' + client.id + '" data-task="' + task.id + '">' +
              '<span class="task-name">' + safeTaskName + '</span>' +
            '</label>' +
            '<span class="task-leader"></span>' +
            prioritySelect +
            priorityPrint +
            '<button class="delete-task-btn" data-action="del-task" data-client="' + client.id + '" data-task="' + task.id + '" title="حذف المهمة">×</button>' +
          '</li>';
        }).join('');
      }

      var visits = client.visits || [];
      var visitsHtml;
      if (visits.length === 0) {
        visitsHtml = '<div class="empty-visits">لا توجد زيارات مسجلة بعد</div>';
      } else {
        visitsHtml = visits.map(function (visit) {
          var safeVisitName = escapeHtml(visit.name);
          var dateBadge = visit.date ? '<span class="visit-date-badge">' + formatVisitDateShort(visit.date) + '</span>' : '';
          return '<li class="visit-item">' +
            '<svg class="visit-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="18" rx="2"/><path d="M16 2v4M8 2v4M3 10h18"/></svg>' +
            '<span class="visit-name" title="' + safeVisitName + '">' + safeVisitName + '</span>' +
            dateBadge +
            '<button class="delete-visit-btn" data-action="del-visit" data-client="' + client.id + '" data-visit="' + visit.id + '" title="حذف الزيارة">×</button>' +
          '</li>';
        }).join('');
      }

      return '<div class="client-card">' +
        '<div class="client-card-header">' +
          '<div class="client-ring">' + renderRingSVG(s.pct, s.total, 56, 6, false) + '</div>' +
          '<div class="client-info">' +
            '<div class="client-name" title="' + safeClientName + '">' + safeClientName + '</div>' +
            '<div class="client-meta">' + metaText + '</div>' +
          '</div>' +
          '<button class="delete-client-btn" data-action="del-client" data-client="' + client.id + '" title="حذف العميل">×</button>' +
        '</div>' +
        '<ul class="task-list">' + tasksHtml + '</ul>' +
        '<div class="add-task-row">' +
          '<input type="text" placeholder="مهمة جديدة..." data-action="task-input" data-client="' + client.id + '">' +
          '<select class="priority-select-new" data-action="new-task-priority" data-client="' + client.id + '">' +
            Object.keys(PRIORITY_META).map(function (p) {
              return '<option value="' + p + '" ' + (p === 'medium' ? 'selected' : '') + '>' + PRIORITY_META[p].label + '</option>';
            }).join('') +
          '</select>' +
          '<button data-action="add-task" data-client="' + client.id + '">+</button>' +
        '</div>' +
        '<div class="section-divider"></div>' +
        '<div class="section-label"><span>الزيارات</span><span class="visit-count-badge">' + visits.length + ' زيارة</span></div>' +
        '<ul class="visit-list">' + visitsHtml + '</ul>' +
        '<div class="visit-warn-banner" id="visitWarn-' + client.id + '"></div>' +
        '<div class="add-visit-row">' +
          '<input type="text" placeholder="زيارة جديدة (مثال: متابعة الجرد)..." data-action="visit-input" data-client="' + client.id + '">' +
          '<input type="date" data-action="visit-date-input" data-client="' + client.id + '" title="تاريخ الزيارة (اختياري)">' +
          '<button data-action="add-visit" data-client="' + client.id + '">+</button>' +
        '</div>' +
      '</div>';
    }).join('');
  }

  function render() {
    clearArmed();
    renderOverview();
    renderComparison();
    renderVisitsComparison();
    renderClients();
    renderVisitsFlatView();
    applyViewMode();
  }

  function applyViewMode() {
    document.getElementById('clientsGrid').style.display = viewMode === 'clients' ? '' : 'none';
    document.getElementById('priorityFilterBar').style.display = viewMode === 'clients' ? '' : 'none';
    document.getElementById('visitsFlatView').style.display = viewMode === 'visits' ? 'flex' : 'none';
    document.querySelectorAll('#viewToggleBar .priority-filter-btn').forEach(function (b) {
      b.classList.toggle('active', b.dataset.view === viewMode);
    });
  }

  // ---- All-visits flat view: every visit across every client, with a date
  // filter and a mini calendar, so visits can be reviewed without any task
  // clutter and it's easy to spot which dates are already booked. ----

  function getAllVisitsFlat() {
    var out = [];
    state.clients.forEach(function (client) {
      (client.visits || []).forEach(function (visit) {
        out.push({ clientId: client.id, clientName: client.name, visitId: visit.id, visitName: visit.name, date: visit.date || '' });
      });
    });
    out.sort(function (a, b) {
      if (a.date && b.date) return a.date < b.date ? -1 : (a.date > b.date ? 1 : 0);
      if (a.date && !b.date) return -1;
      if (!a.date && b.date) return 1;
      return 0;
    });
    return out;
  }

  function formatVisitDateShort(dateStr) {
    var parts = (dateStr || '').split('-');
    if (parts.length !== 3) return dateStr || '';
    var d = new Date(Number(parts[0]), Number(parts[1]) - 1, Number(parts[2]));
    if (isNaN(d.getTime())) return dateStr;
    return d.getDate() + ' ' + arabicMonths[d.getMonth()];
  }

  function renderVisitsFlatView() {
    var wrap = document.getElementById('visitsFlatView');
    var allVisits = getAllVisitsFlat();
    var filtered = calendarSelectedDate ? allVisits.filter(function (v) { return v.date === calendarSelectedDate; }) : allVisits;

    var listHtml;
    if (filtered.length === 0) {
      listHtml = '<div class="visits-flat-empty">' + (calendarSelectedDate ? 'لا توجد زيارات في هذا التاريخ' : 'لا توجد زيارات مسجلة بعد') + '</div>';
    } else {
      listHtml = '<ul class="visits-flat-list">' + filtered.map(function (v) {
        return '<li class="visits-flat-row">' +
          '<svg class="visit-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="18" rx="2"/><path d="M16 2v4M8 2v4M3 10h18"/></svg>' +
          '<span class="vf-date">' + (v.date ? formatVisitDateShort(v.date) : '—') + '</span>' +
          '<span class="vf-name" title="' + escapeHtml(v.visitName) + '">' + escapeHtml(v.visitName) + '</span>' +
          '<span class="vf-client">' + escapeHtml(v.clientName) + '</span>' +
        '</li>';
      }).join('') + '</ul>';
    }

    wrap.innerHTML =
      '<div class="visits-flat-main">' +
        '<div class="visits-flat-header">' +
          '<span class="visits-flat-title">كل الزيارات (' + allVisits.length + ')' + (calendarSelectedDate ? ' — ' + formatVisitDateShort(calendarSelectedDate) : '') + '</span>' +
          (calendarSelectedDate ? '<button class="visits-filter-clear" data-action="cal-reset">مسح الفلترة ×</button>' : '') +
        '</div>' + listHtml +
      '</div>' +
      '<div class="visits-flat-side">' + buildMiniCalendarHtml(allVisits) + '</div>';
  }

  function buildMiniCalendarHtml(allVisits) {
    var visitDates = {};
    allVisits.forEach(function (v) { if (v.date) visitDates[v.date] = (visitDates[v.date] || 0) + 1; });

    var base = new Date();
    base.setDate(1);
    base.setMonth(base.getMonth() + calendarMonthOffset);
    var year = base.getFullYear(), month = base.getMonth();
    var firstDayIndex = new Date(year, month, 1).getDay();
    var daysInMonth = new Date(year, month + 1, 0).getDate();

    var cells = '';
    for (var i = 0; i < firstDayIndex; i++) cells += '<div class="mini-cal-day empty"></div>';
    for (var day = 1; day <= daysInMonth; day++) {
      var dateStr = year + '-' + String(month + 1).padStart(2, '0') + '-' + String(day).padStart(2, '0');
      var hasVisit = !!visitDates[dateStr];
      var isSelected = dateStr === calendarSelectedDate;
      cells += '<div class="mini-cal-day ' + (hasVisit ? 'has-visit' : '') + (isSelected ? ' selected' : '') + '"' +
        (hasVisit ? (' data-action="cal-day" data-date="' + dateStr + '" title="' + visitDates[dateStr] + ' زيارة"') : '') +
        '>' + day + '</div>';
    }

    return '<div class="mini-cal-head">' +
        '<span class="mini-cal-title">' + arabicMonths[month] + ' ' + year + '</span>' +
        '<div class="mini-cal-nav">' +
          '<button data-action="cal-prev" title="الشهر السابق">‹</button>' +
          '<button data-action="cal-next" title="الشهر التالي">›</button>' +
        '</div>' +
      '</div>' +
      '<div class="mini-cal-weekdays"><span>أحد</span><span>إثن</span><span>ثلا</span><span>أرب</span><span>خمي</span><span>جمع</span><span>سبت</span></div>' +
      '<div class="mini-cal-grid">' + cells + '</div>' +
      '<div class="mini-cal-hint">الأيام الملوّنة بها زيارات مسجلة بالفعل — اضغط على أي يوم لعرض زياراته فقط.</div>';
  }

  function addTaskToClient(clientId) {
    var input = document.querySelector('input[data-action="task-input"][data-client="' + clientId + '"]');
    if (!input) return;
    var name = input.value.trim();
    if (!name) return;
    var client = state.clients.find(function (c) { return c.id === clientId; });
    if (!client) return;
    var prioritySelect = document.querySelector('select[data-action="new-task-priority"][data-client="' + clientId + '"]');
    var priority = normalizePriority(prioritySelect ? prioritySelect.value : 'medium');
    client.tasks.push({ id: state.nextTaskId++, name: name, done: false, priority: priority });
    render();
    saveState();
    var newInput = document.querySelector('input[data-action="task-input"][data-client="' + clientId + '"]');
    if (newInput) newInput.focus();
  }

  function addVisitToClient(clientId) {
    var input = document.querySelector('input[data-action="visit-input"][data-client="' + clientId + '"]');
    var dateInput = document.querySelector('input[data-action="visit-date-input"][data-client="' + clientId + '"]');
    if (!input) return;
    var name = input.value.trim();
    if (!name) return;
    var date = dateInput ? dateInput.value : '';
    var client = state.clients.find(function (c) { return c.id === clientId; });
    if (!client) return;
    if (!Array.isArray(client.visits)) client.visits = [];

    // Soft duplicate-date warning: doesn't block adding the visit, just
    // flags that this date is already booked (possibly for another client)
    // so it isn't scheduled twice by mistake.
    var conflict = date ? getAllVisitsFlat().find(function (v) { return v.date === date; }) : null;

    client.visits.push({ id: state.nextVisitId++, name: name, date: date });
    render();
    saveState();

    if (conflict) {
      var banner = document.getElementById('visitWarn-' + clientId);
      if (banner) {
        banner.textContent = '⚠ تنبيه: يوجد بالفعل زيارة أخرى في ' + formatVisitDateShort(date) + ' — "' + conflict.visitName + '" (' + conflict.clientName + ')';
        banner.classList.add('show');
        setTimeout(function () { banner.classList.remove('show'); banner.textContent = ''; }, 6000);
      }
    }

    var newInput = document.querySelector('input[data-action="visit-input"][data-client="' + clientId + '"]');
    if (newInput) newInput.focus();
  }

  function addClient() {
    var input = document.getElementById('newClientInput');
    var name = input.value.trim();
    if (!name) return;
    state.clients.push({ id: state.nextClientId++, name: name, tasks: [], visits: [] });
    input.value = '';
    render();
    saveState();
  }

  document.addEventListener('click', function (e) {
    var btn = e.target.closest('[data-action]');
    if (!btn) return;
    var action = btn.dataset.action;
    if (action === 'toggle' || action === 'task-input' || action === 'change-priority' || action === 'new-task-priority' || action === 'visit-input' || action === 'visit-date-input') return;

    var clientId = btn.dataset.client ? Number(btn.dataset.client) : null;
    var taskId = btn.dataset.task ? Number(btn.dataset.task) : null;

    if (action === 'set-view') {
      viewMode = btn.dataset.view;
      applyViewMode();
      return;
    }

    if (action === 'cal-prev') { calendarMonthOffset--; renderVisitsFlatView(); return; }
    if (action === 'cal-next') { calendarMonthOffset++; renderVisitsFlatView(); return; }
    if (action === 'cal-reset') { calendarSelectedDate = null; renderVisitsFlatView(); return; }
    if (action === 'cal-day') {
      calendarSelectedDate = calendarSelectedDate === btn.dataset.date ? null : btn.dataset.date;
      renderVisitsFlatView();
      return;
    }

    if (action === 'filter-priority') {
      state.filterPriority = btn.dataset.priority;
      document.querySelectorAll('.priority-filter-btn').forEach(function (b) {
        b.classList.toggle('active', b.dataset.priority === state.filterPriority);
      });
      renderClients();
      return;
    }

    if (action === 'del-client') {
      if (isArmed('client', clientId)) {
        clearArmed();
        state.clients = state.clients.filter(function (c) { return c.id !== clientId; });
        render(); saveState();
      } else {
        armDelete(btn, 'client', clientId);
      }
    } else if (action === 'del-task') {
      var compositeId = clientId + '-' + taskId;
      if (isArmed('task', compositeId)) {
        clearArmed();
        var client = state.clients.find(function (c) { return c.id === clientId; });
        if (client) client.tasks = client.tasks.filter(function (t) { return t.id !== taskId; });
        render(); saveState();
      } else {
        armDelete(btn, 'task', compositeId);
      }
    } else if (action === 'add-task') {
      addTaskToClient(clientId);
    } else if (action === 'del-visit') {
      var visitId = btn.dataset.visit ? Number(btn.dataset.visit) : null;
      var compositeVisitId = clientId + '-' + visitId;
      if (isArmed('visit', compositeVisitId)) {
        clearArmed();
        var clientForVisit = state.clients.find(function (c) { return c.id === clientId; });
        if (clientForVisit) clientForVisit.visits = (clientForVisit.visits || []).filter(function (v) { return v.id !== visitId; });
        render(); saveState();
      } else {
        armDelete(btn, 'visit', compositeVisitId);
      }
    } else if (action === 'add-visit') {
      addVisitToClient(clientId);
    }
  });

  document.addEventListener('change', function (e) {
    if (e.target.dataset.action === 'toggle') {
      var clientId = Number(e.target.dataset.client);
      var taskId = Number(e.target.dataset.task);
      var client = state.clients.find(function (c) { return c.id === clientId; });
      if (client) {
        var task = client.tasks.find(function (t) { return t.id === taskId; });
        if (task) { task.done = !task.done; render(); saveState(); }
      }
    } else if (e.target.dataset.action === 'change-priority') {
      var clientId2 = Number(e.target.dataset.client);
      var taskId2 = Number(e.target.dataset.task);
      var client2 = state.clients.find(function (c) { return c.id === clientId2; });
      if (client2) {
        var task2 = client2.tasks.find(function (t) { return t.id === taskId2; });
        if (task2) { task2.priority = normalizePriority(e.target.value); render(); saveState(); }
      }
    }
  });

  document.addEventListener('keydown', function (e) {
    if (e.key === 'Enter' && e.target.dataset.action === 'task-input') {
      addTaskToClient(Number(e.target.dataset.client));
    } else if (e.key === 'Enter' && e.target.dataset.action === 'visit-input') {
      addVisitToClient(Number(e.target.dataset.client));
    }
  });

  document.getElementById('addClientBtn').addEventListener('click', addClient);
  document.getElementById('newClientInput').addEventListener('keydown', function (e) {
    if (e.key === 'Enter') addClient();
  });

  function showPdfError(message) {
    var banner = document.getElementById('pdfErrorBanner');
    banner.textContent = 'تعذر إنشاء ملف PDF: ' + message;
    banner.classList.add('show');
  }
  function hidePdfError() {
    var banner = document.getElementById('pdfErrorBanner');
    banner.classList.remove('show');
    banner.textContent = '';
  }

  // ---------------------------------------------------------------------
  // ENTERPRISE PDF EXPORT
  // Instead of rasterizing the screen (html2canvas), we build a dedicated,
  // print-optimized document (#printRoot) with real vector text, proper
  // A4 pagination, a cover/executive-summary page, and repeating headers &
  // footers with accurate "page X of Y" numbers. We then hand the whole
  // thing to the browser's native print engine (window.print → "Save as
  // PDF"), which yields crisp, infinite-resolution vector output instead
  // of a blurry bitmap.
  // ---------------------------------------------------------------------

  var LOGO_DATA_URI = (function () {
    var img = document.querySelector('.header-logo img');
    return img ? img.getAttribute('src') : '';
  })();

  function ppRingSVG(pct, total, sizeMm, strokeMm) {
    var clamped = Math.max(0, Math.min(100, Math.round(pct)));
    var vb = 100;
    var r = (vb - strokeMm * (vb / sizeMm)) / 2;
    var sw = strokeMm * (vb / sizeMm);
    var circumference = 2 * Math.PI * r;
    var offset = circumference * (1 - clamped / 100);
    var color = getColorForPct(clamped, total);
    return '<svg viewBox="0 0 ' + vb + ' ' + vb + '">' +
      '<circle class="pp-ring-bg" cx="' + (vb/2) + '" cy="' + (vb/2) + '" r="' + r + '" stroke-width="' + sw + '"></circle>' +
      '<circle cx="' + (vb/2) + '" cy="' + (vb/2) + '" r="' + r + '" stroke-width="' + sw + '" stroke="' + color + '" stroke-linecap="round" stroke-dasharray="' + circumference.toFixed(2) + '" stroke-dashoffset="' + offset.toFixed(2) + '"></circle>' +
    '</svg><div class="pp-client-ring-text" style="color:' + color + '">' + clamped + '%</div>';
  }

  function ppHeader(dateTimeStr) {
    return '<div class="pp-header">' +
      '<div class="pp-header-brand">' +
        (LOGO_DATA_URI ? '<img src="' + LOGO_DATA_URI + '" alt="">' : '') +
        '<span class="pp-brand-text">تقرير متابعة أداء العملاء</span>' +
      '</div>' +
      '<div class="pp-header-title">' + escapeHtml(dateTimeStr) + '</div>' +
    '</div>';
  }

  function ppFooter(pageNum, totalPages) {
    return '<div class="pp-footer">' +
      '<span class="pp-footer-left">لوحة متابعة مهام العملاء</span>' +
      '<span class="pp-page-num">صفحة ' + pageNum + ' من ' + totalPages + '</span>' +
    '</div>';
  }

  function buildCoverPageHtml(dateTimeStr) {
    var totalClients = state.clients.length;
    var allTasks = state.clients.reduce(function (acc, c) { return acc.concat(c.tasks); }, []);
    var totalTasks = allTasks.length;
    var doneTasks = allTasks.filter(function (t) { return t.done; }).length;
    var overallPct = totalTasks === 0 ? 0 : Math.round((doneTasks / totalTasks) * 100);
    var totalVisits = state.clients.reduce(function (acc, c) { return acc + (c.visits || []).length; }, 0);
    var pendingTasks = totalTasks - doneTasks;
    var highPriorityOpen = allTasks.filter(function (t) { return !t.done && normalizePriority(t.priority) === 'high'; }).length;
    var fullyDoneClients = state.clients.filter(function (c) { var s = getClientStats(c); return s.total > 0 && s.pct === 100; }).length;

    var summaryText = totalClients === 0
      ? 'لا يوجد عملاء مسجلون حاليًا في النظام.'
      : ('يغطي هذا التقرير أداء ' + totalClients + ' عميلًا بإجمالي ' + totalTasks + ' مهمة، منها ' + doneTasks +
         ' مهمة مكتملة (' + overallPct + '%) و' + pendingTasks + ' مهمة قيد الإنجاز. ' +
         (highPriorityOpen > 0 ? ('يوجد حاليًا ' + highPriorityOpen + ' مهمة مرتفعة الأولوية لم تُنجز بعد وتستحق المتابعة العاجلة. ') : 'لا توجد مهام مرتفعة الأولوية متأخرة حاليًا. ') +
         'كما تم تسجيل ' + totalVisits + ' زيارة ميدانية عبر جميع العملاء، وأنهى ' + fullyDoneClients + ' من العملاء جميع مهامهم المسجلة بالكامل.');

    var kpis = '<div class="pp-kpi-grid">' +
      '<div class="pp-kpi-card"><div class="pp-kpi-value">' + totalClients + '</div><div class="pp-kpi-label">عدد العملاء</div></div>' +
      '<div class="pp-kpi-card"><div class="pp-kpi-value">' + totalTasks + '</div><div class="pp-kpi-label">إجمالي المهام</div></div>' +
      '<div class="pp-kpi-card"><div class="pp-kpi-value">' + doneTasks + '</div><div class="pp-kpi-label">المهام المكتملة</div></div>' +
      '<div class="pp-kpi-card"><div class="pp-kpi-value">' + totalVisits + '</div><div class="pp-kpi-label">إجمالي الزيارات</div></div>' +
      '<div class="pp-kpi-card pp-kpi-hero"><div class="pp-kpi-value">' + overallPct + '%</div><div class="pp-kpi-label">نسبة الإنجاز الكلية</div></div>' +
    '</div>';

    var compareRows = state.clients.length === 0 ? '' : state.clients.map(function (client) {
      var s = getClientStats(client);
      var color = getColorForPct(s.pct, s.total);
      var safeName = escapeHtml(client.name);
      return '<div class="pp-compare-row">' +
        '<div class="pp-compare-name">' + safeName + '</div>' +
        '<div class="pp-compare-track"><div class="pp-compare-fill" style="width:' + s.pct + '%;background:' + color + ';"></div></div>' +
        '<div class="pp-compare-pct" style="color:' + color + '">' + s.pct + '%</div>' +
      '</div>';
    }).join('');

    var maxVisits = Math.max(1, state.clients.reduce(function (m, c) { return Math.max(m, (c.visits || []).length); }, 0));
    var visitRows = state.clients.length === 0 ? '' : state.clients.map(function (client) {
      var count = (client.visits || []).length;
      var widthPct = Math.round((count / maxVisits) * 100);
      var safeName = escapeHtml(client.name);
      return '<div class="pp-compare-row">' +
        '<div class="pp-compare-name">' + safeName + '</div>' +
        '<div class="pp-compare-track"><div class="pp-compare-fill" style="width:' + widthPct + '%;background:#be185d;"></div></div>' +
        '<div class="pp-compare-pct" style="color:#be185d">' + count + '</div>' +
      '</div>';
    }).join('');

    return '<section class="pp-page pp-cover">' +
      '<div class="pp-cover-band">' +
        '<div class="pp-cover-eyebrow">تقرير تنفيذي · إدارة متابعة العملاء</div>' +
        '<div class="pp-cover-title">تقرير متابعة أداء العملاء والزيارات</div>' +
        '<div class="pp-cover-sub">نظرة شاملة على حالة المهام ونسب الإنجاز والزيارات الميدانية لكل عميل</div>' +
        '<div class="pp-cover-meta-row">' +
          '<span>تاريخ الإصدار: <b>' + escapeHtml(dateTimeStr) + '</b></span>' +
          '<span>عدد العملاء: <b>' + totalClients + '</b></span>' +
          '<span>نسبة الإنجاز الكلية: <b>' + overallPct + '%</b></span>' +
        '</div>' +
      '</div>' +
      '<div class="pp-section-title"><span class="pp-dot"></span>الملخص التنفيذي</div>' +
      '<div class="pp-exec-summary">' + escapeHtml(summaryText) + '</div>' +
      '<div class="pp-section-title"><span class="pp-dot"></span>المؤشرات الرئيسية</div>' +
      kpis +
      (state.clients.length ? ('<div class="pp-section-title"><span class="pp-dot"></span>مقارنة نسبة الإنجاز بين العملاء</div><div class="pp-compare-block">' + compareRows + '</div>') : '') +
      (state.clients.length ? ('<div class="pp-section-title"><span class="pp-dot"></span>مقارنة عدد الزيارات بين العملاء</div><div class="pp-compare-block">' + visitRows + '</div>') : '') +
    '</section>';
  }

  function buildClientCardHtml(client) {
    var s = getClientStats(client);
    var safeName = escapeHtml(client.name);
    var metaText = s.total === 0 ? 'لا توجد مهام مسجلة' : (s.done + ' من ' + s.total + ' مهام مكتملة');

    var sortedTasks = client.tasks.slice().sort(function (a, b) {
      return PRIORITY_META[normalizePriority(a.priority)].order - PRIORITY_META[normalizePriority(b.priority)].order;
    });

    var rowsHtml;
    if (sortedTasks.length === 0) {
      rowsHtml = '<tr class="pp-empty-row"><td colspan="3">لا توجد مهام مسجلة لهذا العميل</td></tr>';
    } else {
      rowsHtml = sortedTasks.map(function (task) {
        var priority = normalizePriority(task.priority);
        return '<tr>' +
          '<td><span class="pp-status-dot ' + (task.done ? 'on' : 'off') + '"></span>' +
          '<span class="' + (task.done ? 'pp-task-name-done' : '') + '">' + escapeHtml(task.name) + '</span></td>' +
          '<td>' + (task.done ? 'مكتملة' : 'قيد الإنجاز') + '</td>' +
          '<td><span class="pp-pill ' + priority + '">' + PRIORITY_META[priority].label + '</span></td>' +
        '</tr>';
      }).join('');
    }

    var visits = client.visits || [];
    var visitsHtml = visits.length === 0
      ? '<div class="pp-visit-row" style="opacity:.6">لا توجد زيارات مسجلة بعد</div>'
      : visits.map(function (v) { return '<div class="pp-visit-row">' + escapeHtml(v.name) + '</div>'; }).join('');

    return '<div class="pp-client-card">' +
      '<div class="pp-client-head">' +
        '<div class="pp-client-ring">' + ppRingSVG(s.pct, s.total, 15, 2.2) + '</div>' +
        '<div>' +
          '<div class="pp-client-name">' + safeName + '</div>' +
          '<div class="pp-client-meta">' + metaText + '</div>' +
        '</div>' +
        '<div class="pp-client-badges"><span class="pp-badge pp-badge-visits">' + visits.length + ' زيارة</span></div>' +
      '</div>' +
      '<table class="pp-table"><thead><tr><th>المهمة</th><th>الحالة</th><th>الأولوية</th></tr></thead><tbody>' + rowsHtml + '</tbody></table>' +
      '<div class="pp-visits-title"><span>سجل الزيارات الميدانية</span></div>' +
      visitsHtml +
    '</div>';
  }

  // Accurate per-page packing: instead of guessing each card's height with a
  // formula (which either overflows or leaves a large blank gap when the
  // guess is off), we render every card into an invisible, off-screen stage
  // using the exact same CSS, measure its real height, and pack pages based
  // on those real numbers. Small cards now pack tightly together on a page;
  // only a genuinely oversized card gets a page of its own.
  function mmToPxRatio() {
    var probe = document.createElement('div');
    probe.style.cssText = 'position:fixed;left:-9999px;top:0;height:200mm;width:1mm;visibility:hidden;';
    document.body.appendChild(probe);
    var ratio = probe.getBoundingClientRect().height / 200;
    document.body.removeChild(probe);
    return ratio || 3.7795;
  }

  // Cards are measured at COLUMN width (half a page, since two clients sit
  // side by side in a page) because a card wraps its text differently at
  // half-width than at full-width. A card that, even alone, is too tall to
  // fit the page is treated as "oversized" and gets its own full-width page
  // instead of being squeezed next to another client.
  var PAGE_CONTENT_WIDTH_MM = 178; // 210mm page minus 16mm margins each side
  var COLUMN_GAP_MM = 6;
  var COLUMN_WIDTH_MM = (PAGE_CONTENT_WIDTH_MM - COLUMN_GAP_MM) / 2;
  var USABLE_PAGE_HEIGHT_MM = 297 - 26 - 20; // page minus top/bottom padding reserved for header/footer

  function measureClientCardHeightsMm(clients, widthMm) {
    var pxPerMm = mmToPxRatio();
    var stage = document.createElement('div');
    stage.style.cssText = 'position:fixed;left:-9999px;top:0;width:' + widthMm + 'mm;visibility:hidden;' +
      "font-family:'Cairo','Tajawal',Tahoma,'Segoe UI',Arial,sans-serif;direction:rtl;";
    document.body.appendChild(stage);
    var heights = clients.map(function (client) {
      stage.innerHTML = buildClientCardHtml(client);
      return stage.firstElementChild.getBoundingClientRect().height / pxPerMm;
    });
    document.body.removeChild(stage);
    return heights;
  }

  // Groups clients into page "slots": either a pair (two clients rendered
  // side by side in two columns) or a solo (one oversized client alone on a
  // full-width page). Order is preserved.
  function groupClientsIntoPages(clients) {
    var colHeights = measureClientCardHeightsMm(clients, COLUMN_WIDTH_MM);
    var slots = [];
    var pairBuffer = [];

    function flushPair() {
      if (pairBuffer.length) {
        slots.push({ type: 'pair', clients: pairBuffer });
        pairBuffer = [];
      }
    }

    clients.forEach(function (client, i) {
      var oversized = colHeights[i] > USABLE_PAGE_HEIGHT_MM;
      if (oversized) {
        flushPair();
        slots.push({ type: 'solo', clients: [client] });
      } else {
        pairBuffer.push(client);
        if (pairBuffer.length === 2) flushPair();
      }
    });
    flushPair();
    return slots;
  }

  function buildClientSlotHtml(slot) {
    if (slot.type === 'solo') {
      return buildClientCardHtml(slot.clients[0]);
    }
    return '<div class="pp-clients-pair-grid">' +
      slot.clients.map(buildClientCardHtml).join('') +
    '</div>';
  }

  async function exportPDF() {
    var btn = document.getElementById('downloadBtn');
    var btnText = document.getElementById('downloadBtnText');
    var originalText = btnText.textContent;
    btn.disabled = true;
    btnText.textContent = 'جاري التجهيز...';
    hidePdfError();

    try {
      var printRoot = document.getElementById('printRoot');
      var now = new Date();
      var dateTimeStr = formatArabicDate(now).replace('تاريخ التقرير: ', '') + ' — ' +
        String(now.getHours()).padStart(2, '0') + ':' + String(now.getMinutes()).padStart(2, '0');

      var clientPageGroups = groupClientsIntoPages(state.clients);
      var totalPages = 1 + (clientPageGroups.length || (state.clients.length === 0 ? 1 : 0));

      var html = ppHeader(dateTimeStr) + ppFooter(1, totalPages) + buildCoverPageHtml(dateTimeStr);

      if (state.clients.length === 0) {
        html += '<section class="pp-page">' + ppHeader(dateTimeStr) + ppFooter(2, totalPages) +
          '<div class="pp-section-title"><span class="pp-dot"></span>تفاصيل العملاء</div>' +
          '<div style="text-align:center;padding:20mm;color:#94a3b8;font-size:9pt;">لا يوجد عملاء بعد.</div>' +
        '</section>';
      } else {
        clientPageGroups.forEach(function (slot, idx) {
          var pageNum = idx + 2;
          html += '<section class="pp-page">' + ppHeader(dateTimeStr) + ppFooter(pageNum, totalPages) +
            (idx === 0 ? '<div class="pp-section-title"><span class="pp-dot"></span>تفاصيل العملاء والمهام</div>' : '') +
            buildClientSlotHtml(slot) +
          '</section>';
        });
      }

      printRoot.innerHTML = html;
      document.body.classList.add('pdf-export-mode');
      await new Promise(function (r) { setTimeout(r, 60); });

      window.print();

      document.body.classList.remove('pdf-export-mode');
      printRoot.innerHTML = '';
    } catch (err) {
      console.error(err);
      document.body.classList.remove('pdf-export-mode');
      showPdfError((err && err.message) ? err.message : 'خطأ غير معروف، حاول مجددًا.');
    }

    btnText.textContent = originalText;
    btn.disabled = false;
  }
  document.getElementById('downloadBtn').addEventListener('click', exportPDF);

  document.getElementById('reportDate').textContent = formatArabicDate(new Date());
  loadState();
})();
</script>
</body>
</html>

