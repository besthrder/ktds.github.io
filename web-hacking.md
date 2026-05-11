[web-hacking.html](https://github.com/user-attachments/files/27587564/web-hacking.html)
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>웹 취약점 분석 및 모의해킹 | KTDS</title>
<meta name="description" content="KTDS 임직원을 위한 3일(21H) 웹 취약점 분석 및 모의해킹 과정. OWASP Top 10부터 공격 시나리오 실습까지, 실무 중심 웹 보안 교육." />
<style>
  /* ============ Reset & Base ============ */
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  html { scroll-behavior: smooth; }
  :root {
    --kt-red: #E60012;
    --kt-red-dark: #B8000F;
    --kt-black: #1A1A1A;
    --kt-charcoal: #2C2C2C;
    --kt-gray-900: #333333;
    --kt-gray-700: #555555;
    --kt-gray-500: #888888;
    --kt-gray-300: #D4D4D4;
    --kt-gray-100: #F5F5F5;
    --kt-gray-50: #FAFAFA;
    --kt-white: #FFFFFF;
    --kt-accent: #FF3D4E;
    --shadow-sm: 0 2px 8px rgba(0,0,0,0.06);
    --shadow-md: 0 8px 24px rgba(0,0,0,0.10);
    --shadow-lg: 0 16px 48px rgba(0,0,0,0.16);
    --radius-sm: 8px;
    --radius-md: 14px;
    --radius-lg: 24px;
  }
  body {
    font-family: 'Pretendard', -apple-system, BlinkMacSystemFont, 'Apple SD Gothic Neo', 'Malgun Gothic', '맑은 고딕', sans-serif;
    color: var(--kt-black);
    background: var(--kt-white);
    line-height: 1.6;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  a { color: inherit; text-decoration: none; }
  img { max-width: 100%; display: block; }

  /* ============ Layout ============ */
  .container { width: 100%; max-width: 1180px; margin: 0 auto; padding: 0 24px; }
  section { padding: 96px 0; position: relative; }
  .section-label {
    display: inline-block; font-size: 13px; font-weight: 700; letter-spacing: 0.18em;
    color: var(--kt-red); text-transform: uppercase; margin-bottom: 12px;
  }
  .section-title {
    font-size: 40px; font-weight: 800; line-height: 1.25; color: var(--kt-black);
    margin-bottom: 16px; letter-spacing: -0.02em;
  }
  .section-subtitle {
    font-size: 17px; color: var(--kt-gray-700); margin-bottom: 56px; max-width: 720px;
  }

  /* ============ Navigation ============ */
  .nav {
    position: fixed; top: 0; left: 0; right: 0; z-index: 100;
    background: rgba(26,26,26,0.92); backdrop-filter: blur(12px);
    border-bottom: 1px solid rgba(255,255,255,0.08);
  }
  .nav-inner {
    max-width: 1180px; margin: 0 auto; padding: 14px 24px;
    display: flex; align-items: center; justify-content: space-between;
  }
  .nav-brand { display: flex; align-items: center; gap: 12px; color: var(--kt-white); font-weight: 800; }
  .nav-brand .dot { width: 10px; height: 10px; background: var(--kt-red); border-radius: 2px; }
  .nav-brand .brand-name { font-size: 16px; letter-spacing: -0.01em; }
  .nav-brand .brand-tag {
    font-size: 11px; color: var(--kt-gray-300); font-weight: 500;
    padding: 3px 8px; border: 1px solid rgba(255,255,255,0.15); border-radius: 999px;
  }
  .nav-links { display: flex; gap: 28px; }
  .nav-links a {
    font-size: 14px; color: rgba(255,255,255,0.75); font-weight: 500;
    transition: color 0.2s ease; text-decoration: none; cursor: pointer;
  }
  .nav-links a:hover { color: var(--kt-white); text-decoration: none; }
  .nav-cta {
    background: var(--kt-red); color: var(--kt-white) !important;
    padding: 8px 16px; border-radius: 6px; font-weight: 700 !important;
  }
  .nav-cta:hover { background: var(--kt-red-dark) !important; }
  @media (max-width: 760px) { .nav-links { display: none; } }

  /* ============ HERO ============ */
  .hero {
    background: linear-gradient(135deg, #0F0F10 0%, #1A1A1A 50%, #2C2C2C 100%);
    color: var(--kt-white); padding: 160px 0 48px; overflow: hidden; position: relative;
  }
  .hero::before {
    content: ""; position: absolute; top: -180px; right: -180px; width: 540px; height: 540px;
    background: radial-gradient(circle, rgba(230,0,18,0.22) 0%, transparent 65%);
    filter: blur(20px);
  }
  .hero::after {
    content: ""; position: absolute; bottom: -120px; left: -120px; width: 380px; height: 380px;
    background: radial-gradient(circle, rgba(255,61,78,0.12) 0%, transparent 65%);
    filter: blur(20px);
  }
  .hero-inner { position: relative; z-index: 2; }
  .hero-eyebrow {
    display: inline-flex; align-items: center; gap: 10px;
    padding: 8px 16px; border: 1px solid rgba(255,255,255,0.18);
    border-radius: 999px; background: rgba(255,255,255,0.04);
    font-size: 13px; color: rgba(255,255,255,0.85); margin-bottom: 28px;
    font-weight: 500;
  }
  .hero-eyebrow .pulse {
    width: 8px; height: 8px; border-radius: 50%; background: var(--kt-red);
    box-shadow: 0 0 0 0 rgba(230,0,18,0.6); animation: pulse 1.8s infinite;
  }
  @keyframes pulse {
    0% { box-shadow: 0 0 0 0 rgba(230,0,18,0.6); }
    70% { box-shadow: 0 0 0 12px rgba(230,0,18,0); }
    100% { box-shadow: 0 0 0 0 rgba(230,0,18,0); }
  }
  .hero h1 {
    font-size: clamp(36px, 5.4vw, 64px);
    font-weight: 900; line-height: 1.1; letter-spacing: -0.03em;
    margin-bottom: 24px; max-width: 980px;
  }
  .hero h1 .accent { color: var(--kt-red); }
  .hero h1 .underline {
    background: linear-gradient(180deg, transparent 65%, rgba(230,0,18,0.4) 65%);
    padding: 0 4px;
  }
  .hero-lede {
    font-size: clamp(16px, 1.4vw, 19px); color: rgba(255,255,255,0.78);
    max-width: 720px; margin-bottom: 40px; line-height: 1.65;
  }
  .hero-actions { display: flex; gap: 14px; flex-wrap: wrap; margin-bottom: 56px; }
  .btn-primary {
    display: inline-flex; align-items: center; gap: 8px;
    background: var(--kt-red); color: var(--kt-white);
    padding: 16px 28px; border-radius: 8px; font-weight: 700; font-size: 15px;
    border: none; cursor: pointer; transition: all 0.2s ease;
    box-shadow: 0 8px 24px rgba(230,0,18,0.32);
  }
  .btn-primary:hover { background: var(--kt-red-dark); transform: translateY(-1px); box-shadow: 0 12px 32px rgba(230,0,18,0.42); }
  .btn-ghost {
    display: inline-flex; align-items: center; gap: 8px;
    background: transparent; color: var(--kt-white);
    padding: 16px 24px; border-radius: 8px; font-weight: 600; font-size: 15px;
    border: 1px solid rgba(255,255,255,0.25);
    transition: all 0.2s ease;
  }
  .btn-ghost:hover { background: rgba(255,255,255,0.08); border-color: rgba(255,255,255,0.4); }

  /* Meta strip */
  .hero-meta {
    display: flex; flex-wrap: wrap; gap: 20px;
    padding-top: 32px; border-top: 1px solid rgba(255,255,255,0.10);
  }
  .meta-item { display: flex; flex-direction: column; gap: 4px; }
  .meta-label { font-size: 12px; color: #FFB800; font-weight: 700; letter-spacing: normal; text-transform: none; }
  .meta-value { font-size: 18px; color: var(--kt-white); font-weight: 700; }

  /* ============ Stats ============ */
  .stats { background: var(--kt-gray-50); padding: 88px 0; }
  .stats-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 24px; }
  @media (max-width: 880px) { .stats-grid { grid-template-columns: 1fr; } }
  .stat-card {
    background: var(--kt-white); padding: 36px 32px; border-radius: var(--radius-md);
    box-shadow: var(--shadow-sm); border: 1px solid var(--kt-gray-100);
    position: relative; overflow: hidden; transition: all 0.3s ease;
  }
  .stat-card:hover { transform: translateY(-4px); box-shadow: var(--shadow-md); }
  .stat-card::before {
    content: ""; position: absolute; top: 0; left: 0; width: 4px; height: 100%;
    background: var(--kt-red);
  }
  .stat-number {
    font-size: 56px; font-weight: 900; color: var(--kt-red);
    line-height: 1; letter-spacing: -0.04em; margin-bottom: 12px;
  }
  .stat-number .unit { font-size: 32px; font-weight: 700; }
  .stat-title { font-size: 17px; font-weight: 700; color: var(--kt-black); margin-bottom: 8px; }
  .stat-desc { font-size: 14px; color: var(--kt-gray-700); line-height: 1.55; margin-bottom: 14px; }
  .stat-source {
    font-size: 11px; color: var(--kt-gray-500); font-weight: 500;
    padding-top: 12px; border-top: 1px dashed var(--kt-gray-300);
  }

  /* ============ WHO + OUTCOME ============ */
  .audience { background: var(--kt-white); padding: 110px 0; }
  .audience-grid {
    display: grid; grid-template-columns: 1fr 1fr; gap: 32px; margin-bottom: 56px;
  }
  @media (max-width: 880px) { .audience-grid { grid-template-columns: 1fr; } }
  .audience-card {
    background: var(--kt-gray-50); border-radius: var(--radius-md);
    padding: 36px; border: 1px solid var(--kt-gray-100); transition: all 0.3s ease;
  }
  .audience-card:hover { transform: translateY(-4px); box-shadow: var(--shadow-md); }
  .audience-icon {
    width: 56px; height: 56px; border-radius: 12px;
    background: var(--kt-red); color: var(--kt-white);
    display: flex; align-items: center; justify-content: center;
    font-size: 26px; margin-bottom: 20px;
  }
  .audience-card h3 { font-size: 22px; font-weight: 800; color: var(--kt-black); margin-bottom: 8px; }
  .audience-card .role {
    font-size: 13px; color: var(--kt-red); font-weight: 700; letter-spacing: 0.06em;
    margin-bottom: 20px; text-transform: uppercase;
  }
  .transform-row {
    display: grid; grid-template-columns: 1fr auto 1fr; gap: 16px; align-items: center;
    padding: 14px 0; border-top: 1px solid var(--kt-gray-300);
  }
  .transform-row:first-of-type { border-top: none; padding-top: 4px; }
  .transform-before { color: var(--kt-gray-500); font-size: 14px; text-decoration: line-through; text-decoration-color: var(--kt-gray-300); }
  .transform-arrow { color: var(--kt-red); font-weight: 800; font-size: 14px; }
  .transform-after { color: var(--kt-black); font-size: 14px; font-weight: 600; }

  /* Outcome banner */
  .outcome-banner {
    background: linear-gradient(135deg, var(--kt-black) 0%, var(--kt-charcoal) 100%);
    color: var(--kt-white); border-radius: var(--radius-lg);
    padding: 44px 48px; display: grid; grid-template-columns: 1fr auto; gap: 32px; align-items: center;
    position: relative; overflow: hidden;
  }
  .outcome-banner::after {
    content: ""; position: absolute; top: -80px; right: -80px; width: 240px; height: 240px;
    background: radial-gradient(circle, rgba(230,0,18,0.25) 0%, transparent 70%);
  }
  .outcome-banner > * { position: relative; z-index: 1; }
  @media (max-width: 760px) { .outcome-banner { grid-template-columns: 1fr; padding: 32px 28px; } }
  .outcome-banner h4 { font-size: 14px; color: rgba(255,255,255,0.65); font-weight: 600; margin-bottom: 8px; letter-spacing: 0.04em; }
  .outcome-banner p { font-size: 22px; font-weight: 800; line-height: 1.4; max-width: 640px; }
  .outcome-banner p .hl { color: var(--kt-accent); }
  .outcome-banner .src { font-size: 12px; color: rgba(255,255,255,0.5); margin-top: 12px; }

  /* ============ CURRICULUM ============ */
  .curriculum { background: var(--kt-gray-50); padding: 110px 0; }
  .timeline { display: grid; grid-template-columns: repeat(2, 1fr); gap: 24px; }
  @media (max-width: 880px) { .timeline { grid-template-columns: 1fr; } }
  .day-card {
    background: var(--kt-white); border-radius: var(--radius-md);
    padding: 32px 28px; border: 1px solid var(--kt-gray-100);
    box-shadow: var(--shadow-sm); position: relative; transition: all 0.3s ease;
    display: flex; flex-direction: column;
  }
  .day-card:hover { transform: translateY(-6px); box-shadow: var(--shadow-md); }
  .day-card.featured { border-top: 4px solid var(--kt-red); }
  .day-card.featured-2 { border-top: 4px solid var(--kt-charcoal); }
  .day-header {
    display: flex; align-items: baseline; justify-content: space-between; margin-bottom: 8px;
  }
  .day-number { font-size: 13px; font-weight: 700; color: var(--kt-red); letter-spacing: 0.12em; text-transform: uppercase; }
  .day-hours {
    font-size: 12px; color: var(--kt-white); background: var(--kt-black);
    padding: 4px 10px; border-radius: 999px; font-weight: 700;
  }
  .day-title { font-size: 22px; font-weight: 800; color: var(--kt-black); margin-bottom: 18px; line-height: 1.3; letter-spacing: -0.01em; }
  .module-list { list-style: none; display: flex; flex-direction: column; gap: 14px; flex: 1; }
  .module-item { padding-bottom: 14px; border-bottom: 1px dashed var(--kt-gray-300); }
  .module-item:last-child { border-bottom: none; }
  .module-name {
    font-size: 14px; font-weight: 700; color: var(--kt-black); margin-bottom: 4px;
    display: flex; justify-content: space-between; align-items: center; gap: 8px;
  }
  .module-desc { font-size: 13px; color: var(--kt-gray-700); line-height: 1.5; }
  .deliverable-chip {
    display: inline-block; font-size: 11px; font-weight: 600;
    color: var(--kt-gray-700); background: var(--kt-gray-100);
    padding: 3px 8px; border-radius: 4px; margin-top: 6px; margin-right: 4px;
  }
  .deliverable-chip.hot { color: var(--kt-red); background: rgba(230,0,18,0.08); }

  /* ============ METHODOLOGY ============ */
  .methodology { background: var(--kt-white); padding: 110px 0; }
  .method-flow {
    display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; margin-bottom: 56px;
  }
  @media (max-width: 880px) { .method-flow { grid-template-columns: repeat(2, 1fr); gap: 16px; } }
  .method-step {
    background: var(--kt-gray-50); padding: 22px 16px; border-radius: var(--radius-md);
    text-align: center; border: 1px solid var(--kt-gray-100); position: relative;
  }
  .method-step::after {
    content: "→"; position: absolute; right: -16px; top: 50%; transform: translateY(-50%);
    color: var(--kt-red); font-size: 22px; font-weight: 800;
  }
  .method-step:last-child::after { display: none; }
  @media (max-width: 880px) { .method-step::after { display: none; } }
  .method-step-num {
    width: 32px; height: 32px; border-radius: 50%; background: var(--kt-red); color: var(--kt-white);
    font-size: 14px; font-weight: 800; display: flex; align-items: center; justify-content: center;
    margin: 0 auto 12px;
  }
  .method-step-title { font-size: 14px; font-weight: 700; color: var(--kt-black); margin-bottom: 4px; }
  .method-step-desc { font-size: 12px; color: var(--kt-gray-700); line-height: 1.4; }

  /* Practice split */
  .practice-split { display: grid; grid-template-columns: 1fr 1fr; gap: 24px; }
  @media (max-width: 880px) { .practice-split { grid-template-columns: 1fr; } }
  .practice-card {
    background: var(--kt-gray-50); padding: 36px 32px; border-radius: var(--radius-md);
    border-left: 4px solid var(--kt-red);
  }
  .practice-card h4 {
    font-size: 14px; font-weight: 700; color: var(--kt-red); letter-spacing: 0.12em;
    text-transform: uppercase; margin-bottom: 12px;
  }
  .practice-card h3 { font-size: 22px; font-weight: 800; color: var(--kt-black); margin-bottom: 18px; line-height: 1.3; }
  .practice-card ul { list-style: none; }
  .practice-card li {
    padding: 10px 0 10px 24px; position: relative; font-size: 14px;
    color: var(--kt-gray-900); border-bottom: 1px solid var(--kt-gray-300);
  }
  .practice-card li:last-child { border-bottom: none; }
  .practice-card li::before { content: "✓"; position: absolute; left: 0; color: var(--kt-red); font-weight: 800; }

  /* Prereq callout */
  .prereq-callout {
    background: linear-gradient(135deg, var(--kt-black) 0%, var(--kt-charcoal) 100%);
    color: var(--kt-white); border-radius: var(--radius-lg);
    padding: 28px 36px; margin-bottom: 40px;
    display: grid; grid-template-columns: 56px 1fr; gap: 24px; align-items: center;
    position: relative; overflow: hidden;
  }
  .prereq-callout::after {
    content: ""; position: absolute; top: -60px; right: -60px; width: 220px; height: 220px;
    background: radial-gradient(circle, rgba(230,0,18,0.22) 0%, transparent 70%);
  }
  .prereq-callout > * { position: relative; z-index: 1; }
  .prereq-icon {
    width: 56px; height: 56px; border-radius: 12px;
    background: var(--kt-red); color: var(--kt-white);
    display: flex; align-items: center; justify-content: center; font-size: 22px;
  }
  .prereq-content .prereq-tag {
    display: inline-block; font-size: 11px; font-weight: 700; letter-spacing: 0.12em;
    color: var(--kt-accent); text-transform: uppercase; margin-bottom: 6px;
  }
  .prereq-content h4 { font-size: 19px; font-weight: 800; margin-bottom: 6px; color: var(--kt-white); }
  .prereq-content p { font-size: 14px; color: rgba(255,255,255,0.78); line-height: 1.55; }
  @media (max-width: 760px) { .prereq-callout { grid-template-columns: 1fr; padding: 26px; } .prereq-icon { margin: 0 auto; } }

  /* ============ DIFFERENTIATORS + CTA ============ */
  .differentiators {
    background: linear-gradient(180deg, var(--kt-gray-50) 0%, var(--kt-white) 100%); padding: 110px 0;
  }
  .diff-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 24px; margin-bottom: 56px; }
  @media (max-width: 760px) { .diff-grid { grid-template-columns: 1fr; } }
  .diff-card {
    background: var(--kt-white); padding: 32px; border-radius: var(--radius-md);
    border: 1px solid var(--kt-gray-100); transition: all 0.3s ease; display: flex; gap: 20px;
  }
  .diff-card:hover { transform: translateY(-3px); box-shadow: var(--shadow-md); }
  .diff-icon {
    flex-shrink: 0; width: 48px; height: 48px; border-radius: 10px;
    background: rgba(230,0,18,0.08); color: var(--kt-red);
    display: flex; align-items: center; justify-content: center; font-size: 22px;
  }
  .diff-card h4 { font-size: 17px; font-weight: 800; color: var(--kt-black); margin-bottom: 6px; }
  .diff-card p { font-size: 14px; color: var(--kt-gray-700); line-height: 1.55; }

  /* CTA */
  .cta-final {
    background: var(--kt-black); color: var(--kt-white);
    border-radius: var(--radius-lg); padding: 56px 48px; text-align: center;
    position: relative; overflow: hidden;
  }
  .cta-final::before {
    content: ""; position: absolute; top: -100px; left: 50%; transform: translateX(-50%);
    width: 600px; height: 600px;
    background: radial-gradient(circle, rgba(230,0,18,0.18) 0%, transparent 60%);
  }
  .cta-final > * { position: relative; z-index: 1; }
  .cta-final h2 {
    font-size: clamp(28px, 3vw, 38px); font-weight: 900; line-height: 1.25;
    margin-bottom: 14px; letter-spacing: -0.02em;
  }
  .cta-final h2 .hl { color: var(--kt-red); }
  .cta-final p { font-size: 16px; color: rgba(255,255,255,0.75); margin-bottom: 32px; max-width: 560px; margin-left: auto; margin-right: auto; }
  .cta-meta {
    display: inline-flex; flex-wrap: wrap; gap: 24px; justify-content: center;
    padding: 14px 28px; background: rgba(255,255,255,0.05); border-radius: 999px;
    margin-top: 28px; font-size: 13px; color: rgba(255,255,255,0.8);
  }
  .cta-meta span { display: inline-flex; align-items: center; gap: 6px; }
  .cta-meta span::before { content: "•"; color: var(--kt-red); }
  .cta-meta span:first-child::before { content: ""; }

  /* ============ Footer ============ */
  footer { background: var(--kt-black); color: rgba(255,255,255,0.55); padding: 32px 0; font-size: 13px; }
  .footer-inner {
    max-width: 1180px; margin: 0 auto; padding: 0 24px;
    display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 16px;
  }
  .footer-inner .brand-row { display: flex; align-items: center; gap: 12px; color: var(--kt-white); font-weight: 700; }
  .footer-inner .brand-row .dot { width: 8px; height: 8px; background: var(--kt-red); border-radius: 2px; }

  /* ============ Animations ============ */
  @media (prefers-reduced-motion: no-preference) {
    .reveal { opacity: 0; transform: translateY(20px); transition: all 0.6s ease; }
    .reveal.is-visible { opacity: 1; transform: translateY(0); }
  }

  /* 과정명 태그 */
  .course-name-tag {
    display: inline-block;
    font-size: 22px; font-weight: 800;
    color: #FFB800;
    letter-spacing: -0.01em;
    margin-bottom: 28px;
  }

  /* ============ Mobile ============ */
  @media (max-width: 760px) {
    section { padding: 64px 0; }
    .hero { padding: 130px 0 80px; }
    .section-title { font-size: 30px; }
    .hero-meta { gap: 20px; }
    .stat-number { font-size: 44px; }
    .stat-number .unit { font-size: 26px; }
    .outcome-banner p { font-size: 18px; }
    .cta-final { padding: 44px 28px; }
  }
</style>
</head>
<body>

<!-- ============ NAVIGATION ============ -->
<nav class="nav">
  <div class="nav-inner">
    <a onclick="goTo('hero')" class="nav-brand" style="cursor:pointer;">
      <span class="dot"></span>
      <span class="brand-name">KTDS</span>
      <span class="brand-tag">Security College</span>
    </a>

  </div>
</nav>

<!-- ============ 1) HERO ============ -->
<section class="hero" id="hero">
  <div class="container hero-inner">
    <div class="hero-eyebrow">
      <span class="pulse"></span>
      KTDS Security College · 3일 집중 과정
    </div>
    <div class="course-name-tag">웹 취약점 분석 및 모의해킹</div>
    <h1>
      <span class="accent">웹 취약점</span>을 직접 찾고<br/>
      <span class="underline">공격 시나리오</span>로 대응하는 과정
    </h1>
    <p class="hero-lede" style="white-space: nowrap;">
      OWASP Top 10 취약점 분석부터 실제 공격 시나리오 기반 모의해킹까지 — 웹 애플리케이션 보안 취약점을 진단하고 대응하는 실무 역량을 3일 만에 확보합니다.
    </p>
    <div class="hero-actions">
      <a onclick="goTo('curriculum')" class="btn-primary" style="cursor:pointer;">커리큘럼 보기 →</a>
      <a onclick="goTo('apply')" class="btn-ghost" style="cursor:pointer;">과정 특징</a>
    </div>
    <div class="hero-meta">
      <div class="meta-item">
        <span class="meta-label">교육일정</span>
        <span class="meta-value">06.15 (월) · 06.17 (수)</span>
      </div>
      <div class="meta-item">
        <span class="meta-label">교육기간</span>
        <span class="meta-value">3일 · 21시간</span>
      </div>
      <div class="meta-item">
        <span class="meta-label">강의장소</span>
        <span class="meta-value">209호</span>
      </div>
      <div class="meta-item">
        <span class="meta-label">실습비중</span>
        <span class="meta-value">이론 40% / 실습 60%</span>
      </div>
      <div class="meta-item">
        <span class="meta-label">추천직무</span>
        <span class="meta-value">웹 개발자 · 보안 담당자 · 취약점 분석 담당자</span>
      </div>

    </div>
  </div>
</section>

<!-- ============ 2) WHY (Stats) ============ -->
<section class="stats" id="why">
  <div class="container">
    <span class="section-label">Why Web Security</span>
    <h2 class="section-title">왜 지금 웹 취약점 분석인가</h2>
    <p class="section-subtitle" style="max-width: 100%; white-space: nowrap;">웹 서비스가 공격의 주 타깃이 된 지 오래입니다. 취약점을 직접 찾고 대응하지 못하면 서비스 전체가 위험해집니다.</p>
    <div class="stats-grid">
      <div class="stat-card">
        <div class="stat-number">75<span class="unit">%</span></div>
        <div class="stat-title">사이버 공격의 웹 애플리케이션 타깃 비율</div>
        <div class="stat-desc">전체 사이버 공격의 75% 이상이 웹 애플리케이션을 대상으로 합니다. SQL Injection, XSS 등 웹 취약점 이해는 필수 보안 역량입니다.</div>
        <div class="stat-source">Verizon · Data Breach Investigations Report 2024</div>
      </div>
      <div class="stat-card">
        <div class="stat-number">10<span class="unit">배</span></div>
        <div class="stat-title">모의해킹 경험자의 취약점 탐지 속도</div>
        <div class="stat-desc">실제 공격 시나리오를 직접 경험한 보안 담당자는 그렇지 않은 경우보다 취약점 탐지와 대응 속도가 평균 10배 이상 빠릅니다.</div>
        <div class="stat-source">KTDS Security College</div>
      </div>
      <div class="stat-card">
        <div class="stat-number">3<span class="unit">일</span></div>
        <div class="stat-title">실무 취약점 분석 역량 확보까지 걸리는 시간</div>
        <div class="stat-desc">취약점 진단, 공격 시나리오 분석, 모의해킹 실습까지 — 3일 과정으로 현장에서 바로 활용 가능한 웹 보안 역량을 확보합니다.</div>
        <div class="stat-source">KTDS Security College · 과정 설계 기준</div>
      </div>
    </div>
  </div>
