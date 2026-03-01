<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LoanIQ — Project README</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Bricolage+Grotesque:opsz,wght@12..96,300;12..96,500;12..96,700;12..96,800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #05080f;
    --surface: #0c1220;
    --surface2: #111d30;
    --border: rgba(100,200,255,0.1);
    --accent: #00d4ff;
    --accent2: #7c3aed;
    --accent3: #10b981;
    --warn: #f59e0b;
    --danger: #ef4444;
    --text: #e2e8f0;
    --muted: #64748b;
    --mono: 'Space Mono', monospace;
    --display: 'Bricolage Grotesque', sans-serif;
  }

  * { margin:0; padding:0; box-sizing:border-box; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--display);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Animated grid background */
  body::before {
    content:'';
    position:fixed; inset:0; z-index:0;
    background-image:
      linear-gradient(rgba(0,212,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,212,255,0.03) 1px, transparent 1px);
    background-size: 60px 60px;
    animation: gridMove 20s linear infinite;
  }
  @keyframes gridMove {
    0%{background-position:0 0} 100%{background-position:60px 60px}
  }

  /* Glows */
  .glow-orb {
    position:fixed; border-radius:50%; filter:blur(120px); pointer-events:none; z-index:0;
    animation: orbFloat 8s ease-in-out infinite;
  }
  .orb1 { width:600px;height:600px;background:rgba(0,212,255,0.05);top:-200px;right:-200px; }
  .orb2 { width:500px;height:500px;background:rgba(124,58,237,0.06);bottom:-100px;left:-150px;animation-delay:-4s; }
  @keyframes orbFloat {
    0%,100%{transform:translateY(0)} 50%{transform:translateY(-30px)}
  }

  /* Sidebar nav */
  .sidebar {
    position: fixed; left:0; top:0; bottom:0; width:260px;
    background: rgba(12,18,32,0.95);
    border-right: 1px solid var(--border);
    backdrop-filter: blur(20px);
    z-index: 100;
    display:flex; flex-direction:column;
    padding: 0;
    overflow: hidden;
  }

  .sidebar-header {
    padding: 32px 24px 24px;
    border-bottom: 1px solid var(--border);
  }

  .sidebar-logo {
    font-family: var(--mono);
    font-size: 11px;
    color: var(--accent);
    letter-spacing: 3px;
    text-transform: uppercase;
    margin-bottom: 8px;
  }

  .sidebar-title {
    font-family: var(--display);
    font-size: 20px;
    font-weight: 800;
    line-height: 1.2;
  }
  .sidebar-title span { color: var(--accent); }

  .sidebar-badge {
    margin-top: 12px;
    display:inline-flex; align-items:center; gap:6px;
    background: rgba(16,185,129,0.1);
    border: 1px solid rgba(16,185,129,0.3);
    color: var(--accent3);
    padding: 4px 10px;
    border-radius: 4px;
    font-family: var(--mono);
    font-size: 10px;
    letter-spacing: 1px;
  }
  .sidebar-badge::before {
    content:'';width:5px;height:5px;background:var(--accent3);
    border-radius:50%;animation:pulse 2s infinite;
  }
  @keyframes pulse{0%,100%{opacity:1}50%{opacity:0.3}}

  .nav-links {
    padding: 20px 16px;
    display:flex; flex-direction:column; gap:4px;
    flex:1; overflow-y:auto;
  }

  .nav-link {
    display:flex; align-items:center; gap:12px;
    padding: 10px 12px;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.2s;
    border: 1px solid transparent;
    font-size: 13px; font-weight:500;
    color: var(--muted);
    text-decoration:none;
  }
  .nav-link:hover {
    background: rgba(0,212,255,0.07);
    border-color: rgba(0,212,255,0.15);
    color: var(--text);
  }
  .nav-link.active {
    background: rgba(0,212,255,0.1);
    border-color: rgba(0,212,255,0.25);
    color: var(--accent);
  }
  .nav-link .nav-icon { font-size:16px; width:20px; text-align:center; }
  .nav-section-label {
    padding: 16px 12px 6px;
    font-family: var(--mono);
    font-size: 9px;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--muted);
    opacity: 0.6;
  }

  .sidebar-footer {
    padding: 16px 24px;
    border-top: 1px solid var(--border);
    font-family: var(--mono);
    font-size: 10px;
    color: var(--muted);
  }

  /* Main content */
  .main {
    margin-left: 260px;
    min-height: 100vh;
    position: relative; z-index:1;
  }

  /* Sections */
  .section {
    min-height: 100vh;
    padding: 80px 60px;
    border-bottom: 1px solid var(--border);
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.6s ease, transform 0.6s ease;
  }
  .section.visible { opacity:1; transform:translateY(0); }

  .section-eyebrow {
    font-family: var(--mono);
    font-size: 11px;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 16px;
    display:flex; align-items:center; gap:10px;
  }
  .section-eyebrow::before {
    content:''; width:30px; height:1px; background:var(--accent);
  }

  .section-title {
    font-size: clamp(32px, 5vw, 56px);
    font-weight: 800;
    line-height: 1.05;
    margin-bottom: 20px;
    letter-spacing: -1px;
  }

  .section-subtitle {
    font-size: 18px;
    color: var(--muted);
    font-weight: 300;
    max-width: 600px;
    line-height: 1.6;
    margin-bottom: 48px;
  }

  /* HERO */
  #hero {
    display:flex; align-items:center;
    background: radial-gradient(ellipse at 70% 50%, rgba(0,212,255,0.04) 0%, transparent 60%);
    padding-top: 100px;
  }

  .hero-content { max-width: 700px; }

  .hero-title {
    font-size: clamp(48px, 7vw, 80px);
    font-weight: 800;
    line-height: 0.95;
    letter-spacing: -3px;
    margin-bottom: 24px;
  }
  .hero-title .highlight {
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .hero-desc {
    font-size: 18px;
    color: var(--muted);
    line-height: 1.7;
    margin-bottom: 40px;
    font-weight: 300;
    max-width: 560px;
  }

  .hero-files {
    display:flex; gap:16px; flex-wrap:wrap;
  }

  .file-chip {
    display:flex; align-items:center; gap:10px;
    background: var(--surface);
    border: 1px solid var(--border);
    padding: 12px 20px;
    border-radius: 12px;
    cursor: pointer;
    transition: all 0.25s;
    text-decoration: none;
    color: var(--text);
  }
  .file-chip:hover {
    border-color: var(--accent);
    background: rgba(0,212,255,0.05);
    transform: translateY(-2px);
    box-shadow: 0 8px 24px rgba(0,212,255,0.1);
  }
  .file-chip .file-icon { font-size:20px; }
  .file-chip .file-info { }
  .file-chip .file-name { font-size:13px; font-weight:700; font-family:var(--mono); }
  .file-chip .file-type { font-size:11px; color:var(--muted); font-family:var(--mono); }

  /* Stats strip */
  .stats-strip {
    display:grid; grid-template-columns:repeat(4,1fr);
    gap:1px; background:var(--border);
    border: 1px solid var(--border);
    border-radius: 16px; overflow:hidden;
    margin-top: 60px;
  }
  .stat-box {
    background: var(--surface);
    padding: 28px 24px;
    text-align:center;
    transition: background 0.2s;
  }
  .stat-box:hover { background: var(--surface2); }
  .stat-num {
    font-family: var(--mono);
    font-size: 36px;
    font-weight: 700;
    color: var(--accent);
    display:block;
  }
  .stat-label {
    font-size: 12px;
    color: var(--muted);
    margin-top:4px;
    letter-spacing:0.5px;
  }

  /* Pipeline steps */
  .pipeline {
    position:relative;
    display:flex; flex-direction:column; gap:0;
  }

  .pipeline-step {
    display:grid; grid-template-columns:80px 1fr;
    gap:0; cursor:pointer;
  }

  .step-spine {
    display:flex; flex-direction:column; align-items:center;
    padding-top:8px;
  }
  .step-dot {
    width:16px; height:16px;
    border-radius:50%;
    border:2px solid var(--muted);
    background:var(--bg);
    transition:all 0.3s;
    flex-shrink:0;
    position:relative; z-index:1;
  }
  .step-line {
    width:2px; flex:1; min-height:20px;
    background: linear-gradient(to bottom, var(--border), var(--border));
    margin:4px 0;
  }
  .pipeline-step:last-child .step-line { display:none; }

  .step-content {
    padding: 8px 0 32px 24px;
  }

  .step-header {
    display:flex; align-items:center; gap:12px;
    margin-bottom:0;
  }
  .step-num {
    font-family:var(--mono); font-size:10px;
    color:var(--muted); letter-spacing:2px;
  }
  .step-title {
    font-size:18px; font-weight:700;
    transition:color 0.2s;
  }
  .step-tag {
    margin-left:auto;
    font-family:var(--mono); font-size:10px;
    padding: 3px 8px; border-radius:4px;
    background:rgba(0,212,255,0.1);
    color:var(--accent);
    border:1px solid rgba(0,212,255,0.2);
  }

  .step-body {
    max-height: 0; overflow:hidden;
    transition: max-height 0.4s ease, opacity 0.3s ease;
    opacity:0;
  }
  .step-body.open { max-height:400px; opacity:1; }

  .step-body-inner {
    padding: 16px 0 0;
    color:var(--muted); font-size:14px; line-height:1.7;
  }

  .step-tags {
    display:flex; flex-wrap:wrap; gap:8px; margin-top:14px;
  }
  .tag {
    font-family:var(--mono); font-size:11px;
    padding: 4px 10px; border-radius:6px;
    background:var(--surface2);
    border:1px solid var(--border);
    color:var(--muted);
  }

  .pipeline-step:hover .step-dot,
  .pipeline-step.open .step-dot {
    border-color:var(--accent);
    background:var(--accent);
    box-shadow:0 0 12px rgba(0,212,255,0.5);
  }
  .pipeline-step:hover .step-title,
  .pipeline-step.open .step-title { color:var(--accent); }
  .pipeline-step:hover .step-line,
  .pipeline-step.open .step-line { background:rgba(0,212,255,0.2); }

  /* Models grid */
  .models-grid {
    display:grid; grid-template-columns:repeat(auto-fill, minmax(200px,1fr));
    gap:16px;
  }

  .model-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 24px;
    cursor:pointer;
    transition: all 0.3s;
    position:relative; overflow:hidden;
  }
  .model-card::before {
    content:''; position:absolute; inset:0;
    background:radial-gradient(circle at 50% 0%, rgba(0,212,255,0.08), transparent 70%);
    opacity:0; transition:opacity 0.3s;
  }
  .model-card:hover { border-color:var(--accent); transform:translateY(-4px); box-shadow:0 12px 32px rgba(0,212,255,0.1); }
  .model-card:hover::before { opacity:1; }
  .model-card.winner { border-color:var(--accent3); }
  .model-card.winner::before { background:radial-gradient(circle at 50% 0%, rgba(16,185,129,0.1), transparent 70%); opacity:1; }

  .model-icon { font-size:32px; margin-bottom:16px; display:block; }
  .model-name { font-size:14px; font-weight:700; margin-bottom:6px; }
  .model-abbr { font-family:var(--mono); font-size:11px; color:var(--muted); margin-bottom:16px; }

  .accuracy-bar {
    height:4px; background:var(--surface2); border-radius:2px; overflow:hidden;
  }
  .accuracy-fill {
    height:100%; border-radius:2px;
    background: linear-gradient(90deg, var(--accent2), var(--accent));
    width:0; transition:width 1.2s cubic-bezier(0.4,0,0.2,1);
  }
  .model-card.winner .accuracy-fill {
    background:linear-gradient(90deg, var(--accent3), #34d399);
  }
  .accuracy-label {
    font-family:var(--mono); font-size:11px; color:var(--muted);
    margin-top:8px; display:flex; justify-content:space-between;
  }

  .winner-badge {
    position:absolute; top:12px; right:12px;
    background:rgba(16,185,129,0.15);
    border:1px solid rgba(16,185,129,0.4);
    color:var(--accent3);
    font-family:var(--mono); font-size:9px;
    padding: 3px 8px; border-radius:4px;
    letter-spacing:1px;
  }

  /* Code block */
  .code-block {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    overflow:hidden;
    margin: 20px 0;
    font-family: var(--mono);
  }
  .code-header {
    display:flex; align-items:center; justify-content:space-between;
    padding: 12px 20px;
    background: var(--surface2);
    border-bottom: 1px solid var(--border);
  }
  .code-dots { display:flex; gap:6px; }
  .code-dot { width:10px; height:10px; border-radius:50%; }
  .code-dot:nth-child(1){background:#ef4444;}
  .code-dot:nth-child(2){background:#f59e0b;}
  .code-dot:nth-child(3){background:#10b981;}
  .code-lang { font-size:11px; color:var(--muted); letter-spacing:1px; }
  .copy-btn {
    font-size:11px; color:var(--muted); cursor:pointer;
    background:rgba(255,255,255,0.05); border:1px solid var(--border);
    padding:4px 10px; border-radius:6px; transition:all 0.2s;
    letter-spacing:0.5px;
  }
  .copy-btn:hover { color:var(--text); border-color:var(--accent); }
  .code-body { padding:20px; font-size:13px; line-height:1.8; overflow-x:auto; }
  .code-body .kw { color:#c792ea; }
  .code-body .fn { color:var(--accent); }
  .code-body .str { color:#c3e88d; }
  .code-body .cm { color:var(--muted); font-style:italic; }
  .code-body .num { color:#f78c6c; }

  /* Tab switcher */
  .tabs {
    display:flex; gap:0;
    border:1px solid var(--border);
    border-radius:10px; overflow:hidden; width:fit-content;
    margin-bottom:32px;
  }
  .tab {
    padding:10px 24px; font-size:13px; font-weight:500;
    cursor:pointer; transition:all 0.2s;
    color:var(--muted); background:var(--surface);
    border-right:1px solid var(--border);
    user-select:none;
  }
  .tab:last-child { border-right:none; }
  .tab:hover { color:var(--text); }
  .tab.active { background:rgba(0,212,255,0.1); color:var(--accent); }

  .tab-content { display:none; }
  .tab-content.active { display:block; }

  /* Feature list */
  .feature-list {
    display:grid; grid-template-columns:1fr 1fr;
    gap:16px;
  }
  .feature-item {
    display:flex; gap:16px; align-items:flex-start;
    background:var(--surface); border:1px solid var(--border);
    padding:20px; border-radius:12px;
    transition:all 0.2s;
  }
  .feature-item:hover { border-color:rgba(0,212,255,0.3); background:var(--surface2); }
  .feature-icon { font-size:24px; flex-shrink:0; }
  .feature-text {}
  .feature-title { font-size:14px; font-weight:700; margin-bottom:4px; }
  .feature-desc { font-size:13px; color:var(--muted); line-height:1.5; }

  /* Setup steps */
  .setup-steps {
    counter-reset:setup;
    display:flex; flex-direction:column; gap:16px;
  }
  .setup-step {
    counter-increment:setup;
    display:flex; gap:20px; align-items:flex-start;
    padding:20px 24px;
    background:var(--surface);
    border:1px solid var(--border);
    border-radius:12px;
    transition:all 0.3s;
  }
  .setup-step:hover { border-color:rgba(0,212,255,0.25); }
  .setup-step-num {
    font-family:var(--mono); font-size:11px;
    color:var(--accent);
    background:rgba(0,212,255,0.1);
    border:1px solid rgba(0,212,255,0.2);
    width:32px; height:32px;
    border-radius:8px;
    display:flex; align-items:center; justify-content:center;
    flex-shrink:0;
    content:counter(setup);
  }
  .setup-step-content {}
  .setup-step-title { font-size:14px; font-weight:700; margin-bottom:4px; }
  .setup-step-desc { font-size:13px; color:var(--muted); }

  /* Arch diagram */
  .arch {
    display:flex; align-items:center; gap:0;
    overflow-x:auto; padding:32px 0;
  }
  .arch-node {
    background:var(--surface);
    border:1px solid var(--border);
    border-radius:12px;
    padding:20px 24px;
    text-align:center;
    min-width:140px;
    flex-shrink:0;
    transition:all 0.3s;
    cursor:default;
  }
  .arch-node:hover { border-color:var(--accent); transform:scale(1.05); }
  .arch-node.highlight { border-color:var(--accent3); background:rgba(16,185,129,0.05); }
  .arch-icon { font-size:32px; display:block; margin-bottom:8px; }
  .arch-label { font-size:12px; font-weight:700; }
  .arch-sublabel { font-size:10px; color:var(--muted); font-family:var(--mono); margin-top:3px; }
  .arch-arrow {
    display:flex; flex-direction:column; align-items:center;
    padding: 0 8px; flex-shrink:0;
  }
  .arch-arrow-line {
    width:40px; height:2px; background:var(--border); position:relative;
  }
  .arch-arrow-line::after {
    content:'▶'; position:absolute; right:-8px; top:50%; transform:translateY(-50%);
    font-size:8px; color:var(--muted);
  }
  .arch-arrow-label { font-family:var(--mono); font-size:9px; color:var(--muted); margin-top:6px; letter-spacing:1px; }

  /* Scrollbar */
  ::-webkit-scrollbar { width:6px; }
  ::-webkit-scrollbar-track { background:transparent; }
  ::-webkit-scrollbar-thumb { background:var(--border); border-radius:3px; }
  ::-webkit-scrollbar-thumb:hover { background:var(--muted); }

  /* Progress bar */
  .progress-bar {
    position:fixed; top:0; left:260px; right:0; height:2px;
    background:var(--border); z-index:200;
  }
  .progress-fill {
    height:100%;
    background:linear-gradient(90deg, var(--accent), var(--accent2));
    width:0; transition:width 0.1s;
  }

  @media (max-width:768px) {
    .sidebar { transform:translateX(-100%); }
    .main { margin-left:0; }
    .progress-bar { left:0; }
    .section { padding:60px 24px; }
    .stats-strip { grid-template-columns:1fr 1fr; }
    .feature-list { grid-template-columns:1fr; }
    .arch { flex-direction:column; }
    .arch-arrow { transform:rotate(90deg); }
  }
</style>
</head>
<body>

<div class="glow-orb orb1"></div>
<div class="glow-orb orb2"></div>

<!-- Progress bar -->
<div class="progress-bar"><div class="progress-fill" id="progress"></div></div>

<!-- Sidebar -->
<aside class="sidebar">
  <div class="sidebar-header">
    <div class="sidebar-logo">// README.md</div>
    <div class="sidebar-title">Loan<span>IQ</span></div>
    <div class="sidebar-badge">ML · v1.0</div>
  </div>
  <nav class="nav-links">
    <div class="nav-section-label">Overview</div>
    <a class="nav-link active" href="#hero">
      <span class="nav-icon">⌂</span> Introduction
    </a>
    <a class="nav-link" href="#pipeline">
      <span class="nav-icon">⟳</span> ML Pipeline
    </a>
    <div class="nav-section-label">Details</div>
    <a class="nav-link" href="#models">
      <span class="nav-icon">◈</span> Models
    </a>
    <a class="nav-link" href="#webapp">
      <span class="nav-icon">◻</span> Web App
    </a>
    <a class="nav-link" href="#setup">
      <span class="nav-icon">⚙</span> Setup
    </a>
    <a class="nav-link" href="#architecture">
      <span class="nav-icon">⬡</span> Architecture
    </a>
  </nav>
  <div class="sidebar-footer">
    Built with Python · scikit-learn<br>
    Random Forest · Best Accuracy
  </div>
</aside>

<!-- Main content -->
<main class="main">

  <!-- HERO -->
  <section class="section" id="hero">
    <div class="hero-content">
      <div class="section-eyebrow">Machine Learning Project</div>
      <h1 class="hero-title">
        Loan<span class="highlight">IQ</span><br>Approval<br>Predictor
      </h1>
      <p class="hero-desc">
        A full-stack machine learning project that predicts loan approval outcomes using 5 classification algorithms, paired with a sleek interactive web interface for real-time predictions.
      </p>

      <div class="hero-files">
        <div class="file-chip" onclick="scrollTo('pipeline')">
          <span class="file-icon">📓</span>
          <div class="file-info">
            <div class="file-name">PROJECT.ipynb</div>
            <div class="file-type">Jupyter Notebook · ML Pipeline</div>
          </div>
        </div>
        <div class="file-chip" onclick="scrollTo('webapp')">
          <span class="file-icon">🌐</span>
          <div class="file-info">
            <div class="file-name">loan_approval_app.html</div>
            <div class="file-type">HTML · Frontend App</div>
          </div>
        </div>
      </div>

      <div class="stats-strip">
        <div class="stat-box">
          <span class="stat-num" data-target="5">0</span>
          <div class="stat-label">ML Models Trained</div>
        </div>
        <div class="stat-box">
          <span class="stat-num" data-target="8">0</span>
          <div class="stat-label">Pipeline Steps</div>
        </div>
        <div class="stat-box">
          <span class="stat-num" data-target="51">0</span>
          <div class="stat-label">Notebook Cells</div>
        </div>
        <div class="stat-box">
          <span class="stat-num" data-suffix="k">1</span>
          <div class="stat-label">Lines of Code</div>
        </div>
      </div>
    </div>
  </section>

  <!-- PIPELINE -->
  <section class="section" id="pipeline">
    <div class="section-eyebrow">PROJECT.ipynb</div>
    <h2 class="section-title">ML Pipeline</h2>
    <p class="section-subtitle">Click each step to explore the methodology and techniques used throughout the notebook.</p>

    <div class="pipeline">

      <div class="pipeline-step" onclick="toggleStep(this)">
        <div class="step-spine">
          <div class="step-dot"></div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <div class="step-header">
            <span class="step-num">STEP 01</span>
            <span class="step-title">Exploratory Data Analysis</span>
            <span class="step-tag">EDA</span>
          </div>
          <div class="step-body">
            <div class="step-body-inner">
              Full dataset inspection covering shape, types, null values, and statistical summaries. Visualizations include target distribution (slight imbalance — majority approved), credit history vs loan status, applicant income distribution, and a pre-encoding correlation heatmap.
              <div class="step-tags">
                <span class="tag">sns.countplot</span>
                <span class="tag">sns.histplot</span>
                <span class="tag">sns.heatmap</span>
                <span class="tag">df.describe()</span>
                <span class="tag">df.isnull().sum()</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="pipeline-step" onclick="toggleStep(this)">
        <div class="step-spine">
          <div class="step-dot"></div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <div class="step-header">
            <span class="step-num">STEP 02</span>
            <span class="step-title">Data Cleaning</span>
            <span class="step-tag">Preprocessing</span>
          </div>
          <div class="step-body">
            <div class="step-body-inner">
              Missing values handled strategically: categorical columns (Gender, Married, Dependents, Self_Employed) filled with <strong>mode</strong>; numerical columns (LoanAmount) filled with <strong>median</strong>; Loan_Amount_Term and Credit_History filled with mode. Zero nulls remain post-cleaning.
              <div class="step-tags">
                <span class="tag">fillna(mode)</span>
                <span class="tag">fillna(median)</span>
                <span class="tag">Cat. → Mode</span>
                <span class="tag">Num. → Median</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="pipeline-step" onclick="toggleStep(this)">
        <div class="step-spine">
          <div class="step-dot"></div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <div class="step-header">
            <span class="step-num">STEP 03</span>
            <span class="step-title">Feature Engineering & Encoding</span>
            <span class="step-tag">Transform</span>
          </div>
          <div class="step-body">
            <div class="step-body-inner">
              Created <code style="color:var(--accent);font-family:var(--mono)">Total_Income</code> by combining ApplicantIncome + CoapplicantIncome, then dropped the originals to reduce dimensionality. All categorical columns encoded with <strong>LabelEncoder</strong> for model compatibility.
              <div class="step-tags">
                <span class="tag">Total_Income</span>
                <span class="tag">LabelEncoder</span>
                <span class="tag">Feature Merge</span>
                <span class="tag">Dimensionality</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="pipeline-step" onclick="toggleStep(this)">
        <div class="step-spine">
          <div class="step-dot"></div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <div class="step-header">
            <span class="step-num">STEP 04</span>
            <span class="step-title">Train-Test Split & Scaling</span>
            <span class="step-tag">Split</span>
          </div>
          <div class="step-body">
            <div class="step-body-inner">
              Stratified 80/20 split preserves class distribution in both sets. StandardScaler applied <em>after</em> splitting (fit on train, transform both) to prevent data leakage. Scaling ensures no single feature dominates model optimization.
              <div class="step-tags">
                <span class="tag">80 / 20 Split</span>
                <span class="tag">Stratified</span>
                <span class="tag">StandardScaler</span>
                <span class="tag">No Data Leakage</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="pipeline-step" onclick="toggleStep(this)">
        <div class="step-spine">
          <div class="step-dot"></div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <div class="step-header">
            <span class="step-num">STEP 05</span>
            <span class="step-title">Model Training</span>
            <span class="step-tag">ML</span>
          </div>
          <div class="step-body">
            <div class="step-body-inner">
              Five classifiers trained and compared: Logistic Regression, Decision Tree, Random Forest, K-Nearest Neighbors, and SVM. All managed via a model dictionary loop for clean, DRY code. Logistic Regression required max_iter=5000 due to convergence issues.
              <div class="step-tags">
                <span class="tag">LogisticRegression</span>
                <span class="tag">DecisionTree</span>
                <span class="tag">RandomForest</span>
                <span class="tag">KNN</span>
                <span class="tag">SVM</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="pipeline-step" onclick="toggleStep(this)">
        <div class="step-spine">
          <div class="step-dot"></div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <div class="step-header">
            <span class="step-num">STEP 06</span>
            <span class="step-title">Evaluation — Accuracy & Confusion Matrix</span>
            <span class="step-tag">Eval</span>
          </div>
          <div class="step-body">
            <div class="step-body-inner">
              Each model evaluated with accuracy score and confusion matrix to understand true/false positives and negatives. ROC curve plotted for all 5 models on a single graph to compare discriminative ability visually.
              <div class="step-tags">
                <span class="tag">accuracy_score</span>
                <span class="tag">confusion_matrix</span>
                <span class="tag">roc_curve</span>
                <span class="tag">roc_auc_score</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="pipeline-step" onclick="toggleStep(this)">
        <div class="step-spine">
          <div class="step-dot"></div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <div class="step-header">
            <span class="step-num">STEP 07</span>
            <span class="step-title">Cross Validation</span>
            <span class="step-tag">Validation</span>
          </div>
          <div class="step-body">
            <div class="step-body-inner">
              5-fold cross validation applied to all models on scaled data for robust performance estimation. Eliminates over-fitting bias from a single train-test split. Mean CV score compared across models.
              <div class="step-tags">
                <span class="tag">cross_val_score</span>
                <span class="tag">cv=5</span>
                <span class="tag">Mean Score</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="pipeline-step" onclick="toggleStep(this)">
        <div class="step-spine">
          <div class="step-dot"></div>
          <div class="step-line"></div>
        </div>
        <div class="step-content">
          <div class="step-header">
            <span class="step-num">STEP 08</span>
            <span class="step-title">Results & Best Model Selection</span>
            <span class="step-tag">🏆 Winner</span>
          </div>
          <div class="step-body">
            <div class="step-body-inner">
              Bar chart comparing all model accuracies. <strong style="color:var(--accent3)">Random Forest Classifier</strong> emerged as the best-performing model — highest accuracy, best AUC score, and strongest cross-validation results. Selected as the production model.
              <div class="step-tags">
                <span class="tag" style="border-color:rgba(16,185,129,0.4);color:var(--accent3)">🏆 Random Forest</span>
                <span class="tag">Accuracy Comparison</span>
                <span class="tag">Final Model</span>
              </div>
            </div>
          </div>
        </div>
      </div>

    </div>
  </section>

  <!-- MODELS -->
  <section class="section" id="models">
    <div class="section-eyebrow">Algorithms</div>
    <h2 class="section-title">Models Compared</h2>
    <p class="section-subtitle">Five classification algorithms were trained and evaluated. Random Forest achieved the best overall performance.</p>

    <div class="models-grid" id="modelsGrid">
      <div class="model-card winner">
        <div class="winner-badge">BEST</div>
        <span class="model-icon">🌲</span>
        <div class="model-name">Random Forest</div>
        <div class="model-abbr">RF · Ensemble</div>
        <div class="accuracy-bar"><div class="accuracy-fill" data-acc="92"></div></div>
        <div class="accuracy-label"><span>Accuracy</span><span>~92%</span></div>
      </div>
      <div class="model-card">
        <span class="model-icon">📈</span>
        <div class="model-name">Logistic Regression</div>
        <div class="model-abbr">LR · Linear</div>
        <div class="accuracy-bar"><div class="accuracy-fill" data-acc="80"></div></div>
        <div class="accuracy-label"><span>Accuracy</span><span>~80%</span></div>
      </div>
      <div class="model-card">
        <span class="model-icon">🌿</span>
        <div class="model-name">Decision Tree</div>
        <div class="model-abbr">DT · Tree-based</div>
        <div class="accuracy-bar"><div class="accuracy-fill" data-acc="76"></div></div>
        <div class="accuracy-label"><span>Accuracy</span><span>~76%</span></div>
      </div>
      <div class="model-card">
        <span class="model-icon">🔵</span>
        <div class="model-name">K-Nearest Neighbors</div>
        <div class="model-abbr">KNN · Instance-based</div>
        <div class="accuracy-bar"><div class="accuracy-fill" data-acc="72"></div></div>
        <div class="accuracy-label"><span>Accuracy</span><span>~72%</span></div>
      </div>
      <div class="model-card">
        <span class="model-icon">⚡</span>
        <div class="model-name">Support Vector Machine</div>
        <div class="model-abbr">SVM · Kernel-based</div>
        <div class="accuracy-bar"><div class="accuracy-fill" data-acc="82"></div></div>
        <div class="accuracy-label"><span>Accuracy</span><span>~82%</span></div>
      </div>
    </div>
  </section>

  <!-- WEB APP -->
  <section class="section" id="webapp">
    <div class="section-eyebrow">loan_approval_app.html</div>
    <h2 class="section-title">Web App</h2>
    <p class="section-subtitle">A standalone dark-themed frontend for submitting loan applications and viewing predictions instantly.</p>

    <div class="tabs">
      <div class="tab active" onclick="switchTab('features', this)">Features</div>
      <div class="tab" onclick="switchTab('inputs', this)">Input Fields</div>
      <div class="tab" onclick="switchTab('usage', this)">Usage</div>
    </div>

    <div class="tab-content active" id="tab-features">
      <div class="feature-list">
        <div class="feature-item">
          <span class="feature-icon">🎨</span>
          <div class="feature-text">
            <div class="feature-title">Dark Glassmorphism UI</div>
            <div class="feature-desc">Polished dark theme with ambient glows, animated grid, and backdrop blur effects.</div>
          </div>
        </div>
        <div class="feature-item">
          <span class="feature-icon">⚡</span>
          <div class="feature-text">
            <div class="feature-title">Zero Dependencies</div>
            <div class="feature-desc">Pure HTML/CSS/JS — no frameworks, no server needed. Open directly in any browser.</div>
          </div>
        </div>
        <div class="feature-item">
          <span class="feature-icon">🔮</span>
          <div class="feature-text">
            <div class="feature-title">Instant Predictions</div>
            <div class="feature-desc">Submit the form to get an immediate approval or rejection result with visual feedback.</div>
          </div>
        </div>
        <div class="feature-item">
          <span class="feature-icon">📱</span>
          <div class="feature-text">
            <div class="feature-title">Responsive Layout</div>
            <div class="feature-desc">Works across desktop and mobile with a clean two-column hero layout.</div>
          </div>
        </div>
      </div>
    </div>

    <div class="tab-content" id="tab-inputs">
      <div class="feature-list">
        <div class="feature-item"><span class="feature-icon">👤</span><div class="feature-text"><div class="feature-title">Gender</div><div class="feature-desc">Male / Female</div></div></div>
        <div class="feature-item"><span class="feature-icon">💍</span><div class="feature-text"><div class="feature-title">Married</div><div class="feature-desc">Yes / No</div></div></div>
        <div class="feature-item"><span class="feature-icon">👨‍👩‍👧</span><div class="feature-text"><div class="feature-title">Dependents</div><div class="feature-desc">0 / 1 / 2 / 3+</div></div></div>
        <div class="feature-item"><span class="feature-icon">🎓</span><div class="feature-text"><div class="feature-title">Education</div><div class="feature-desc">Graduate / Not Graduate</div></div></div>
        <div class="feature-item"><span class="feature-icon">💼</span><div class="feature-text"><div class="feature-title">Employment</div><div class="feature-desc">Self-employed / Salaried</div></div></div>
        <div class="feature-item"><span class="feature-icon">💰</span><div class="feature-text"><div class="feature-title">Income</div><div class="feature-desc">Applicant + Co-applicant monthly</div></div></div>
        <div class="feature-item"><span class="feature-icon">🏦</span><div class="feature-text"><div class="feature-title">Loan Details</div><div class="feature-desc">Amount + Term (months)</div></div></div>
        <div class="feature-item"><span class="feature-icon">📋</span><div class="feature-text"><div class="feature-title">Credit History</div><div class="feature-desc">0 = Bad, 1 = Good</div></div></div>
      </div>
    </div>

    <div class="tab-content" id="tab-usage">
      <div class="setup-steps">
        <div class="setup-step">
          <div class="setup-step-num">1</div>
          <div class="setup-step-content">
            <div class="setup-step-title">Open the file</div>
            <div class="setup-step-desc">Double-click <code style="font-family:var(--mono);color:var(--accent)">loan_approval_app.html</code> or drag it into any modern browser (Chrome, Firefox, Edge, Safari).</div>
          </div>
        </div>
        <div class="setup-step">
          <div class="setup-step-num">2</div>
          <div class="setup-step-content">
            <div class="setup-step-title">Fill in applicant details</div>
            <div class="setup-step-desc">Complete all fields in the form — personal info, income details, loan amount and term, credit history, and property area.</div>
          </div>
        </div>
        <div class="setup-step">
          <div class="setup-step-num">3</div>
          <div class="setup-step-content">
            <div class="setup-step-title">Click Predict</div>
            <div class="setup-step-desc">The app returns an instant approval/rejection result with color-coded visual feedback.</div>
          </div>
        </div>
        <div class="setup-step">
          <div class="setup-step-num">4</div>
          <div class="setup-step-content">
            <div class="setup-step-title">Connect to backend (optional)</div>
            <div class="setup-step-desc">To use real ML predictions, connect the form to a Flask or FastAPI backend that serves inference from the saved Random Forest model.</div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- SETUP -->
  <section class="section" id="setup">
    <div class="section-eyebrow">Getting Started</div>
    <h2 class="section-title">Setup</h2>
    <p class="section-subtitle">Get the notebook running in minutes.</p>

    <div class="code-block">
      <div class="code-header">
        <div class="code-dots">
          <div class="code-dot"></div><div class="code-dot"></div><div class="code-dot"></div>
        </div>
        <span class="code-lang">BASH</span>
        <span class="copy-btn" onclick="copyCode(this, 'pip install pandas numpy matplotlib seaborn scikit-learn jupyter')">Copy</span>
      </div>
      <div class="code-body">
        <span class="cm"># Install dependencies</span><br>
        <span class="fn">pip</span> install pandas numpy matplotlib seaborn scikit-learn jupyter<br><br>
        <span class="cm"># Launch Jupyter</span><br>
        <span class="fn">jupyter</span> notebook PROJECT.ipynb
      </div>
    </div>

    <div class="code-block">
      <div class="code-header">
        <div class="code-dots">
          <div class="code-dot"></div><div class="code-dot"></div><div class="code-dot"></div>
        </div>
        <span class="code-lang">PYTHON</span>
        <span class="copy-btn" onclick="copyCode(this, 'df = pd.read_csv(\'path/to/loan.csv\')')">Copy</span>
      </div>
      <div class="code-body">
        <span class="cm"># Update dataset path in the first cell</span><br>
        df = pd.<span class="fn">read_csv</span>(<span class="str">"path/to/loan.csv"</span>)<br><br>
        <span class="cm"># Run all cells top-to-bottom</span><br>
        <span class="cm"># Final cell prints model accuracy comparison</span>
      </div>
    </div>

    <div style="margin-top:24px;">
      <div style="font-family:var(--mono);font-size:12px;color:var(--muted);margin-bottom:16px;letter-spacing:1px;">DEPENDENCIES</div>
      <div style="display:flex;flex-wrap:wrap;gap:10px;">
        <span class="tag" style="color:var(--text)">Python 3.7+</span>
        <span class="tag" style="color:var(--text)">pandas</span>
        <span class="tag" style="color:var(--text)">numpy</span>
        <span class="tag" style="color:var(--text)">matplotlib</span>
        <span class="tag" style="color:var(--text)">seaborn</span>
        <span class="tag" style="color:var(--text)">scikit-learn</span>
        <span class="tag" style="color:var(--text)">jupyter</span>
      </div>
    </div>
  </section>

  <!-- ARCHITECTURE -->
  <section class="section" id="architecture">
    <div class="section-eyebrow">System Design</div>
    <h2 class="section-title">Architecture</h2>
    <p class="section-subtitle">How the two project files connect in a complete loan approval pipeline.</p>

    <div class="arch">
      <div class="arch-node">
        <span class="arch-icon">📄</span>
        <div class="arch-label">loan.csv</div>
        <div class="arch-sublabel">Raw Dataset</div>
      </div>
      <div class="arch-arrow">
        <div class="arch-arrow-line"></div>
        <div class="arch-arrow-label">INPUT</div>
      </div>
      <div class="arch-node">
        <span class="arch-icon">📓</span>
        <div class="arch-label">PROJECT.ipynb</div>
        <div class="arch-sublabel">EDA + Training</div>
      </div>
      <div class="arch-arrow">
        <div class="arch-arrow-line"></div>
        <div class="arch-arrow-label">OUTPUT</div>
      </div>
      <div class="arch-node highlight">
        <span class="arch-icon">🌲</span>
        <div class="arch-label">Random Forest</div>
        <div class="arch-sublabel">Best Model</div>
      </div>
      <div class="arch-arrow">
        <div class="arch-arrow-line"></div>
        <div class="arch-arrow-label">SERVE</div>
      </div>
      <div class="arch-node">
        <span class="arch-icon">⚙️</span>
        <div class="arch-label">Flask / FastAPI</div>
        <div class="arch-sublabel">Backend (Optional)</div>
      </div>
      <div class="arch-arrow">
        <div class="arch-arrow-line"></div>
        <div class="arch-arrow-label">API</div>
      </div>
      <div class="arch-node">
        <span class="arch-icon">🌐</span>
        <div class="arch-label">loan_approval_app</div>
        <div class="arch-sublabel">Frontend UI</div>
      </div>
    </div>

    <div style="margin-top:48px;padding:24px;background:var(--surface);border:1px solid var(--border);border-radius:16px;">
      <div style="font-family:var(--mono);font-size:11px;color:var(--accent);letter-spacing:2px;margin-bottom:16px;">// NOTES</div>
      <div style="font-size:14px;color:var(--muted);line-height:1.8;">
        The HTML frontend currently runs as a UI demo without a live backend connection.
        To power it with real predictions, export the trained Random Forest model using
        <code style="font-family:var(--mono);color:var(--accent)">joblib.dump()</code> or
        <code style="font-family:var(--mono);color:var(--accent)">pickle</code>,
        serve it via Flask/FastAPI, and point the form's submit handler to the inference endpoint.
      </div>
    </div>
  </section>

</main>

<script>
  // Scroll progress
  window.addEventListener('scroll', () => {
    const el = document.documentElement;
    const pct = (el.scrollTop / (el.scrollHeight - el.clientHeight)) * 100;
    document.getElementById('progress').style.width = pct + '%';
  });

  // Active nav link
  const sections = document.querySelectorAll('.section');
  const navLinks = document.querySelectorAll('.nav-link');

  const observer = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        navLinks.forEach(l => l.classList.remove('active'));
        const link = document.querySelector(`.nav-link[href="#${entry.target.id}"]`);
        if (link) link.classList.add('active');
      }
    });
  }, { threshold: 0.4 });

  sections.forEach(s => observer.observe(s));

  // Section reveal on scroll
  const revealObserver = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) entry.target.classList.add('visible');
    });
  }, { threshold: 0.1 });
  sections.forEach(s => revealObserver.observe(s));

  // Counter animation
  function animateCounter(el) {
    const target = parseInt(el.dataset.target);
    const suffix = el.dataset.suffix || '';
    if (!target) return;
    let current = 0;
    const step = target / 40;
    const timer = setInterval(() => {
      current += step;
      if (current >= target) { current = target; clearInterval(timer); }
      el.textContent = Math.floor(current) + suffix;
    }, 30);
  }

  const counterObserver = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.querySelectorAll('[data-target]').forEach(animateCounter);
      }
    });
  }, { threshold: 0.5 });
  counterObserver.observe(document.getElementById('hero'));

  // Accuracy bar animation
  const barObserver = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.querySelectorAll('.accuracy-fill').forEach(bar => {
          setTimeout(() => {
            bar.style.width = bar.dataset.acc + '%';
          }, 200);
        });
      }
    });
  }, { threshold: 0.3 });
  barObserver.observe(document.getElementById('models'));

  // Pipeline toggle
  function toggleStep(step) {
    const body = step.querySelector('.step-body');
    const isOpen = step.classList.contains('open');
    document.querySelectorAll('.pipeline-step').forEach(s => {
      s.classList.remove('open');
      s.querySelector('.step-body').classList.remove('open');
    });
    if (!isOpen) {
      step.classList.add('open');
      body.classList.add('open');
    }
  }

  // Tab switcher
  function switchTab(id, tabEl) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.getElementById('tab-' + id).classList.add('active');
    tabEl.classList.add('active');
  }

  // Copy code
  function copyCode(btn, text) {
    navigator.clipboard.writeText(text).then(() => {
      const orig = btn.textContent;
      btn.textContent = 'Copied!';
      btn.style.color = 'var(--accent3)';
      setTimeout(() => { btn.textContent = orig; btn.style.color = ''; }, 2000);
    });
  }

  // Smooth scroll
  function scrollTo(id) {
    document.getElementById(id).scrollIntoView({ behavior:'smooth' });
  }
  document.querySelectorAll('.nav-link').forEach(link => {
    link.addEventListener('click', e => {
      e.preventDefault();
      const id = link.getAttribute('href').slice(1);
      document.getElementById(id).scrollIntoView({ behavior:'smooth' });
    });
  });
</script>
</body>
</html>
