<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Three Guitars</title>
  <link rel="preconnect" href="https://fonts.googleapis.com"/>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;0,900;1,400;1,700&family=Crimson+Pro:ital,wght@0,300;0,400;1,300&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --cream:   #f5efe3;
      --parchment: #ede4d0;
      --warm:    #c8a96e;
      --amber:   #b8832a;
      --mahogany:#5c2e0e;
      --ink:     #1e1208;
      --rust:    #8b3a1a;
    }

    html { scroll-behavior: smooth; }

    body {
      font-family: 'Crimson Pro', Georgia, serif;
      background: var(--cream);
      color: var(--ink);
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* Grain texture overlay */
    body::before {
      content: '';
      position: fixed; inset: 0; z-index: 0;
      pointer-events: none;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
      opacity: 0.35;
    }

    /* ======== HERO ======== */
    .hero {
      position: relative; z-index: 1;
      min-height: 100vh;
      display: flex; flex-direction: column;
      align-items: center; justify-content: center;
      padding: 80px 24px 60px;
      text-align: center;
      border-bottom: 2px solid var(--warm);
    }

    .hero::after {
      content: '';
      position: absolute; bottom: -1px; left: 0; right: 0;
      height: 3px;
      background: repeating-linear-gradient(90deg, var(--amber) 0, var(--amber) 8px, transparent 8px, transparent 16px);
    }

    .issue-tag {
      font-family: 'Crimson Pro', serif;
      font-size: 11px;
      letter-spacing: 4px;
      text-transform: uppercase;
      color: var(--rust);
      margin-bottom: 24px;
      opacity: 0;
      animation: fadeUp 0.6s ease forwards 0.1s;
    }

    .hero-title {
      font-family: 'Playfair Display', Georgia, serif;
      font-size: clamp(52px, 10vw, 100px);
      font-weight: 900;
      line-height: 0.92;
      color: var(--ink);
      letter-spacing: -2px;
      opacity: 0;
      animation: fadeUp 0.8s ease forwards 0.2s;
    }

    .hero-title em {
      font-style: italic;
      color: var(--mahogany);
    }

    .hero-sub {
      font-size: 18px;
      font-weight: 300;
      font-style: italic;
      color: var(--amber);
      margin-top: 24px;
      max-width: 480px;
      line-height: 1.6;
      opacity: 0;
      animation: fadeUp 0.8s ease forwards 0.4s;
    }

    .hero-divider {
      display: flex; align-items: center; gap: 16px;
      margin: 36px 0 16px;
      opacity: 0;
      animation: fadeUp 0.6s ease forwards 0.5s;
    }

    .hero-divider::before,
    .hero-divider::after {
      content: '';
      flex: 1; max-width: 100px;
      height: 1px; background: var(--warm);
    }

    .hero-divider svg {
      width: 20px; height: 20px;
      fill: var(--amber); flex-shrink: 0;
    }

    /* Guitar SVG art */
    .guitar-hero-art {
      position: relative;
      width: 100%; max-width: 600px;
      margin: 40px auto 0;
      opacity: 0;
      animation: fadeUp 1s ease forwards 0.6s;
    }

    /* ======== SECTION LABEL ======== */
    .section-label {
      display: block;
      font-size: 10px;
      letter-spacing: 5px;
      text-transform: uppercase;
      color: var(--rust);
      margin-bottom: 12px;
    }

    /* ======== GUITARS SECTION ======== */
    .guitars-section {
      position: relative; z-index: 1;
      padding: 100px 24px;
      max-width: 1100px;
      margin: 0 auto;
    }

    .section-heading {
      font-family: 'Playfair Display', serif;
      font-size: 38px; font-weight: 700;
      color: var(--ink);
      margin-bottom: 64px;
      text-align: center;
    }

    .section-heading span {
      display: block;
      font-style: italic;
      font-weight: 400;
      color: var(--amber);
      font-size: 22px;
    }

    /* Guitar Cards */
    .guitar-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(290px, 1fr));
      gap: 0;
    }

    .guitar-card {
      border: 1px solid var(--parchment);
      padding: 40px 36px 44px;
      position: relative;
      transition: background 0.3s;
      cursor: default;
    }

    .guitar-card:not(:last-child) {
      border-right: none;
    }

    @media (max-width: 700px) {
      .guitar-card:not(:last-child) {
        border-right: 1px solid var(--parchment);
        border-bottom: none;
      }
    }

    .guitar-card::before {
      content: attr(data-num);
      position: absolute;
      top: 24px; right: 28px;
      font-family: 'Playfair Display', serif;
      font-size: 72px; font-weight: 900;
      color: var(--parchment);
      line-height: 1;
      pointer-events: none;
      transition: color 0.3s;
    }

    .guitar-card:hover::before { color: var(--warm); }
    .guitar-card:hover { background: var(--parchment); }

    .guitar-illustration {
      width: 80px; height: 80px;
      margin-bottom: 24px;
    }

    .guitar-type {
      font-size: 10px; letter-spacing: 4px;
      text-transform: uppercase;
      color: var(--rust);
      margin-bottom: 8px;
      display: block;
    }

    .guitar-name {
      font-family: 'Playfair Display', serif;
      font-size: 26px; font-weight: 700;
      color: var(--ink);
      line-height: 1.15;
      margin-bottom: 16px;
    }

    .guitar-desc {
      font-size: 16px;
      font-weight: 300;
      line-height: 1.8;
      color: #4a3520;
      margin-bottom: 24px;
    }

    .guitar-stats {
      display: flex; flex-direction: column; gap: 8px;
      padding-top: 20px;
      border-top: 1px solid var(--parchment);
    }

    .stat-row {
      display: flex; justify-content: space-between;
      align-items: center;
      font-size: 13px;
    }

    .stat-key {
      color: var(--amber); letter-spacing: 1px;
      text-transform: uppercase; font-size: 11px;
    }

    .stat-val {
      font-family: 'Playfair Display', serif;
      font-weight: 700; font-style: italic;
      color: var(--ink);
    }

    /* ======== QUOTE BAND ======== */
    .quote-band {
      position: relative; z-index: 1;
      background: var(--mahogany);
      padding: 80px 24px;
      text-align: center;
      overflow: hidden;
    }

    .quote-band::before {
      content: '';
      position: absolute; inset: 0;
      background: repeating-linear-gradient(
        45deg,
        transparent,
        transparent 30px,
        rgba(255,255,255,0.015) 30px,
        rgba(255,255,255,0.015) 60px
      );
    }

    .quote-text {
      font-family: 'Playfair Display', serif;
      font-size: clamp(20px, 4vw, 32px);
      font-style: italic;
      font-weight: 400;
      color: var(--cream);
      max-width: 720px;
      margin: 0 auto 20px;
      line-height: 1.55;
      position: relative; z-index: 1;
    }

    .quote-attr {
      font-size: 13px; letter-spacing: 3px;
      text-transform: uppercase;
      color: var(--warm);
      position: relative; z-index: 1;
    }

    /* ======== FOOTER ======== */
    footer {
      position: relative; z-index: 1;
      border-top: 2px solid var(--warm);
      padding: 40px 24px;
      text-align: center;
    }

    footer::before {
      content: '';
      display: block;
      height: 3px;
      background: repeating-linear-gradient(90deg, var(--amber) 0, var(--amber) 8px, transparent 8px, transparent 16px);
      margin-bottom: 40px;
    }

    .footer-title {
      font-family: 'Playfair Display', serif;
      font-size: 20px; font-style: italic;
      color: var(--amber);
      margin-bottom: 8px;
    }

    .footer-copy {
      font-size: 13px; color: var(--warm);
      letter-spacing: 1px;
    }

    /* ======== ANIMATIONS ======== */
    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(24px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    .reveal {
      opacity: 0;
      transform: translateY(30px);
      transition: opacity 0.7s ease, transform 0.7s ease;
    }

    .reveal.visible {
      opacity: 1;
      transform: translateY(0);
    }
  </style>
</head>
<body>

  <!-- HERO -->
  <header class="hero">
    <span class="issue-tag">A Study in Six Strings</span>
    <h1 class="hero-title">Three<br><em>Guitars</em></h1>
    <p class="hero-sub">From the warmth of the acoustic to the roar of the electric — every guitar tells a different story.</p>
    <div class="hero-divider">
      <svg viewBox="0 0 24 24"><path d="M12 2a5 5 0 1 1 0 10A5 5 0 0 1 12 2zm0 12c-3.5 0-6.5 1.5-8 3.5V19a1 1 0 0 0 1 1h14a1 1 0 0 0 1-1v-1.5C18.5 15.5 15.5 14 12 14z"/></svg>
    </div>

    <!-- Hero guitar SVG illustration -->
    <div class="guitar-hero-art">
      <svg viewBox="0 0 600 180" xmlns="http://www.w3.org/2000/svg" style="width:100%;height:auto;">
        <!-- Acoustic -->
        <g transform="translate(60,20)">
          <ellipse cx="60" cy="90" rx="45" ry="55" fill="#c8a96e" stroke="#5c2e0e" stroke-width="2"/>
          <ellipse cx="60" cy="75" rx="32" ry="38" fill="#b8832a" stroke="#5c2e0e" stroke-width="1.5"/>
          <ellipse cx="60" cy="105" rx="38" ry="42" fill="#b8832a" stroke="#5c2e0e" stroke-width="1.5"/>
          <rect x="56" y="10" width="8" height="80" rx="4" fill="#5c2e0e"/>
          <circle cx="60" cy="90" r="14" fill="none" stroke="#5c2e0e" stroke-width="1.5"/>
          <circle cx="60" cy="90" r="9" fill="none" stroke="#8b3a1a" stroke-width="1"/>
          <text x="60" y="160" text-anchor="middle" font-family="Playfair Display,serif" font-size="11" fill="#5c2e0e" font-style="italic">Acoustic</text>
        </g>

        <!-- Electric -->
        <g transform="translate(235,15)">
          <path d="M60 10 C30 10 15 30 15 55 L15 100 C15 130 30 150 60 155 C90 150 105 130 105 100 L105 55 C105 30 90 10 60 10Z" fill="#1e1208" stroke="#c8a96e" stroke-width="2"/>
          <path d="M25 55 C15 55 10 65 10 75 L10 90 C10 100 15 108 25 108 Z" fill="#c8a96e" stroke="#8b3a1a" stroke-width="1.5"/>
          <path d="M95 55 C105 55 110 65 110 75 L110 90 C110 100 105 108 95 108 Z" fill="#c8a96e" stroke="#8b3a1a" stroke-width="1.5"/>
          <rect x="54" y="5" width="12" height="65" rx="3" fill="#2d1a0a"/>
          <rect x="30" y="80" width="60" height="18" rx="4" fill="#b8832a" opacity="0.6"/>
          <rect x="30" y="105" width="60" height="14" rx="4" fill="#b8832a" opacity="0.6"/>
          <line x1="60" y1="10" x2="60" y2="155" stroke="#c8a96e" stroke-width="0.5" opacity="0.4"/>
          <text x="60" y="168" text-anchor="middle" font-family="Playfair Display,serif" font-size="11" fill="#5c2e0e" font-style="italic">Electric</text>
        </g>

        <!-- Classical -->
        <g transform="translate(420,18)">
          <ellipse cx="60" cy="88" rx="48" ry="60" fill="#deb887" stroke="#5c2e0e" stroke-width="2"/>
          <ellipse cx="60" cy="70" rx="34" ry="40" fill="#c8a96e" stroke="#5c2e0e" stroke-width="1.5"/>
          <ellipse cx="60" cy="108" rx="40" ry="44" fill="#c8a96e" stroke="#5c2e0e" stroke-width="1.5"/>
          <rect x="56" y="8" width="8" height="82" rx="4" fill="#5c2e0e"/>
          <circle cx="60" cy="88" r="16" fill="none" stroke="#5c2e0e" stroke-width="1.5"/>
          <circle cx="60" cy="88" r="10" fill="none" stroke="#8b3a1a" stroke-width="1"/>
          <circle cx="60" cy="88" r="5" fill="none" stroke="#8b3a1a" stroke-width="0.5"/>
          <text x="60" y="162" text-anchor="middle" font-family="Playfair Display,serif" font-size="11" fill="#5c2e0e" font-style="italic">Classical</text>
        </g>
      </svg>
    </div>
  </header>

  <!-- GUITARS SECTION -->
  <section class="guitars-section">
    <h2 class="section-heading reveal">
      The Three Voices
      <span>Each guitar carries its own soul</span>
    </h2>

    <div class="guitar-grid">

      <!-- Card 1: Acoustic -->
      <article class="guitar-card reveal" data-num="01">
        <!-- Acoustic SVG icon -->
        <svg class="guitar-illustration" viewBox="0 0 80 80" xmlns="http://www.w3.org/2000/svg">
          <ellipse cx="40" cy="48" rx="26" ry="28" fill="#c8a96e"/>
          <ellipse cx="40" cy="38" rx="19" ry="20" fill="#b8832a"/>
          <ellipse cx="40" cy="58" rx="22" ry="22" fill="#b8832a"/>
          <rect x="37" y="8" width="6" height="40" rx="3" fill="#5c2e0e"/>
          <circle cx="40" cy="48" r="9" fill="none" stroke="#5c2e0e" stroke-width="1.5"/>
          <circle cx="40" cy="48" r="5" fill="none" stroke="#8b3a1a" stroke-width="1"/>
        </svg>
        <span class="guitar-type">Steel String</span>
        <h3 class="guitar-name">The Acoustic Guitar</h3>
        <p class="guitar-desc">The acoustic guitar speaks with honesty. No amplifier needed — its hollow wooden body resonates freely, projecting a warm, full-bodied tone that carries folk ballads, campfire songs, and singer-songwriter confessions alike.</p>
        <div class="guitar-stats">
          <div class="stat-row">
            <span class="stat-key">Tone</span>
            <span class="stat-val">Warm & Natural</span>
          </div>
          <div class="stat-row">
            <span class="stat-key">Best for</span>
            <span class="stat-val">Folk, Country, Pop</span>
          </div>
          <div class="stat-row">
            <span class="stat-key">Body</span>
            <span class="stat-val">Hollow Spruce Top</span>
          </div>
          <div class="stat-row">
            <span class="stat-key">Strings</span>
            <span class="stat-val">Steel, 6-string</span>
          </div>
        </div>
      </article>

      <!-- Card 2: Electric -->
      <article class="guitar-card reveal" style="transition-delay:0.15s;" data-num="02">
        <svg class="guitar-illustration" viewBox="0 0 80 80" xmlns="http://www.w3.org/2000/svg">
          <path d="M40 8 C25 8 16 18 16 30 L16 52 C16 65 25 72 40 74 C55 72 64 65 64 52 L64 30 C64 18 55 8 40 8Z" fill="#1e1208"/>
          <path d="M16 30 C10 30 7 36 7 42 L7 50 C7 56 10 60 16 60Z" fill="#c8a96e"/>
          <path d="M64 30 C70 30 73 36 73 42 L73 50 C73 56 70 60 64 60Z" fill="#c8a96e"/>
          <rect x="36" y="4" width="8" height="38" rx="3" fill="#2d1a0a"/>
          <rect x="22" y="42" width="36" height="10" rx="3" fill="#b8832a" opacity="0.7"/>
          <rect x="22" y="56" width="36" height="8" rx="3" fill="#b8832a" opacity="0.7"/>
        </svg>
        <span class="guitar-type">Solid Body</span>
        <h3 class="guitar-name">The Electric Guitar</h3>
        <p class="guitar-desc">Born from the need to be heard, the electric guitar became the voice of a generation. Its magnetic pickups transform string vibration into electrical signal, unlocking a universe of tone — from clean shimmer to thunderous distortion.</p>
        <div class="guitar-stats">
          <div class="stat-row">
            <span class="stat-key">Tone</span>
            <span class="stat-val">Bright & Versatile</span>
          </div>
          <div class="stat-row">
            <span class="stat-key">Best for</span>
            <span class="stat-val">Rock, Blues, Jazz</span>
          </div>
          <div class="stat-row">
            <span class="stat-key">Body</span>
            <span class="stat-val">Solid Mahogany</span>
          </div>
          <div class="stat-row">
            <span class="stat-key">Strings</span>
            <span class="stat-val">Nickel, 6-string</span>
          </div>
        </div>
      </article>

      <!-- Card 3: Classical -->
      <article class="guitar-card reveal" style="transition-delay:0.3s;" data-num="03">
        <svg class="guitar-illustration" viewBox="0 0 80 80" xmlns="http://www.w3.org/2000/svg">
          <ellipse cx="40" cy="48" rx="28" ry="30" fill="#deb887"/>
          <ellipse cx="40" cy="36" rx="20" ry="22" fill="#c8a96e"/>
          <ellipse cx="40" cy="60" rx="24" ry="24" fill="#c8a96e"/>
          <rect x="37" y="6" width="6" height="44" rx="3" fill="#5c2e0e"/>
          <circle cx="40" cy="48" r="10" fill="none" stroke="#5c2e0e" stroke-width="1.5"/>
          <circle cx="40" cy="48" r="6" fill="none" stroke="#8b3a1a" stroke-width="1"/>
          <circle cx="40" cy="48" r="3" fill="none" stroke="#8b3a1a" stroke-width="0.5"/>
        </svg>
        <span class="guitar-type">Nylon String</span>
        <h3 class="guitar-name">The Classical Guitar</h3>
        <p class="guitar-desc">The oldest of the three, the classical guitar carries centuries of tradition. Nylon strings produce a soft, mellow resonance — ideal for the intricate fingerpicking of flamenco, the delicate arpeggios of Spanish romance, and the depth of Baroque composition.</p>
        <div class="guitar-stats">
          <div class="stat-row">
            <span class="stat-key">Tone</span>
            <span class="stat-val">Mellow & Rich</span>
          </div>
          <div class="stat-row">
            <span class="stat-key">Best for</span>
            <span class="stat-val">Classical, Flamenco</span>
          </div>
          <div class="stat-row">
            <span class="stat-key">Body</span>
            <span class="stat-val">Cedar & Rosewood</span>
          </div>
          <div class="stat-row">
            <span class="stat-key">Strings</span>
            <span class="stat-val">Nylon, 6-string</span>
          </div>
        </div>
      </article>

    </div>
  </section>

  <!-- QUOTE BAND -->
  <div class="quote-band">
    <p class="quote-text">"The guitar is a small orchestra. It is polyphonic. Every string is a different colour, a different voice."</p>
    <span class="quote-attr">— Andrés Segovia</span>
  </div>

  <!-- FOOTER -->
  <footer>
    <p class="footer-title">Three Guitars</p>
    <p class="footer-copy">A celebration of six strings &amp; the sounds they make</p>
  </footer>

  <script>
    const reveals = document.querySelectorAll('.reveal');
    const observer = new IntersectionObserver(entries => {
      entries.forEach(e => {
        if (e.isIntersecting) {
          e.target.classList.add('visible');
          observer.unobserve(e.target);
        }
      });
    }, { threshold: 0.12 });
    reveals.forEach(el => observer.observe(el));
  </script>
</body>
</html>