</section>

<!-- ============ 3) WHO + OUTCOME ============ -->
<section class="audience" id="audience">
  <div class="container">
    <span class="section-label">Who</span>
    <h2 class="section-title">이런 분들을 위한 과정입니다</h2>
    <p class="section-subtitle" style="max-width: 100%; white-space: nowrap;">웹 애플리케이션 보안 취약점을 이해하고 실무에서 직접 분석·대응해야 하는 모든 담당자에게 열려 있습니다.</p>

    <div class="audience-grid">
      <div class="audience-card">
        <div class="audience-icon">🌐</div>
        <h3>웹 서비스 개발자</h3>
        <div class="role">Web Developer</div>
        <div class="transform-row">
          <span class="transform-before">보안 취약점 인식 부족</span>
          <span class="transform-arrow">→</span>
          <span class="transform-after">OWASP Top 10 기반 보안 이해</span>
        </div>
        <div class="transform-row">
          <span class="transform-before">SQL Injection·XSS 개념 모호</span>
          <span class="transform-arrow">→</span>
          <span class="transform-after">주요 취약점 직접 분석·실습</span>
        </div>
        <div class="transform-row">
          <span class="transform-before">보안 코드 작성 기준 없음</span>
          <span class="transform-arrow">→</span>
          <span class="transform-after">취약점 대응 및 보안 강화 적용</span>
        </div>
      </div>
      <div class="audience-card">
        <div class="audience-icon">🛡️</div>
        <h3>보안 담당자</h3>
        <div class="role">Security Officer</div>
        <div class="transform-row">
          <span class="transform-before">이론 중심 보안 지식</span>
          <span class="transform-arrow">→</span>
          <span class="transform-after">공격 시나리오 기반 실전 대응</span>
        </div>
        <div class="transform-row">
          <span class="transform-before">취약점 진단 도구 활용 미흡</span>
          <span class="transform-arrow">→</span>
          <span class="transform-after">OWASP ZAP·Burp Suite 실습</span>
        </div>
        <div class="transform-row">
          <span class="transform-before">모의해킹 경험 부재</span>
          <span class="transform-arrow">→</span>
          <span class="transform-after">웹 모의해킹 시나리오 직접 실습</span>
        </div>
      </div>
      <div class="audience-card">
        <div class="audience-icon">💻</div>
        <h3>애플리케이션 개발 담당자</h3>
        <div class="role">Application Developer</div>
        <div class="transform-row">
          <span class="transform-before">인증·세션 관리 취약 설계</span>
          <span class="transform-arrow">→</span>
          <span class="transform-after">인증·세션 관리 취약점 분석·개선</span>
        </div>
        <div class="transform-row">
          <span class="transform-before">접근 제어 취약점 인식 없음</span>
          <span class="transform-arrow">→</span>
          <span class="transform-after">접근 제어 취약점 분석 실습</span>
        </div>
        <div class="transform-row">
          <span class="transform-before">보안 테스트 방법 모름</span>
          <span class="transform-arrow">→</span>
          <span class="transform-after">취약점 탐지 및 분석 도구 활용</span>
        </div>
      </div>
      <div class="audience-card">
        <div class="audience-icon">🔍</div>
        <h3>보안 취약점 분석 업무 담당자</h3>
        <div class="role">Vulnerability Analyst</div>
        <div class="transform-row">
          <span class="transform-before">취약점 분석 결과 보고 미흡</span>
          <span class="transform-arrow">→</span>
          <span class="transform-after">취약점 분석 결과 보고서 작성</span>
        </div>
        <div class="transform-row">
          <span class="transform-before">공격 시나리오 설계 경험 없음</span>
          <span class="transform-arrow">→</span>
          <span class="transform-after">공격 시나리오 직접 설계·실습</span>
        </div>
        <div class="transform-row">
          <span class="transform-before">취약점 대응 방안 도출 어려움</span>
          <span class="transform-arrow">→</span>
          <span class="transform-after">취약점 대응 및 보안 설정 실습</span>
        </div>
      </div>
    </div>

    <div class="outcome-banner">
      <div>
        <h4>교육 후 기대효과</h4>
        <p><span class="hl">취약점 분석 도구로 웹 취약점을 식별</span>하고<br/>공격 시나리오 기반으로 <span class="hl">취약점 분석 및 대응 역량을 확보</span>할 수 있게 됩니다.</p>
        <p class="src">취약점 진단부터 모의해킹 시나리오까지 3일 집중 실습으로 완성</p>
      </div>
    </div>
  </div>
