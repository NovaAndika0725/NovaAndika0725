<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Portfolio — Full Stack Developer</title>
  <link rel="preconnect" href="https://fonts.googleapis.com"/>
  <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Mono:ital,wght@0,300;0,400;1,300&family=Inter:wght@300;400;500&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #080b10;
      --bg2: #0d1117;
      --bg3: #121820;
      --border: rgba(255,255,255,0.07);
      --accent: #00e5ff;
      --accent2: #7b61ff;
      --text: #e8eaf0;
      --muted: #5a6070;
      --card: #0f1520;
    }

    html { scroll-behavior: smooth; }

    body {
      font-family: 'Inter', sans-serif;
      background: var(--bg);
      color: var(--text);
      overflow-x: hidden;
      cursor: none;
    }

    /* Custom cursor */
    .cursor {
      width: 10px; height: 10px;
      background: var(--accent);
      border-radius: 50%;
      position: fixed; top: 0; left: 0;
      pointer-events: none;
      z-index: 9999;
      transition: transform 0.1s;
      mix-blend-mode: screen;
    }
    .cursor-ring {
      width: 36px; height: 36px;
      border: 1px solid rgba(0,229,255,0.4);
      border-radius: 50%;
      position: fixed; top: 0; left: 0;
      pointer-events: none;
      z-index: 9998;
      transition: transform 0.18s ease, width 0.2s, height 0.2s;
    }

    /* Noise overlay */
    body::before {
      content: '';
      position: fixed; inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: 1;
      opacity: 0.4;
    }

    /* NAV */
    nav {
      position: fixed; top: 0; left: 0; right: 0;
      z-index: 100;
      display: flex; align-items: center; justify-content: space-between;
      padding: 22px 60px;
      backdrop-filter: blur(20px);
      background: rgba(8,11,16,0.7);
      border-bottom: 1px solid var(--border);
    }

    .nav-logo {
      font-family: 'Syne', sans-serif;
      font-weight: 800;
      font-size: 1.2rem;
      letter-spacing: -0.02em;
      color: var(--text);
      text-decoration: none;
    }
    .nav-logo span { color: var(--accent); }

    .nav-links { display: flex; gap: 36px; list-style: none; }
    .nav-links a {
      font-family: 'DM Mono', monospace;
      font-size: 0.78rem;
      color: var(--muted);
      text-decoration: none;
      letter-spacing: 0.08em;
      transition: color 0.2s;
      position: relative;
    }
    .nav-links a::after {
      content: '';
      position: absolute; bottom: -4px; left: 0;
      width: 0; height: 1px;
      background: var(--accent);
      transition: width 0.3s;
    }
    .nav-links a:hover { color: var(--accent); }
    .nav-links a:hover::after { width: 100%; }

    .nav-cta {
      font-family: 'DM Mono', monospace;
      font-size: 0.75rem;
      color: var(--accent);
      border: 1px solid var(--accent);
      padding: 8px 20px;
      text-decoration: none;
      letter-spacing: 0.1em;
      transition: background 0.2s, color 0.2s;
    }
    .nav-cta:hover { background: var(--accent); color: var(--bg); }

    /* SECTIONS */
    section { position: relative; z-index: 2; }

    /* HERO */
    #hero {
      min-height: 100vh;
      display: flex; align-items: center;
      padding: 120px 60px 80px;
      position: relative;
      overflow: hidden;
    }

    .hero-grid-bg {
      position: absolute; inset: 0;
      background-image:
        linear-gradient(rgba(0,229,255,0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(0,229,255,0.03) 1px, transparent 1px);
      background-size: 60px 60px;
    }
    .hero-glow {
      position: absolute;
      width: 600px; height: 600px;
      background: radial-gradient(circle, rgba(0,229,255,0.06) 0%, transparent 70%);
      top: 50%; left: 60%;
      transform: translate(-50%,-50%);
      pointer-events: none;
    }
    .hero-glow2 {
      position: absolute;
      width: 400px; height: 400px;
      background: radial-gradient(circle, rgba(123,97,255,0.07) 0%, transparent 70%);
      top: 20%; left: 20%;
      pointer-events: none;
    }

    .hero-content { max-width: 800px; }

    .hero-tag {
      font-family: 'DM Mono', monospace;
      font-size: 0.75rem;
      color: var(--accent);
      letter-spacing: 0.2em;
      margin-bottom: 24px;
      opacity: 0;
      animation: fadeUp 0.6s 0.2s forwards;
    }

    h1 {
      font-family: 'Syne', sans-serif;
      font-weight: 800;
      font-size: clamp(3rem, 7vw, 6rem);
      line-height: 1;
      letter-spacing: -0.03em;
      margin-bottom: 32px;
      opacity: 0;
      animation: fadeUp 0.7s 0.35s forwards;
    }

    h1 .line2 {
      color: transparent;
      -webkit-text-stroke: 1px rgba(255,255,255,0.2);
    }
    h1 .accent-word { color: var(--accent); }

    .hero-desc {
      font-size: 1rem;
      line-height: 1.8;
      color: var(--muted);
      max-width: 560px;
      margin-bottom: 48px;
      opacity: 0;
      animation: fadeUp 0.7s 0.5s forwards;
    }

    .hero-actions {
      display: flex; gap: 16px; flex-wrap: wrap;
      opacity: 0;
      animation: fadeUp 0.7s 0.65s forwards;
    }

    .btn-primary {
      font-family: 'DM Mono', monospace;
      font-size: 0.8rem;
      letter-spacing: 0.1em;
      padding: 14px 32px;
      background: var(--accent);
      color: var(--bg);
      border: none;
      text-decoration: none;
      cursor: none;
      transition: transform 0.2s, box-shadow 0.2s;
    }
    .btn-primary:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 30px rgba(0,229,255,0.25);
    }

    .btn-secondary {
      font-family: 'DM Mono', monospace;
      font-size: 0.8rem;
      letter-spacing: 0.1em;
      padding: 14px 32px;
      background: transparent;
      color: var(--text);
      border: 1px solid var(--border);
      text-decoration: none;
      cursor: none;
      transition: border-color 0.2s, color 0.2s;
    }
    .btn-secondary:hover { border-color: var(--accent); color: var(--accent); }

    .hero-stats {
      position: absolute; right: 60px; bottom: 80px;
      display: flex; gap: 48px;
      opacity: 0;
      animation: fadeUp 0.7s 0.8s forwards;
    }
    .stat-item { text-align: center; }
    .stat-num {
      font-family: 'Syne', sans-serif;
      font-size: 2.2rem;
      font-weight: 800;
      color: var(--accent);
      line-height: 1;
    }
    .stat-label {
      font-family: 'DM Mono', monospace;
      font-size: 0.7rem;
      color: var(--muted);
      letter-spacing: 0.1em;
      margin-top: 4px;
    }

    /* Section titles */
    .section-header {
      display: flex; align-items: center; gap: 20px;
      margin-bottom: 64px;
    }
    .section-num {
      font-family: 'DM Mono', monospace;
      font-size: 0.7rem;
      color: var(--accent);
      letter-spacing: 0.15em;
    }
    .section-line { flex: 1; height: 1px; background: var(--border); max-width: 60px; }
    h2 {
      font-family: 'Syne', sans-serif;
      font-size: clamp(2rem, 4vw, 3rem);
      font-weight: 800;
      letter-spacing: -0.02em;
    }

    /* ABOUT */
    #about {
      padding: 120px 60px;
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 80px;
      align-items: start;
      max-width: 1400px;
      margin: 0 auto;
    }

    .about-text p {
      font-size: 1.05rem;
      line-height: 1.9;
      color: #8090a8;
      margin-bottom: 20px;
    }
    .about-text p strong { color: var(--text); font-weight: 500; }

    .about-aside {}
    .about-card {
      background: var(--card);
      border: 1px solid var(--border);
      padding: 32px;
      margin-bottom: 20px;
    }
    .about-card h4 {
      font-family: 'DM Mono', monospace;
      font-size: 0.72rem;
      letter-spacing: 0.15em;
      color: var(--accent);
      margin-bottom: 16px;
    }
    .tag-list { display: flex; flex-wrap: wrap; gap: 8px; }
    .tag {
      font-family: 'DM Mono', monospace;
      font-size: 0.72rem;
      padding: 5px 12px;
      border: 1px solid var(--border);
      color: var(--muted);
      letter-spacing: 0.05em;
      transition: border-color 0.2s, color 0.2s;
    }
    .tag:hover { border-color: var(--accent); color: var(--accent); }

    /* SKILLS */
    #skills {
      padding: 100px 60px;
      max-width: 1400px;
      margin: 0 auto;
    }

    .skills-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 1px;
      background: var(--border);
      border: 1px solid var(--border);
    }

    .skill-card {
      background: var(--bg2);
      padding: 36px 28px;
      transition: background 0.2s;
      position: relative;
      overflow: hidden;
    }
    .skill-card::before {
      content: '';
      position: absolute; top: 0; left: 0;
      width: 100%; height: 2px;
      background: var(--accent);
      transform: scaleX(0);
      transform-origin: left;
      transition: transform 0.3s;
    }
    .skill-card:hover { background: var(--bg3); }
    .skill-card:hover::before { transform: scaleX(1); }

    .skill-icon {
      font-size: 1.8rem;
      margin-bottom: 16px;
    }
    .skill-card h3 {
      font-family: 'Syne', sans-serif;
      font-size: 1rem;
      font-weight: 700;
      margin-bottom: 10px;
    }
    .skill-card p {
      font-size: 0.82rem;
      color: var(--muted);
      line-height: 1.6;
    }

    .skill-bar-wrap { margin-top: 14px; }
    .skill-bar-label {
      display: flex; justify-content: space-between;
      font-family: 'DM Mono', monospace;
      font-size: 0.68rem;
      color: var(--muted);
      margin-bottom: 6px;
    }
    .skill-bar {
      height: 2px;
      background: var(--border);
      position: relative;
    }
    .skill-bar-fill {
      height: 100%;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      width: 0;
      transition: width 1.2s ease;
    }

    /* PROJECTS */
    #projects {
      padding: 100px 60px;
      max-width: 1400px;
      margin: 0 auto;
    }

    .projects-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 20px;
    }

    .project-card {
      background: var(--card);
      border: 1px solid var(--border);
      padding: 32px;
      position: relative;
      overflow: hidden;
      transition: border-color 0.3s, transform 0.3s;
    }
    .project-card::after {
      content: '';
      position: absolute; inset: 0;
      background: linear-gradient(135deg, rgba(0,229,255,0.04), transparent);
      opacity: 0;
      transition: opacity 0.3s;
    }
    .project-card:hover {
      border-color: rgba(0,229,255,0.3);
      transform: translateY(-4px);
    }
    .project-card:hover::after { opacity: 1; }

    .project-card.featured {
      grid-column: span 2;
    }

    .project-num {
      font-family: 'DM Mono', monospace;
      font-size: 0.68rem;
      color: var(--muted);
      letter-spacing: 0.1em;
      margin-bottom: 20px;
    }
    .project-card h3 {
      font-family: 'Syne', sans-serif;
      font-size: 1.3rem;
      font-weight: 700;
      margin-bottom: 12px;
    }
    .project-card p {
      font-size: 0.88rem;
      color: var(--muted);
      line-height: 1.7;
      margin-bottom: 24px;
    }
    .project-tags { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 28px; }
    .project-tag {
      font-family: 'DM Mono', monospace;
      font-size: 0.68rem;
      padding: 4px 10px;
      background: rgba(0,229,255,0.07);
      color: var(--accent);
      border: 1px solid rgba(0,229,255,0.2);
      letter-spacing: 0.05em;
    }
    .project-links { display: flex; gap: 16px; }
    .project-link {
      font-family: 'DM Mono', monospace;
      font-size: 0.75rem;
      color: var(--muted);
      text-decoration: none;
      letter-spacing: 0.08em;
      transition: color 0.2s;
      display: flex; align-items: center; gap: 6px;
    }
    .project-link:hover { color: var(--accent); }

    /* EXPERIENCE */
    #experience {
      padding: 100px 60px;
      max-width: 1400px;
      margin: 0 auto;
    }

    .timeline { position: relative; padding-left: 40px; }
    .timeline::before {
      content: '';
      position: absolute; left: 0; top: 8px; bottom: 0;
      width: 1px;
      background: linear-gradient(to bottom, var(--accent), transparent);
    }

    .timeline-item {
      position: relative;
      margin-bottom: 56px;
      opacity: 0;
      transform: translateX(-20px);
      transition: opacity 0.6s, transform 0.6s;
    }
    .timeline-item.visible { opacity: 1; transform: translateX(0); }

    .timeline-item::before {
      content: '';
      position: absolute; left: -44px; top: 8px;
      width: 8px; height: 8px;
      background: var(--accent);
      border-radius: 50%;
      box-shadow: 0 0 12px rgba(0,229,255,0.5);
    }

    .timeline-date {
      font-family: 'DM Mono', monospace;
      font-size: 0.72rem;
      color: var(--accent);
      letter-spacing: 0.1em;
      margin-bottom: 8px;
    }
    .timeline-item h3 {
      font-family: 'Syne', sans-serif;
      font-size: 1.1rem;
      font-weight: 700;
      margin-bottom: 4px;
    }
    .timeline-company {
      font-size: 0.85rem;
      color: var(--muted);
      margin-bottom: 12px;
    }
    .timeline-item p {
      font-size: 0.88rem;
      color: #5a6a7a;
      line-height: 1.7;
    }

    /* CONTACT */
    #contact {
      padding: 100px 60px;
      max-width: 1400px;
      margin: 0 auto;
    }

    .contact-inner {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 80px;
    }

    .contact-info h3 {
      font-family: 'Syne', sans-serif;
      font-size: 1.8rem;
      font-weight: 800;
      line-height: 1.2;
      margin-bottom: 20px;
    }
    .contact-info p {
      font-size: 0.9rem;
      color: var(--muted);
      line-height: 1.8;
      margin-bottom: 36px;
    }
    .contact-links { display: flex; flex-direction: column; gap: 14px; }
    .contact-link {
      display: flex; align-items: center; gap: 14px;
      font-family: 'DM Mono', monospace;
      font-size: 0.82rem;
      color: var(--muted);
      text-decoration: none;
      transition: color 0.2s;
    }
    .contact-link:hover { color: var(--accent); }
    .contact-link-icon {
      width: 36px; height: 36px;
      border: 1px solid var(--border);
      display: flex; align-items: center; justify-content: center;
      font-size: 0.9rem;
      transition: border-color 0.2s;
    }
    .contact-link:hover .contact-link-icon { border-color: var(--accent); }

    .contact-form { display: flex; flex-direction: column; gap: 16px; }

    .form-group { display: flex; flex-direction: column; gap: 8px; }
    .form-group label {
      font-family: 'DM Mono', monospace;
      font-size: 0.7rem;
      color: var(--muted);
      letter-spacing: 0.1em;
    }
    .form-group input,
    .form-group textarea {
      background: var(--card);
      border: 1px solid var(--border);
      color: var(--text);
      padding: 12px 16px;
      font-family: 'Inter', sans-serif;
      font-size: 0.88rem;
      outline: none;
      transition: border-color 0.2s;
      resize: none;
    }
    .form-group input:focus,
    .form-group textarea:focus { border-color: var(--accent); }

    .form-group textarea { height: 120px; }

    .btn-submit {
      font-family: 'DM Mono', monospace;
      font-size: 0.8rem;
      letter-spacing: 0.1em;
      padding: 14px;
      background: var(--accent);
      color: var(--bg);
      border: none;
      cursor: none;
      transition: opacity 0.2s, transform 0.2s;
    }
    .btn-submit:hover { opacity: 0.85; transform: translateY(-1px); }

    /* FOOTER */
    footer {
      border-top: 1px solid var(--border);
      padding: 32px 60px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      position: relative;
      z-index: 2;
    }
    footer p {
      font-family: 'DM Mono', monospace;
      font-size: 0.72rem;
      color: var(--muted);
      letter-spacing: 0.05em;
    }
    .footer-socials { display: flex; gap: 16px; }
    .footer-social {
      font-family: 'DM Mono', monospace;
      font-size: 0.72rem;
      color: var(--muted);
      text-decoration: none;
      transition: color 0.2s;
    }
    .footer-social:hover { color: var(--accent); }

    /* Scroll reveal */
    .reveal {
      opacity: 0;
      transform: translateY(30px);
      transition: opacity 0.7s, transform 0.7s;
    }
    .reveal.visible {
      opacity: 1;
      transform: translateY(0);
    }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(24px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    /* Mobile */
    @media (max-width: 900px) {
      nav { padding: 18px 24px; }
      .nav-links { display: none; }
      #hero { padding: 120px 24px 80px; }
      .hero-stats { right: 24px; bottom: 40px; gap: 24px; }
      #about { grid-template-columns: 1fr; padding: 80px 24px; gap: 40px; }
      #skills { padding: 80px 24px; }
      .skills-grid { grid-template-columns: repeat(2, 1fr); }
      #projects { padding: 80px 24px; }
      .projects-grid { grid-template-columns: 1fr; }
      .project-card.featured { grid-column: span 1; }
      #experience { padding: 80px 24px; }
      #contact { padding: 80px 24px; }
      .contact-inner { grid-template-columns: 1fr; gap: 48px; }
      footer { padding: 24px; flex-direction: column; gap: 16px; }
    }
  </style>
</head>
<body>

  <div class="cursor" id="cursor"></div>
  <div class="cursor-ring" id="cursorRing"></div>

  <!-- NAV -->
  <nav>
    <a href="#hero" class="nav-logo">dev<span>.</span>portfolio</a>
    <ul class="nav-links">
      <li><a href="#about">about</a></li>
      <li><a href="#skills">skills</a></li>
      <li><a href="#projects">projects</a></li>
      <li><a href="#experience">experience</a></li>
      <li><a href="#contact">contact</a></li>
    </ul>
    <a href="#contact" class="nav-cta">hire me</a>
  </nav>

  <!-- HERO -->
  <section id="hero">
    <div class="hero-grid-bg"></div>
    <div class="hero-glow"></div>
    <div class="hero-glow2"></div>

    <div class="hero-content">
      <p class="hero-tag">// full stack developer &amp; ai integrator</p>
      <h1>
        Building<br>
        <span class="line2">scalable</span>&nbsp;<span class="accent-word">web</span><br>
        <span class="line2">applications</span>
      </h1>
      <p class="hero-desc">
        Specializing in React.js, Next.js, Laravel, and Node.js — I craft complete solutions from database architecture to modern frontend interfaces, infused with AI-powered features.
      </p>
      <div class="hero-actions">
        <a href="#projects" class="btn-primary">View Projects</a>
        <a href="#contact" class="btn-secondary">Get In Touch</a>
      </div>
    </div>

    <div class="hero-stats">
      <div class="stat-item">
        <div class="stat-num" data-target="50">0</div>
        <div class="stat-label">projects</div>
      </div>
      <div class="stat-item">
        <div class="stat-num" data-target="5">0</div>
        <div class="stat-label">years exp</div>
      </div>
      <div class="stat-item">
        <div class="stat-num" data-target="30">0</div>
        <div class="stat-label">clients</div>
      </div>
    </div>
  </section>

  <!-- ABOUT -->
  <section id="about">
    <div>
      <div class="section-header reveal">
        <span class="section-num">01 /</span>
        <div class="section-line"></div>
        <h2>About Me</h2>
      </div>
      <div class="about-text reveal">
        <p>I am a <strong>Full Stack Developer</strong> specializing in building scalable, secure, and AI-powered web applications. Experienced in <strong>React.js, Next.js, Laravel, and Node.js</strong>, I develop complete solutions from database architecture to modern frontend interfaces.</p>
        <p>I also integrate <strong>AI APIs</strong> to create intelligent features such as chatbots, automation tools, and data-driven systems. My focus is clean architecture, performance optimization, and secure backend implementation.</p>
        <p>I deliver reliable, <strong>production-ready applications</strong> that help businesses grow through smart technology solutions.</p>
      </div>
    </div>

    <div class="about-aside">
      <div class="about-card reveal">
        <h4>// frontend stack</h4>
        <div class="tag-list">
          <span class="tag">React.js</span>
          <span class="tag">Next.js</span>
          <span class="tag">TypeScript</span>
          <span class="tag">Tailwind CSS</span>
          <span class="tag">Vue.js</span>
        </div>
      </div>
      <div class="about-card reveal">
        <h4>// backend stack</h4>
        <div class="tag-list">
          <span class="tag">Laravel</span>
          <span class="tag">Node.js</span>
          <span class="tag">Express</span>
          <span class="tag">REST API</span>
          <span class="tag">GraphQL</span>
        </div>
      </div>
      <div class="about-card reveal">
        <h4>// ai &amp; database</h4>
        <div class="tag-list">
          <span class="tag">OpenAI API</span>
          <span class="tag">LangChain</span>
          <span class="tag">MySQL</span>
          <span class="tag">PostgreSQL</span>
          <span class="tag">MongoDB</span>
          <span class="tag">Redis</span>
        </div>
      </div>
    </div>
  </section>

  <!-- SKILLS -->
  <section id="skills">
    <div class="section-header reveal">
      <span class="section-num">02 /</span>
      <div class="section-line"></div>
      <h2>Skills</h2>
    </div>

    <div class="skills-grid reveal">
      <div class="skill-card">
        <div class="skill-icon">⚛️</div>
        <h3>Frontend</h3>
        <p>Modern React & Next.js with TypeScript for performant UIs</p>
        <div class="skill-bar-wrap">
          <div class="skill-bar-label"><span>React.js</span><span>90%</span></div>
          <div class="skill-bar"><div class="skill-bar-fill" data-width="90"></div></div>
        </div>
        <div class="skill-bar-wrap">
          <div class="skill-bar-label"><span>Next.js</span><span>85%</span></div>
          <div class="skill-bar"><div class="skill-bar-fill" data-width="85"></div></div>
        </div>
      </div>
      <div class="skill-card">
        <div class="skill-icon">🛠️</div>
        <h3>Backend</h3>
        <p>Laravel & Node.js APIs with clean architecture patterns</p>
        <div class="skill-bar-wrap">
          <div class="skill-bar-label"><span>Laravel</span><span>92%</span></div>
          <div class="skill-bar"><div class="skill-bar-fill" data-width="92"></div></div>
        </div>
        <div class="skill-bar-wrap">
          <div class="skill-bar-label"><span>Node.js</span><span>88%</span></div>
          <div class="skill-bar"><div class="skill-bar-fill" data-width="88"></div></div>
        </div>
      </div>
      <div class="skill-card">
        <div class="skill-icon">🤖</div>
        <h3>AI Integration</h3>
        <p>Building intelligent features with OpenAI, Gemini, and custom models</p>
        <div class="skill-bar-wrap">
          <div class="skill-bar-label"><span>OpenAI API</span><span>85%</span></div>
          <div class="skill-bar"><div class="skill-bar-fill" data-width="85"></div></div>
        </div>
        <div class="skill-bar-wrap">
          <div class="skill-bar-label"><span>LangChain</span><span>75%</span></div>
          <div class="skill-bar"><div class="skill-bar-fill" data-width="75"></div></div>
        </div>
      </div>
      <div class="skill-card">
        <div class="skill-icon">🗄️</div>
        <h3>Database</h3>
        <p>Relational and NoSQL database architecture & optimization</p>
        <div class="skill-bar-wrap">
          <div class="skill-bar-label"><span>MySQL/PgSQL</span><span>90%</span></div>
          <div class="skill-bar"><div class="skill-bar-fill" data-width="90"></div></div>
        </div>
        <div class="skill-bar-wrap">
          <div class="skill-bar-label"><span>MongoDB</span><span>80%</span></div>
          <div class="skill-bar"><div class="skill-bar-fill" data-width="80"></div></div>
        </div>
      </div>
    </div>
  </section>

  <!-- PROJECTS -->
  <section id="projects">
    <div class="section-header reveal">
      <span class="section-num">03 /</span>
      <div class="section-line"></div>
      <h2>Projects</h2>
    </div>

    <div class="projects-grid">
      <div class="project-card featured reveal">
        <div class="project-num">001 — featured</div>
        <h3>AI-Powered SaaS Platform</h3>
        <p>A full-featured SaaS application with AI chatbot, user management, subscription billing, and analytics dashboard. Built with Next.js frontend and Laravel backend, integrated with OpenAI GPT-4 for intelligent automation.</p>
        <div class="project-tags">
          <span class="project-tag">Next.js</span>
          <span class="project-tag">Laravel</span>
          <span class="project-tag">OpenAI</span>
          <span class="project-tag">PostgreSQL</span>
          <span class="project-tag">Stripe</span>
        </div>
        <div class="project-links">
          <a href="#" class="project-link">↗ Live Demo</a>
          <a href="#" class="project-link">⌥ GitHub</a>
        </div>
      </div>

      <div class="project-card reveal">
        <div class="project-num">002</div>
        <h3>Real-time Chat App</h3>
        <p>Scalable real-time messaging platform with WebSocket, end-to-end encryption, and AI-powered message summarization.</p>
        <div class="project-tags">
          <span class="project-tag">Node.js</span>
          <span class="project-tag">Socket.io</span>
          <span class="project-tag">React</span>
          <span class="project-tag">Redis</span>
        </div>
        <div class="project-links">
          <a href="#" class="project-link">↗ Live Demo</a>
          <a href="#" class="project-link">⌥ GitHub</a>
        </div>
      </div>

      <div class="project-card reveal">
        <div class="project-num">003</div>
        <h3>E-Commerce Engine</h3>
        <p>Headless e-commerce solution with multi-vendor support, dynamic pricing engine, and AI product recommendations.</p>
        <div class="project-tags">
          <span class="project-tag">Laravel</span>
          <span class="project-tag">Vue.js</span>
          <span class="project-tag">MySQL</span>
          <span class="project-tag">AI API</span>
        </div>
        <div class="project-links">
          <a href="#" class="project-link">↗ Live Demo</a>
          <a href="#" class="project-link">⌥ GitHub</a>
        </div>
      </div>

      <div class="project-card reveal">
        <div class="project-num">004</div>
        <h3>Data Analytics Dashboard</h3>
        <p>Business intelligence platform with real-time data visualization, automated reports, and predictive analytics.</p>
        <div class="project-tags">
          <span class="project-tag">Next.js</span>
          <span class="project-tag">Node.js</span>
          <span class="project-tag">MongoDB</span>
          <span class="project-tag">Chart.js</span>
        </div>
        <div class="project-links">
          <a href="#" class="project-link">↗ Live Demo</a>
          <a href="#" class="project-link">⌥ GitHub</a>
        </div>
      </div>

      <div class="project-card reveal">
        <div class="project-num">005</div>
        <h3>Automation Workflow Tool</h3>
        <p>No-code automation builder with drag-and-drop interface, AI task generation, and third-party API integrations.</p>
        <div class="project-tags">
          <span class="project-tag">React</span>
          <span class="project-tag">Express</span>
          <span class="project-tag">LangChain</span>
          <span class="project-tag">Docker</span>
        </div>
        <div class="project-links">
          <a href="#" class="project-link">↗ Live Demo</a>
          <a href="#" class="project-link">⌥ GitHub</a>
        </div>
      </div>
    </div>
  </section>

  <!-- EXPERIENCE -->
  <section id="experience">
    <div class="section-header reveal">
      <span class="section-num">04 /</span>
      <div class="section-line"></div>
      <h2>Experience</h2>
    </div>

    <div class="timeline">
      <div class="timeline-item">
        <div class="timeline-date">2022 — Present</div>
        <h3>Senior Full Stack Developer</h3>
        <div class="timeline-company">Tech Startup — Remote</div>
        <p>Lead development of AI-integrated web applications. Architected microservices backend, reduced API response time by 60%, and integrated LLM-based automation features serving 10k+ users.</p>
      </div>
      <div class="timeline-item">
        <div class="timeline-date">2020 — 2022</div>
        <h3>Full Stack Developer</h3>
        <div class="timeline-company">Digital Agency — Jakarta, ID</div>
        <p>Built and maintained 15+ client web applications using Laravel and React.js. Managed database design, server infrastructure, and CI/CD pipelines for production deployments.</p>
      </div>
      <div class="timeline-item">
        <div class="timeline-date">2019 — 2020</div>
        <h3>Backend Developer</h3>
        <div class="timeline-company">Software House — Freelance</div>
        <p>Developed RESTful APIs for mobile and web clients using Node.js and Laravel. Focused on security, authentication systems, and performance optimization.</p>
      </div>
    </div>
  </section>

  <!-- CONTACT -->
  <section id="contact">
    <div class="section-header reveal">
      <span class="section-num">05 /</span>
      <div class="section-line"></div>
      <h2>Contact</h2>
    </div>

    <div class="contact-inner">
      <div class="contact-info reveal">
        <h3>Let's build something great together.</h3>
        <p>Open to freelance projects, full-time positions, and exciting collaborations. If you have an idea, I'd love to hear it.</p>
        <div class="contact-links">
          <a href="mailto:hello@yourmail.com" class="contact-link">
            <div class="contact-link-icon">✉</div>
            hello@yourmail.com
          </a>
          <a href="https://linkedin.com" class="contact-link" target="_blank">
            <div class="contact-link-icon">in</div>
            linkedin.com/in/yourprofile
          </a>
          <a href="https://github.com" class="contact-link" target="_blank">
            <div class="contact-link-icon">⌥</div>
            github.com/yourprofile
          </a>
          <a href="#" class="contact-link">
            <div class="contact-link-icon">↓</div>
            Download CV
          </a>
        </div>
      </div>

      <div class="reveal">
        <form class="contact-form" onsubmit="handleSubmit(event)">
          <div class="form-group">
            <label>// YOUR NAME</label>
            <input type="text" placeholder="John Doe" required />
          </div>
          <div class="form-group">
            <label>// EMAIL ADDRESS</label>
            <input type="email" placeholder="john@example.com" required />
          </div>
          <div class="form-group">
            <label>// MESSAGE</label>
            <textarea placeholder="Tell me about your project..."></textarea>
          </div>
          <button type="submit" class="btn-submit">Send Message →</button>
        </form>
      </div>
    </div>
  </section>

  <!-- FOOTER -->
  <footer>
    <p>© 2025 — Full Stack Developer. Built with precision.</p>
    <div class="footer-socials">
      <a href="#" class="footer-social">GitHub</a>
      <a href="#" class="footer-social">LinkedIn</a>
      <a href="#" class="footer-social">Twitter</a>
    </div>
  </footer>

  <script>
    // Custom cursor
    const cursor = document.getElementById('cursor');
    const ring = document.getElementById('cursorRing');
    let mx = 0, my = 0, rx = 0, ry = 0;

    document.addEventListener('mousemove', e => {
      mx = e.clientX; my = e.clientY;
      cursor.style.transform = `translate(${mx - 5}px, ${my - 5}px)`;
    });

    function animRing() {
      rx += (mx - rx) * 0.12;
      ry += (my - ry) * 0.12;
      ring.style.transform = `translate(${rx - 18}px, ${ry - 18}px)`;
      requestAnimationFrame(animRing);
    }
    animRing();

    document.querySelectorAll('a, button').forEach(el => {
      el.addEventListener('mouseenter', () => {
        cursor.style.transform += ' scale(2)';
        ring.style.width = '56px';
        ring.style.height = '56px';
      });
      el.addEventListener('mouseleave', () => {
        ring.style.width = '36px';
        ring.style.height = '36px';
      });
    });

    // Scroll reveal
    const reveals = document.querySelectorAll('.reveal');
    const timelineItems = document.querySelectorAll('.timeline-item');

    const revealObserver = new IntersectionObserver((entries) => {
      entries.forEach((entry, i) => {
        if (entry.isIntersecting) {
          setTimeout(() => entry.target.classList.add('visible'), i * 80);
          revealObserver.unobserve(entry.target);
        }
      });
    }, { threshold: 0.1 });

    reveals.forEach(el => revealObserver.observe(el));
    timelineItems.forEach(el => revealObserver.observe(el));

    // Skill bars
    const barObserver = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.querySelectorAll('.skill-bar-fill').forEach(bar => {
            bar.style.width = bar.dataset.width + '%';
          });
          barObserver.unobserve(entry.target);
        }
      });
    }, { threshold: 0.3 });

    document.querySelectorAll('.skills-grid').forEach(el => barObserver.observe(el));

    // Counter animation
    function animateCount(el, target, suffix = '+') {
      let start = 0;
      const duration = 1800;
      const step = timestamp => {
        if (!start) start = timestamp;
        const progress = Math.min((timestamp - start) / duration, 1);
        const eased = 1 - Math.pow(1 - progress, 3);
        el.textContent = Math.floor(eased * target) + suffix;
        if (progress < 1) requestAnimationFrame(step);
      };
      requestAnimationFrame(step);
    }

    const statsObserver = new IntersectionObserver(entries => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          document.querySelectorAll('[data-target]').forEach(el => {
            animateCount(el, parseInt(el.dataset.target));
          });
          statsObserver.disconnect();
        }
      });
    }, { threshold: 0.5 });

    const heroStats = document.querySelector('.hero-stats');
    if (heroStats) statsObserver.observe(heroStats);

    // Active nav link
    const sections = document.querySelectorAll('section[id]');
    window.addEventListener('scroll', () => {
      let current = '';
      sections.forEach(s => {
        if (window.scrollY >= s.offsetTop - 120) current = s.id;
      });
      document.querySelectorAll('.nav-links a').forEach(a => {
        a.style.color = a.getAttribute('href') === '#' + current ? 'var(--accent)' : '';
      });
    });

    // Form submit
    function handleSubmit(e) {
      e.preventDefault();
      const btn = e.target.querySelector('.btn-submit');
      btn.textContent = 'Message Sent ✓';
      btn.style.background = '#00c48c';
      setTimeout(() => {
        btn.textContent = 'Send Message →';
        btn.style.background = '';
        e.target.reset();
      }, 3000);
    }
  </script>
</body>
</html>
