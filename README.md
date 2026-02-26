<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Comprehensive Guide to Google NotebookLM — First Edition</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link href="https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400;0,600;1,400&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet" />
  <style>
    :root {
      --blue:       #2563eb;
      --blue-light: #eff6ff;
      --blue-mid:   #bfdbfe;
      --blue-dark:  #1d4ed8;
      --text:       #1e293b;
      --muted:      #64748b;
      --border:     #e2e8f0;
      --bg:         #f8faff;
      --white:      #ffffff;
      --gold:       #d97706;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html { scroll-behavior: smooth; }

    body {
      font-family: 'Inter', sans-serif;
      color: var(--text);
      background: var(--white);
      overflow-x: hidden;
    }

    /* NAV */
    nav {
      background: var(--white);
      border-bottom: 1px solid var(--border);
      padding: 15px 24px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      position: sticky;
      top: 0;
      z-index: 100;
    }
    .nav-brand { font-size: 0.88rem; font-weight: 600; color: var(--blue); text-decoration: none; }
    .nav-dl {
      font-size: 0.84rem; font-weight: 600; color: var(--blue);
      text-decoration: none; border: 1.5px solid var(--blue-mid);
      background: var(--blue-light); padding: 7px 16px; border-radius: 8px; transition: all 0.2s;
    }
    .nav-dl:hover { background: var(--blue-mid); }

    /* HERO */
    .hero {
      background: var(--bg);
      border-bottom: 1px solid var(--border);
      padding: 72px 24px 80px;
      text-align: center;
      position: relative;
      overflow: hidden;
    }
    .hero::before {
      content: '';
      position: absolute; top: -100px; left: 50%; transform: translateX(-50%);
      width: 700px; height: 350px;
      background: radial-gradient(ellipse, rgba(37,99,235,0.07) 0%, transparent 70%);
      pointer-events: none;
    }

    .badge-edition {
      display: inline-flex; align-items: center; gap: 6px;
      background: #fffbeb; border: 1px solid #fde68a; color: var(--gold);
      font-size: 0.74rem; font-weight: 700; letter-spacing: 0.1em;
      text-transform: uppercase; padding: 5px 14px; border-radius: 100px;
      margin-bottom: 12px;
      animation: fadeUp 0.5s ease both;
    }

    .badge-oe {
      display: inline-block;
      background: var(--blue-light); color: var(--blue);
      font-size: 0.74rem; font-weight: 600; letter-spacing: 0.1em;
      text-transform: uppercase; padding: 5px 14px; border-radius: 100px;
      margin-bottom: 24px;
      animation: fadeUp 0.5s ease 0.1s both;
    }

    .hero h1 {
      font-family: 'Lora', serif;
      font-size: clamp(2.2rem, 5.5vw, 3.5rem);
      color: var(--text); line-height: 1.2;
      max-width: 700px; margin: 0 auto 10px;
      animation: fadeUp 0.6s ease 0.15s both;
    }
    .hero h1 span { color: var(--blue); }

    .hero-sub-title {
      font-family: 'Lora', serif; font-style: italic;
      font-size: 1.05rem; color: var(--muted);
      margin-bottom: 18px;
      animation: fadeUp 0.6s ease 0.2s both;
    }

    .hero p {
      font-size: 1rem; color: var(--muted);
      max-width: 500px; margin: 0 auto 36px;
      line-height: 1.75; font-weight: 400;
      animation: fadeUp 0.6s ease 0.25s both;
    }

    .hero-actions {
      display: flex; justify-content: center; gap: 12px; flex-wrap: wrap;
      animation: fadeUp 0.6s ease 0.3s both;
    }

    .btn {
      display: inline-flex; align-items: center; gap: 8px;
      padding: 13px 26px; border-radius: 9px;
      font-size: 0.92rem; font-weight: 600;
      text-decoration: none; transition: all 0.2s ease;
      border: none; cursor: pointer;
    }
    .btn-primary {
      background: var(--blue); color: white;
      box-shadow: 0 2px 10px rgba(37,99,235,0.25);
    }
    .btn-primary:hover { background: var(--blue-dark); transform: translateY(-2px); box-shadow: 0 6px 20px rgba(37,99,235,0.3); }
    .btn-ghost {
      background: var(--white); color: var(--text);
      border: 1.5px solid var(--border);
    }
    .btn-ghost:hover { border-color: var(--blue); color: var(--blue); transform: translateY(-2px); }

    .stats-row {
      display: flex; justify-content: center;
      background: var(--white); border: 1px solid var(--border);
      border-radius: 14px; max-width: 520px;
      margin: 52px auto 0; overflow: hidden;
      animation: fadeUp 0.6s ease 0.35s both;
    }
    .stat { flex: 1; padding: 18px 12px; text-align: center; border-right: 1px solid var(--border); }
    .stat:last-child { border-right: none; }
    .stat-n { font-family: 'Lora', serif; font-size: 1.65rem; color: var(--blue); display: block; line-height: 1; margin-bottom: 4px; }
    .stat-l { font-size: 0.7rem; color: var(--muted); text-transform: uppercase; letter-spacing: 0.08em; font-weight: 500; }

    /* SECTIONS */
    .section { padding: 76px 24px; }
    .section-alt { background: var(--bg); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); }
    .container { max-width: 960px; margin: 0 auto; }

    .sec-label { font-size: 0.7rem; font-weight: 700; letter-spacing: 0.14em; text-transform: uppercase; color: var(--blue); margin-bottom: 10px; }
    .sec-title { font-family: 'Lora', serif; font-size: clamp(1.65rem, 3.5vw, 2.2rem); color: var(--text); margin-bottom: 12px; line-height: 1.25; }
    .sec-sub { font-size: 0.97rem; color: var(--muted); max-width: 500px; line-height: 1.7; }

    /* ABOUT GRID */
    .about-grid { display: grid; grid-template-columns: 1.1fr 0.9fr; gap: 52px; align-items: start; margin-top: 44px; }
    .about-text p { color: var(--muted); line-height: 1.8; margin-bottom: 16px; font-size: 0.96rem; }

    .feature-list { display: flex; flex-direction: column; gap: 12px; }
    .feature-item {
      display: flex; gap: 14px; align-items: flex-start;
      background: var(--white); border: 1px solid var(--border);
      border-radius: 11px; padding: 16px 18px; transition: border-color 0.2s;
    }
    .feature-item:hover { border-color: var(--blue-mid); }
    .fi-icon { font-size: 1.25rem; flex-shrink: 0; margin-top: 1px; }
    .feature-item h4 { font-size: 0.9rem; font-weight: 600; color: var(--text); margin-bottom: 3px; }
    .feature-item p { font-size: 0.81rem; color: var(--muted); line-height: 1.5; }

    /* CHAPTERS */
    .chapters-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(270px, 1fr)); gap: 14px; margin-top: 44px; }
    .chapter-card {
      background: var(--white); border: 1px solid var(--border);
      border-radius: 11px; padding: 20px 22px; transition: all 0.2s;
    }
    .chapter-card:hover { border-color: var(--blue-mid); box-shadow: 0 4px 18px rgba(37,99,235,0.08); transform: translateY(-2px); }
    .ch-num { font-size: 0.68rem; font-weight: 700; letter-spacing: 0.1em; text-transform: uppercase; color: var(--blue); margin-bottom: 6px; }
    .chapter-card h3 { font-size: 0.91rem; font-weight: 600; color: var(--text); margin-bottom: 5px; line-height: 1.4; }
    .chapter-card p { font-size: 0.8rem; color: var(--muted); line-height: 1.6; }

    /* AUDIENCE */
    .audience-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 18px; margin-top: 44px; }
    .audience-card {
      border: 1px solid var(--border); border-radius: 14px;
      padding: 30px 24px; background: var(--white); transition: all 0.2s;
    }
    .audience-card:hover { box-shadow: 0 8px 28px rgba(37,99,235,0.09); transform: translateY(-3px); border-color: var(--blue-mid); }
    .aud-icon { font-size: 2rem; display: block; margin-bottom: 14px; }
    .audience-card h3 { font-family: 'Lora', serif; font-size: 1.2rem; color: var(--text); margin-bottom: 10px; }
    .audience-card p { font-size: 0.85rem; color: var(--muted); line-height: 1.7; }

    /* AUTHOR */
    .author-card {
      background: var(--white); border: 1px solid var(--border);
      border-radius: 18px; padding: 38px 44px;
      display: flex; align-items: center; gap: 32px;
    }
    .author-av {
      width: 76px; height: 76px; border-radius: 50%;
      background: linear-gradient(135deg, #2563eb, #0891b2);
      display: flex; align-items: center; justify-content: center;
      font-family: 'Lora', serif; font-size: 1.8rem; color: white; flex-shrink: 0;
    }
    .author-info h3 { font-family: 'Lora', serif; font-size: 1.3rem; color: var(--text); margin-bottom: 4px; }
    .author-role { font-size: 0.82rem; color: var(--blue); font-weight: 500; margin-bottom: 10px; }
    .author-info p { font-size: 0.87rem; color: var(--muted); line-height: 1.7; }
    .author-links { display: flex; gap: 8px; margin-top: 14px; flex-wrap: wrap; }
    .author-link {
      font-size: 0.76rem; color: var(--blue); text-decoration: none;
      border: 1px solid var(--blue-mid); background: var(--blue-light);
      padding: 4px 12px; border-radius: 100px; font-weight: 500; transition: all 0.2s;
    }
    .author-link:hover { background: var(--blue-mid); }

    /* CTA */
    .cta-section {
      background: var(--blue-light);
      border-top: 1px solid var(--blue-mid);
      border-bottom: 1px solid var(--blue-mid);
      padding: 76px 24px; text-align: center;
    }
    .cta-edition {
      display: inline-block; background: white; border: 1px solid var(--blue-mid);
      color: var(--blue); font-size: 0.76rem; font-weight: 600;
      padding: 5px 14px; border-radius: 100px; margin-bottom: 20px; letter-spacing: 0.06em;
    }
    .cta-section h2 { font-family: 'Lora', serif; font-size: clamp(1.65rem, 4vw, 2.3rem); color: var(--text); margin-bottom: 12px; }
    .cta-section p { color: var(--muted); font-size: 0.96rem; margin-bottom: 30px; max-width: 420px; margin-left: auto; margin-right: auto; line-height: 1.7; }
    .cta-actions { display: flex; gap: 12px; justify-content: center; flex-wrap: wrap; }

    /* FOOTER */
    footer { background: var(--white); border-top: 1px solid var(--border); padding: 26px 24px; text-align: center; }
    footer p { font-size: 0.82rem; color: var(--muted); }
    footer a { color: var(--blue); text-decoration: none; }
    footer a:hover { text-decoration: underline; }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(16px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    @media (max-width: 768px) {
      .about-grid { grid-template-columns: 1fr; gap: 28px; }
      .audience-grid { grid-template-columns: 1fr; }
      .author-card { flex-direction: column; text-align: center; padding: 28px 22px; gap: 18px; }
      .author-links { justify-content: center; }
    }
    @media (max-width: 500px) {
      .hero-actions, .cta-actions { flex-direction: column; align-items: center; }
      .btn { width: 100%; max-width: 290px; justify-content: center; }
      .stat { padding: 14px 8px; }
    }
  </style>
</head>
<body>

<!-- NAV -->
<nav>
  <a href="#" class="nav-brand">📓 NotebookLM Guide</a>
  <a href="NotebookLM_Comprehensive__Guide.pdf" class="nav-dl" download>↓ Download PDF</a>
</nav>

<!-- HERO -->
<header class="hero">
  <div class="badge-edition">★ First Edition · March 2026</div><br/>
  <span class="badge-oe">Open Educational Resource</span>
  <h1>Comprehensive Guide to<br/><span>Google NotebookLM</span></h1>
  <p class="hero-sub-title">For Educators, Researchers &amp; Students</p>
  <p>A practical, 248-page reference — from prompting principles to advanced Studio customization. Free to download and share.</p>
  <div class="hero-actions">
    <a href="NotebookLM_Comprehensive__Guide.pdf" class="btn btn-primary" download>↓ Download PDF — Free</a>
    <a href="https://zaidalhuda.github.io/" class="btn btn-ghost" target="_blank">Author's Website ↗</a>
  </div>
  <div class="stats-row">
    <div class="stat"><span class="stat-n">248</span><span class="stat-l">Pages</span></div>
    <div class="stat"><span class="stat-n">13</span><span class="stat-l">Chapters</span></div>
    <div class="stat"><span class="stat-n">5</span><span class="stat-l">Appendices</span></div>
    <div class="stat"><span class="stat-n">Free</span><span class="stat-l">Open Access</span></div>
  </div>
</header>

<!-- ABOUT -->
<section class="section">
  <div class="container">
    <p class="sec-label">About the Guide</p>
    <h2 class="sec-title">Everything you need to master NotebookLM</h2>
    <p class="sec-sub">Eight parts, 13 chapters, and 5 appendices — designed to be immediately actionable for any academic context.</p>
    <div class="about-grid">
      <div class="about-text">
        <p>This first-edition guide takes you from the very basics of NotebookLM through to advanced, real-world workflows. Whether you're setting up your first notebook or orchestrating complex multi-source research, every section gives you the practical knowledge to move forward confidently.</p>
        <p>Written by Dr. Zaid Al-Huda and published in March 2026, the guide is screenshot-rich, filled with ready-to-use prompt templates, and grounded in real classroom and research use cases.</p>
        <a href="NotebookLM_Comprehensive__Guide.pdf" class="btn btn-primary" download style="margin-top: 10px;">↓ Download Free PDF</a>
      </div>
      <div class="feature-list">
        <div class="feature-item">
          <span class="fi-icon">🎯</span>
          <div>
            <h4>Source-Grounded AI</h4>
            <p>Understand how NotebookLM's RAG approach differs from ChatGPT, Gemini, and Claude — and why it matters for accuracy.</p>
          </div>
        </div>
        <div class="feature-item">
          <span class="fi-icon">📋</span>
          <div>
            <h4>Prompt Templates Library</h4>
            <p>Dozens of ready-to-use prompts for research, teaching, writing, and study — included in Appendix A.</p>
          </div>
        </div>
        <div class="feature-item">
          <span class="fi-icon">🎙️</span>
          <div>
            <h4>Studio Tools Deep Dive</h4>
            <p>Audio Overviews, Mind Maps, Study Guides, Flashcards — step-by-step instructions for every tool.</p>
          </div>
        </div>
        <div class="feature-item">
          <span class="fi-icon">⚖️</span>
          <div>
            <h4>Ethics &amp; Limitations</h4>
            <p>A candid look at what NotebookLM can and cannot do, and how to use it responsibly in academic settings.</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- CHAPTERS -->
<section class="section section-alt">
  <div class="container">
    <p class="sec-label">Contents</p>
    <h2 class="sec-title">What's inside — First Edition</h2>
    <p class="sec-sub">Thirteen chapters covering foundations, customization, Studio tools, collaboration, and audience-specific applications.</p>
    <div class="chapters-grid">
      <div class="chapter-card"><p class="ch-num">Part I · Ch. 1</p><h3>Introduction to NotebookLM</h3><p>What it is, how it differs from other AI tools, privacy policy, and free vs. paid plans.</p></div>
      <div class="chapter-card"><p class="ch-num">Part II · Ch. 2</p><h3>Prompting Principles</h3><p>Core prompting strategies, clarity techniques, and how to structure effective queries.</p></div>
      <div class="chapter-card"><p class="ch-num">Part II · Ch. 3</p><h3>Managing Sources</h3><p>PDFs, URLs, YouTube videos, Google Docs — uploading, organizing, and troubleshooting.</p></div>
      <div class="chapter-card"><p class="ch-num">Part II · Ch. 4</p><h3>The Chat Panel</h3><p>Your AI research companion — citations, saving responses, and the 2025 engine upgrade.</p></div>
      <div class="chapter-card"><p class="ch-num">Part III · Ch. 5</p><h3>Custom Goals, Personas &amp; Style</h3><p>Tailoring NotebookLM's behavior with custom instructions and output style settings.</p></div>
      <div class="chapter-card"><p class="ch-num">Part IV · Ch. 6</p><h3>Audio &amp; Video Overviews</h3><p>Generating podcast-style deep dives, debates, and critiques from your sources.</p></div>
      <div class="chapter-card"><p class="ch-num">Part IV · Ch. 7</p><h3>Mind Maps &amp; Reports</h3><p>Visualizing knowledge and generating structured documents — briefings, FAQs, slide decks.</p></div>
      <div class="chapter-card"><p class="ch-num">Part V · Ch. 8</p><h3>Learning Tools</h3><p>Flashcards, Study Guides, Timeline Documents, and integrated learning strategies.</p></div>
      <div class="chapter-card"><p class="ch-num">Part VI · Ch. 9</p><h3>Sharing &amp; Public Notebooks</h3><p>Private collaboration, public notebooks, featured notebooks, and usage analytics.</p></div>
      <div class="chapter-card"><p class="ch-num">Part VII · Ch. 10</p><h3>NotebookLM for Teachers</h3><p>Lesson planning, differentiation, assessment design, and classroom integration.</p></div>
      <div class="chapter-card"><p class="ch-num">Part VII · Ch. 11</p><h3>NotebookLM for Researchers</h3><p>Literature reviews, data analysis, grant writing, and research workflow optimization.</p></div>
      <div class="chapter-card"><p class="ch-num">Part VII · Ch. 12</p><h3>NotebookLM for Students</h3><p>Exam prep, essay writing, thesis support, and effective study techniques.</p></div>
      <div class="chapter-card"><p class="ch-num">Part VIII · Ch. 13</p><h3>Tips, Limitations &amp; Ethics</h3><p>Best practices, known limitations, ethical considerations, and the road ahead.</p></div>
      <div class="chapter-card"><p class="ch-num">Appendices A – E</p><h3>Reference Materials</h3><p>Prompt templates library, plan comparison, troubleshooting guide, glossary, and quick-start cheat sheet.</p></div>
    </div>
  </div>
</section>

<!-- AUDIENCE -->
<section class="section">
  <div class="container">
    <p class="sec-label">Who it's for</p>
    <h2 class="sec-title">Built for three audiences</h2>
    <p class="sec-sub">Each audience gets a dedicated chapter with tailored workflows, examples, and strategies.</p>
    <div class="audience-grid">
      <div class="audience-card"><span class="aud-icon">🎓</span><h3>Teachers</h3><p>Design richer lessons, create differentiated materials, build student-facing notebooks, and save hours of preparation time each week.</p></div>
      <div class="audience-card"><span class="aud-icon">🔬</span><h3>Researchers</h3><p>Manage literature at scale, synthesize findings across dozens of sources, and accelerate the writing of papers, grants, and reports.</p></div>
      <div class="audience-card"><span class="aud-icon">📖</span><h3>Students</h3><p>Master complex topics faster with AI-powered flashcards, audio overviews, and guided Q&amp;A from your own study materials.</p></div>
    </div>
  </div>
</section>

<!-- AUTHOR -->
<section class="section section-alt">
  <div class="container">
    <p class="sec-label">Author</p>
    <h2 class="sec-title">About the author</h2>
    <p class="sec-sub" style="margin-bottom: 32px;">Senior Lecturer and AI researcher committed to making cutting-edge tools accessible across academia.</p>
    <div class="author-card">
      <div class="author-av">Z</div>
      <div class="author-info">
        <h3>Dr. Zaid Al-Huda</h3>
        <p class="author-role">Senior Lecturer &amp; Distinguished Associate Research Fellow · Stirling College, Chengdu University (CDU)</p>
        <p>Specialist in Computer Vision, Explainable AI, and Deep Learning — 40+ journal articles, 1,000+ citations, H-index of 19. This first-edition guide reflects his commitment to making AI tools practical and accessible for the academic community.</p>
        <div class="author-links">
          <a href="https://zaidalhuda.github.io/" class="author-link" target="_blank">Website</a>
          <a href="https://scholar.google.com/citations?hl=en&user=ZTvKDLAAAAAJ" class="author-link" target="_blank">Google Scholar</a>
          <a href="https://www.linkedin.com/in/zaidal-huda/" class="author-link" target="_blank">LinkedIn</a>
          <a href="https://www.researchgate.net/profile/Zaid_Al-Huda" class="author-link" target="_blank">ResearchGate</a>
          <a href="https://orcid.org/0000-0002-8920-7635" class="author-link" target="_blank">ORCID</a>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- CTA -->
<section class="cta-section">
  <div class="cta-edition">✦ First Edition · March 2026</div>
  <h2>Download the full guide — free</h2>
  <p>248 pages. 13 chapters. 5 appendices. No sign-up required. Share freely with your students and colleagues.</p>
  <div class="cta-actions">
    <a href="NotebookLM_Comprehensive__Guide.pdf" class="btn btn-primary" download>↓ Download PDF</a>
    <a href="https://github.com/zaidalhuda/notebooklm-guide" class="btn btn-ghost" target="_blank">View on GitHub ↗</a>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <p>First Edition © 2026 <a href="https://zaidalhuda.github.io/" target="_blank">Dr. Zaid Al-Huda</a> &nbsp;·&nbsp; Free for educational use &nbsp;·&nbsp; <a href="https://github.com/zaidalhuda/notebooklm-guide" target="_blank">GitHub Repository</a></p>
</footer>

</body>
</html>