</section>

<!-- ============ 4) CURRICULUM ============ -->
<section class="curriculum" id="curriculum">
  <div class="container">
    <span class="section-label">Curriculum</span>
    <h2 class="section-title">3일 커리큘럼</h2>
    <p class="section-subtitle" style="white-space: nowrap;">
      웹 취약점 진단 이해부터 취약점 분석, 모의해킹 실습까지 — 매일 실습 산출물을 만들며 체득합니다.
    </p>
    <div class="timeline">

      <!-- Day 1 -->
      <div class="day-card featured">
        <div class="day-header">
          <span class="day-number">Day 1 · 웹 취약점 진단 이해</span>
          <span class="day-hours">7H</span>
        </div>
        <h3 class="day-title">"웹 취약점이 무엇이고 어떻게 진단하는지" 이해하는 날</h3>
        <ul class="module-list">
          <li class="module-item">
            <div class="module-name">웹 애플리케이션 보안 개요</div>
            <div class="module-desc">웹 서비스 구조와 보안 위협의 관계, 웹 보안의 핵심 원칙과 주요 공격 유형 이해</div>
          </li>
          <li class="module-item">
            <div class="module-name">웹 취약점 진단 절차 이해</div>
            <div class="module-desc">취약점 진단 프로세스, 진단 범위 설정, 취약점 분류 체계와 위험도 평가 방법</div>
          </li>
          <li class="module-item">
            <div class="module-name">OWASP Top 10 취약점 분석</div>
            <div class="module-desc">전 세계 표준 웹 취약점 분류인 OWASP Top 10 항목별 원리, 발생 원인, 실제 사례 분석</div>
          </li>
          <li class="module-item">
            <div class="module-name">웹 공격 유형 이해</div>
            <div class="module-desc">인젝션, 인증 우회, 정보 노출 등 주요 웹 공격 유형별 동작 원리와 특징 파악</div>
            <span class="deliverable-chip hot">취약점 테스트 환경 구성</span>
            <span class="deliverable-chip hot">웹 취약점 진단 도구 활용 실습</span>
          </li>
        </ul>
      </div>

      <!-- Day 2 -->
      <div class="day-card featured-2">
        <div class="day-header">
          <span class="day-number">Day 2 · 웹 취약점 분석</span>
          <span class="day-hours">7H</span>
        </div>
        <h3 class="day-title">"주요 취약점을 직접 분석하고 탐지하는" 날</h3>
        <ul class="module-list">
          <li class="module-item">
            <div class="module-name">SQL Injection 취약점 분석</div>
            <div class="module-desc">SQL Injection 원리, 공격 유형(Error·Blind·Time-based), 탐지 및 방어 방법 실습</div>
          </li>
          <li class="module-item">
            <div class="module-name">Cross-Site Scripting(XSS) 분석</div>
            <div class="module-desc">Stored·Reflected·DOM 기반 XSS 공격 원리와 필터링 우회 기법, 방어 코딩 방법</div>
          </li>
          <li class="module-item">
            <div class="module-name">인증·세션 관리 취약점 · 접근 제어 취약점 분석</div>
            <div class="module-desc">세션 하이재킹, 토큰 취약점, 수평·수직 권한 상승 공격 분석과 안전한 인증 설계 방법</div>
            <span class="deliverable-chip hot">공격 시나리오 기반 취약점 분석 실습</span>
            <span class="deliverable-chip hot">취약점 탐지 및 분석 실습</span>
          </li>
        </ul>
      </div>

      <!-- Day 3 -->
      <div class="day-card" style="border-top: 4px solid #555555;">
        <div class="day-header">
          <span class="day-number">Day 3 · 모의해킹 실습</span>
          <span class="day-hours">7H</span>
        </div>
        <h3 class="day-title">"실제 공격 시나리오로 모의해킹을 직접 수행하는" 날</h3>
        <ul class="module-list">
          <li class="module-item">
            <div class="module-name">웹 모의해킹 절차 이해</div>
            <div class="module-desc">모의해킹 수행 단계(정찰·스캐닝·익스플로잇·보고), 법적·윤리적 범위와 허가 기반 수행 원칙</div>
          </li>
          <li class="module-item">
            <div class="module-name">공격 시나리오 설계</div>
            <div class="module-desc">실제 웹 서비스를 대상으로 한 공격 시나리오 구성 방법과 취약점 연계 공격 흐름 설계</div>
          </li>
          <li class="module-item">
            <div class="module-name">취약점 대응 및 보안 강화 · 취약점 분석 결과 보고</div>
            <div class="module-desc">발견된 취약점에 대한 보안 설정 및 코드 수준 대응 방법, 취약점 분석 보고서 작성 방법</div>
            <span class="deliverable-chip hot">웹 모의해킹 시나리오 실습</span>
            <span class="deliverable-chip hot">취약점 대응 및 보안 설정 실습</span>
          </li>
        </ul>
      </div>

    </div>
  </div>
</section>

<!-- ============ 5) METHODOLOGY ============ -->
<section class="methodology" id="methodology">
  <div class="container">
    <span class="section-label">Methodology</span>
    <h2 class="section-title">우리는 이렇게 가르칩니다</h2>
    <p class="section-subtitle" style="max-width: 100%; white-space: nowrap;">
      이론 강의로 끝나지 않습니다. DVWA·WebGoat 실습 환경에서 취약점을 직접 공격하고 방어하는 실습 60% 과정입니다.
    </p>

    <div class="method-flow">
      <div class="method-step">
        <div class="method-step-num">1</div>
        <div class="method-step-title">취약점 이해</div>
        <div class="method-step-desc">OWASP Top 10·웹 공격 유형 파악</div>
      </div>
      <div class="method-step">
        <div class="method-step-num">2</div>
        <div class="method-step-title">취약점 분석</div>
        <div class="method-step-desc">SQL Injection·XSS·인증 취약점 실습</div>
      </div>
      <div class="method-step">
        <div class="method-step-num">3</div>
        <div class="method-step-title">모의해킹</div>
        <div class="method-step-desc">공격 시나리오 설계·실습 수행</div>
      </div>
      <div class="method-step">
        <div class="method-step-num">4</div>
        <div class="method-step-title">대응·보고</div>
        <div class="method-step-desc">보안 강화·취약점 분석 보고서 작성</div>
      </div>
    </div>

    <div class="prereq-callout">
      <div class="prereq-icon">📚</div>
      <div class="prereq-content">
        <span class="prereq-tag">사전 지식 안내</span>
        <h4>이 정도면 충분합니다</h4>
        <p>웹 애플리케이션 구조 이해, HTTP 및 웹 서비스 기본 이해, 기본적인 보안 개념 이해 수준이면 누구나 참여할 수 있습니다. 연계 과정으로 Secure Coding 및 애플리케이션 보안, 보안 로그 분석 및 침해 대응을 참고하시기 바랍니다.</p>
      </div>
    </div>

    <div class="practice-split">
      <div class="practice-card">
        <h4>제공 기술·플랫폼</h4>
        <h3>실습에 바로 활용 가능한 환경을 제공합니다.</h3>
        <ul>
          <li>웹 취약점 실습 환경 (DVWA / WebGoat 등)</li>
          <li>취약점 분석 도구 (OWASP ZAP / Burp Suite 등)</li>
          <li>웹 애플리케이션 테스트 환경</li>
        </ul>
      </div>
      <div class="practice-card">
        <h4>실습 산출물</h4>
        <h3>매일 손으로 만들어 가져가는 결과물이 있습니다.</h3>
        <ul>
          <li>Day 1: 취약점 테스트 환경 구성 · 웹 취약점 진단 도구 활용 실습 결과물</li>
          <li>Day 2: 공격 시나리오 기반 취약점 분석 · 취약점 탐지 및 분석 실습 결과물</li>
          <li>Day 3: 웹 모의해킹 시나리오 실습 · 취약점 대응 및 보안 설정 실습 결과물</li>
        </ul>
      </div>
    </div>
  </div>
</section>

<!-- ============ 6) DIFFERENTIATORS + CTA ============ -->
<section class="differentiators" id="apply">
  <div class="container">
    <span class="section-label">Why This Course</span>
    <h2 class="section-title">다른 보안 교육과 무엇이 다릅니까?</h2>
    <p class="section-subtitle" style="white-space: nowrap;">
      취약점 개념 강의로 끝나지 않습니다. 실제 공격 환경에서 직접 취약점을 찾고 모의해킹을 수행하는 과정입니다.
    </p>

    <div class="diff-grid">
      <div class="diff-card">
        <div class="diff-icon">🎯</div>
        <div>
          <h4>진단 → 분석 → 모의해킹 한 흐름으로</h4>
          <p>취약점 진단 이해(Day 1) → 주요 취약점 분석(Day 2) → 공격 시나리오 기반 모의해킹(Day 3). 분절된 강의가 아닌 실전 보안 역량을 쌓는 하나의 여정으로 완성됩니다.</p>
        </div>
      </div>
      <div class="diff-card">
        <div class="diff-icon">🔍</div>
        <div>
          <h4>DVWA·WebGoat 실습 환경 제공</h4>
          <p>실제 취약한 웹 애플리케이션 실습 환경(DVWA / WebGoat)에서 직접 공격하고 방어합니다. 이론이 아닌 손으로 체득하는 보안 교육입니다.</p>
        </div>
      </div>
      <div class="diff-card">
        <div class="diff-icon">⚡</div>
        <div>
          <h4>실습비중 60%</h4>
          <p>이론 40%, 실습 60%로 구성됩니다. 취약점 탐지, 공격 시나리오 실습, 보안 설정까지 강의실에서 직접 공격하고 방어하며 역량을 체득합니다.</p>
        </div>
      </div>
      <div class="diff-card">
        <div class="diff-icon">🧭</div>
        <div>
          <h4>취약점 분석 보고서까지 완성</h4>
          <p>모의해킹으로 끝나지 않습니다. 발견된 취약점에 대한 대응 방안과 취약점 분석 결과 보고서까지 작성해 실무에 바로 활용할 수 있습니다.</p>
        </div>
      </div>
    </div>

    <!-- Final CTA -->
    <div class="cta-final">
      <h2>"공격자의 시각으로 취약점을 찾고<br/><span class="hl">방어자로서 대응합니다.</span>"</h2>
      <p>3일이면 충분합니다. 웹 보안 역량을 팀의 기본 역량으로 만들어 보세요.</p>
      <div class="cta-meta">
        <span>3일 21시간</span>
        <span>이론 40% · 실습 60%</span>
        <span>웹 개발자 · 보안 담당자 · 취약점 분석 담당자</span>
        <span>06.15 (월) · 06.17 (수) · 209호</span>
      </div>
    </div>
  </div>
</section>

<!-- ============ FOOTER ============ -->
<footer>
  <div class="footer-inner">
    <div class="brand-row">
      <span class="dot"></span>
      <span>KTDS Security College</span>
    </div>
    <div>
      © 2026 KTDS. 웹 취약점 분석 및 모의해킹 과정 · 사내 교육 자료
    </div>
  </div>
</footer>

<script>
  // 부드러운 스크롤 이동 함수
  function goTo(id) {
    const target = document.getElementById(id);
    if (!target) return;
    const navHeight = document.querySelector('.nav').offsetHeight;
    const top = target.getBoundingClientRect().top + window.scrollY - navHeight - 16;
    window.scrollTo({ top: top, behavior: 'smooth' });
  }

  // 스크롤 애니메이션
  (function () {
    if (!('IntersectionObserver' in window)) return;
    const targets = document.querySelectorAll('.stat-card, .audience-card, .day-card, .method-step, .practice-card, .diff-card, .outcome-banner');
    targets.forEach(el => el.classList.add('reveal'));
    const io = new IntersectionObserver(entries => {
      entries.forEach(e => {
        if (e.isIntersecting) {
          e.target.classList.add('is-visible');
          io.unobserve(e.target);
        }
      });
    }, { threshold: 0.12 });
    targets.forEach(el => io.observe(el));
  })();

</script>

</body>
</html>
