Build a lightweight plain HTML/CSS/JS slide-style demo site for a project called "{{PROJECT_NAME}}".

The output should feel like a premium SaaS/product demo deck, not a scrollable landing page. It must be a single-viewport, non-overflowing horizontal slide
experience with reusable slide data and polished product-site styling.

Before building, ask me these questions one at a time, waiting for my answer after each:

Question 1:
“What hero mockup do you want?”

- If I say “none”, “default”, or give no specific mockup, use a polished code-native dashboard/product mockup made with HTML/CSS.
- The default mockup should look like a floating SaaS dashboard window: rounded browser shell, three small window dots, search pill, left sidebar, content
  panels, task rows, timeline pills, and small chart bars.
- If I describe a specific mockup or visual, use image generation only when a raster/image asset is appropriate. Otherwise build it code-native.

Question 2:
“Do you want a color scheme or visual style?”

- Examples: light mode, dark mode, cyberpunk, editorial, enterprise, warm, monochrome, playful.
- If I say “none” or “default”, use the default premium light theme:
  - white background
  - dark ink text
  - muted slate body copy
  - soft blue radial glow
  - subtle peach/violet accents
  - pill CTAs
  - rounded dashboard panels
  - restrained shadows
  - calm SaaS typography

Question 3:
“How many slides do you want, and what is the title of each slide?”

- Ask only for slide count and titles.
- After I answer, infer concise supporting copy and CTAs unless I provide exact copy.
- The first slide must be the hero.
- One slide should be informational/workflow-oriented unless the titles clearly imply otherwise.
- One slide should be a demo/media slide when appropriate.
- The hero CTA should navigate to the demo/media slide by default.

Implementation requirements:

- Use plain `index.html`, `styles.css`, and `app.js`.
- No React, no framework, no build tooling unless explicitly requested.
- The site must work by opening `index.html` directly in a browser.
- The page must not scroll vertically or horizontally through native browser scroll.
- Use slide transitions via CSS transform.
- Keep slide configuration in a single `slides` array in `app.js`.
- Make CTAs and slide order easy to edit.
- Include nav links, previous/next buttons, dots, hash routing, and keyboard navigation.
- Use `inert` on inactive slides.
- Pause videos when leaving the demo slide.
- Use `prefers-reduced-motion`.
- Avoid visual clutter, oversized decorative blobs, and long landing-page sections.

Use this HTML structure closely:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{{PROJECT_NAME}} | Demo</title>
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <main class="deck-shell" aria-label="{{PROJECT_NAME}} demo slides">
      <nav class="top-nav" aria-label="Main navigation">
        <a class="wordmark" href="#slide-1" data-goto="0" aria-label="{{PROJECT_NAME}} home">
          <span>{{PROJECT_INITIAL}}</span>
          {{PROJECT_NAME}}
        </a>

        <div class="nav-links" aria-label="Slide navigation"></div>

        <button class="nav-cta" type="button" data-goto="{{DEMO_SLIDE_INDEX}}">View demo</button>
      </nav>

      <section class="slides-viewport" aria-live="polite">
        <div class="slides-track"></div>
      </section>

      <div class="deck-controls" aria-label="Presentation controls">
        <button class="icon-button" type="button" data-prev aria-label="Previous slide">
          <svg viewBox="0 0 24 24" aria-hidden="true">
            <path d="m15 18-6-6 6-6" />
          </svg>
        </button>

        <div class="slide-dots" aria-label="Slide picker"></div>

        <button class="icon-button" type="button" data-next aria-label="Next slide">
          <svg viewBox="0 0 24 24" aria-hidden="true">
            <path d="m9 6 6 6-6 6" />
          </svg>
        </button>
      </div>
    </main>

    <script src="app.js" defer></script>
  </body>
</html>

Use this JS pattern closely: const slides = [ { id: 'hero', navLabel: 'Intro', render: () => `
<article class="slide hero-slide" aria-labelledby="hero-title">
  <div class="hero-copy">
    <p class="eyebrow">{{HERO_EYEBROW}}</p>
    <h1 id="hero-title">{{HERO_TITLE}}</h1>
    <p class="hero-subtitle">{{HERO_SUBTITLE}}</p>
    <div class="hero-actions">
      <button class="button button-primary" type="button" data-goto="{{DEMO_SLIDE_INDEX}}">View demo</button>
      <button class="button button-secondary" type="button" data-goto="1">How it works</button>
    </div>
  </div>
  ${heroMockup()}
</article>
`, }, { id: 'workflow', navLabel: 'Workflow', render: () => `
<article class="slide info-slide" aria-labelledby="workflow-title">
  <div class="info-panel">
    <div class="info-copy">
      <p class="info-kicker">Build flow</p>
      <h2 id="workflow-title">{{INFO_TITLE}}</h2>
      <p>{{INFO_COPY}}</p>
      <div class="info-actions">
        <button class="button button-primary" type="button" data-goto="{{DEMO_SLIDE_INDEX}}">View demo</button>
        <button class="button button-secondary" type="button" data-goto="0">Back to hero</button>
      </div>
    </div>
    <div class="info-stack" aria-label="Workflow highlights">
      ${featureCard('01', 'Plan', 'Turn the request into scoped work.')} ${featureCard('02', 'Build', 'Create the core
      product experience.')} ${featureCard('03', 'Review', 'Check the result before sharing.')}
    </div>
  </div>
</article>
`, }, { id: 'demo', navLabel: 'Demo', render: () => `
<article class="slide demo-slide" aria-labelledby="demo-title">
  <div class="demo-frame">
    <div class="demo-heading">
      <h2 id="demo-title">{{DEMO_TITLE}}</h2>
      <p>{{DEMO_COPY}}</p>
    </div>
    <video class="demo-video" src="{{DEMO_VIDEO_SRC}}" controls playsinline preload="metadata"></video>
  </div>
</article>
`, }, ] let activeSlide = getInitialSlide() const track = document.querySelector('.slides-track') const navLinks =
document.querySelector('.nav-links') const dots = document.querySelector('.slide-dots') const deck =
document.querySelector('.deck-shell') track.innerHTML = slides.map((slide) => slide.render()).join('')
deck.style.setProperty('--slide-count', slides.length) navLinks.innerHTML = slides .map((slide, index) => `<button
  type="button"
  data-goto="${index}"
>
  ${slide.navLabel}</button
>`) .join('') dots.innerHTML = slides .map( (slide, index) => `<button
  class="slide-dot"
  type="button"
  data-goto="${index}"
  aria-label="Go to ${slide.navLabel}"
></button
>`, ) .join('') document.addEventListener('click', (event) => { const trigger = event.target.closest('[data-goto],
[data-prev], [data-next]') if (!trigger) return if (trigger.hasAttribute('data-prev')) { goTo(activeSlide - 1) return }
if (trigger.hasAttribute('data-next')) { goTo(activeSlide + 1) return } goTo(Number(trigger.dataset.goto)) })
document.addEventListener('keydown', (event) => { if (event.key === 'ArrowRight' || event.key === 'PageDown')
goTo(activeSlide + 1) if (event.key === 'ArrowLeft' || event.key === 'PageUp') goTo(activeSlide - 1) })
window.addEventListener('hashchange', () => { const slideIndex = getInitialSlide() if (slideIndex !== activeSlide)
goTo(slideIndex, { replace: true }) }) goTo(activeSlide, { replace: true }) function goTo(index, options = {}) {
activeSlide = wrap(index) deck.style.setProperty('--active-slide', activeSlide) document.querySelectorAll('.nav-links
[data-goto], .slide-dot').forEach((item) => { const isActive = Number(item.dataset.goto) === activeSlide
item.toggleAttribute('aria-current', isActive) }) document.querySelectorAll('.slide').forEach((slide, index) => {
slide.toggleAttribute('inert', index !== activeSlide) }) document.querySelectorAll('.demo-video').forEach((video) => {
if (activeSlide !== Number('{{DEMO_SLIDE_INDEX}}')) video.pause() }) const hash = `#slide-${activeSlide + 1}` if
(window.location.hash !== hash) { if (options.replace) window.history.replaceState(null, '', hash) else
window.history.pushState(null, '', hash) } } function wrap(index) { return (index + slides.length) % slides.length }
function getInitialSlide() { const match = window.location.hash.match(/^#slide-(\d+)$/) if (!match) return 0 return
Math.min(Math.max(Number(match[1]) - 1, 0), slides.length - 1) } function featureCard(number, title, copy) { return `
<section class="feature-card">
  <span class="stat-label">${number}</span>
  <h3>${title}</h3>
  <p>${copy}</p>
</section>
` } For the default hero mockup, use this function shape: function heroMockup() { const tasks = ['Scaffold app shell',
'Wire core flow', 'Build dashboard', 'Review launch'] return `
<div class="hero-visual" aria-label="Product workspace preview">
  <div class="visual-glow"></div>

  <div class="dashboard-shell">
    <div class="window-bar">
      <span></span>
      <span></span>
      <span></span>
      <div class="window-search">Search workspace</div>
    </div>

    <div class="dashboard-layout">
      <aside class="dashboard-sidebar" aria-label="Workspace sidebar">
        <div class="sidebar-brand">{{PROJECT_INITIAL}}</div>
        <div class="sidebar-pill active"></div>
        <div class="sidebar-pill"></div>
        <div class="sidebar-pill short"></div>
        <div class="sidebar-spacer"></div>
        <div class="sidebar-dot"></div>
      </aside>

      <div class="dashboard-main">
        <div class="dashboard-topline">
          <div>
            <p>Today</p>
            <h2>{{MOCKUP_TITLE}}</h2>
          </div>
          <div class="topline-actions">
            <span>Spec</span>
            <span>Demo</span>
          </div>
        </div>

        <div class="dashboard-grid">
          <section class="panel task-panel">
            <div class="panel-header">
              <span>Build queue</span>
              <small>4 steps</small>
            </div>

            ${tasks .map( (task, index) => `
            <div class="task-row">
              <span class="check check-${index + 1}"></span>
              <p>${task}</p>
              <small>${index + 1}d</small>
            </div>
            `, ) .join('')}
          </section>

          <section class="panel calendar-panel">
            <div class="panel-header">
              <span>Release path</span>
              <small>v1</small>
            </div>
            <div class="calendar-lines"><span></span><span></span><span></span><span></span><span></span></div>
            <div class="timeline-card primary">Implementation</div>
            <div class="timeline-card secondary">QA pass</div>
          </section>

          <section class="panel chart-panel">
            <div class="panel-header">
              <span>Readiness</span>
              <small>92%</small>
            </div>
            <div class="bars">
              <span style="height: 44%"></span>
              <span style="height: 62%"></span>
              <span style="height: 50%"></span>
              <span style="height: 78%"></span>
              <span style="height: 68%"></span>
              <span style="height: 86%"></span>
            </div>
          </section>
        </div>
      </div>
    </div>
  </div>
</div>
` } Use this CSS foundation closely, adapting only colors, fonts, and spacing when the requested theme requires it:
:root { --ink: #101725; --muted: #667085; --soft: #8a95a6; --line: rgba(15, 23, 42, 0.1); --paper: #ffffff; --mist:
#f8fafc; --blue: #94caff; --peach: #ffb28e; --violet: #b7a7ff; --sans: Inter, ui-sans-serif, system-ui, -apple-system,
BlinkMacSystemFont, "Segoe UI", sans-serif; --accent: "Bodoni 72", Didot, "Bodoni 72 Smallcaps", Baskerville, "Times New
Roman", serif; color: var(--ink); background: var(--paper); font-family: var(--sans); font-synthesis: none;
text-rendering: optimizeLegibility; -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; } * {
box-sizing: border-box; } html, body { width: 100%; height: 100%; margin: 0; overflow: hidden; background: var(--paper);
} button { font: inherit; } .deck-shell { position: relative; width: 100vw; height: 100svh; min-width: 320px; overflow:
hidden; color: var(--ink); background: linear-gradient(180deg, rgba(255, 255, 255, 0) 0, var(--paper) 28rem),
radial-gradient(circle at 50% 10%, rgba(113, 179, 255, 0.2), rgba(255, 255, 255, 0) 34rem), var(--paper); isolation:
isolate; } .top-nav { position: fixed; top: 24px; left: 50%; z-index: 20; width: min(1240px, calc(100vw - 96px));
height: 56px; display: grid; grid-template-columns: 1fr auto 1fr; align-items: center; transform: translateX(-50%);
color: var(--muted); font-size: 13px; font-weight: 560; } .wordmark, .nav-links button, .nav-cta, .button { color:
inherit; text-decoration: none; } .wordmark { justify-self: start; display: inline-flex; align-items: center; gap: 10px;
color: var(--ink); font-weight: 700; } .wordmark span { width: 26px; height: 26px; border-radius: 8px; display: grid;
place-items: center; background: linear-gradient(135deg, rgba(255, 255, 255, 0.72), rgba(255, 255, 255, 0)),
linear-gradient(135deg, #0e1726, #526377); box-shadow: 0 10px 22px rgba(14, 23, 38, 0.18); color: #fff; font-size: 13px;
font-weight: 800; } .nav-links { display: inline-flex; gap: 30px; justify-content: center; } .nav-links button,
.nav-cta, .icon-button { border: 0; cursor: pointer; } .nav-links button { padding: 0; color: inherit; background:
transparent; transition: color 180ms ease; } .nav-links button:hover, .nav-links button[aria-current="page"],
.nav-cta:hover { color: var(--ink); } .nav-cta { justify-self: end; padding: 10px 16px; border: 1px solid rgba(18, 24,
38, 0.1); border-radius: 999px; color: var(--ink); background: rgba(255, 255, 255, 0.72); box-shadow: 0 12px 30px
rgba(15, 23, 42, 0.06); } .slides-viewport { width: 100%; height: 100%; overflow: hidden; } .slides-track { display:
flex; width: calc(var(--slide-count, 3) * 100vw); height: 100%; transform: translate3d(calc(var(--active-slide, 0) *
-100vw), 0, 0); transition: transform 620ms cubic-bezier(0.22, 1, 0.36, 1); will-change: transform; } .slide { position:
relative; flex: 0 0 100vw; width: 100vw; height: 100svh; padding: 104px 48px 92px; display: grid; place-items: center;
overflow: hidden; } .hero-slide { grid-template-rows: auto minmax(0, 1fr); align-content: center; gap: clamp(24px,
4.2vh, 54px); padding-bottom: 52px; } .hero-copy { position: relative; z-index: 2; width: min(900px, 100%); margin: 0
auto; text-align: center; } .eyebrow { margin: 0 0 22px; font-family: var(--accent); font-size: 22px; font-style:
italic; line-height: 1.2; color: #626a78; } .hero-copy h1, .info-copy h2 { margin: 0 auto; color: var(--ink);
letter-spacing: -0.065em; } .hero-copy h1 { max-width: 900px; font-size: clamp(56px, 7.1vw, 112px); line-height: 0.93; }
.hero-subtitle { width: min(590px, 100%); margin: 28px auto 0; color: var(--muted); font-size: 17px; line-height: 1.65;
} .hero-actions, .info-actions { display: flex; justify-content: center; gap: 12px; margin-top: 34px; } .button {
display: inline-flex; min-height: 48px; align-items: center; justify-content: center; border: 0; border-radius: 999px;
padding: 0 22px; font-size: 14px; font-weight: 680; cursor: pointer; transition: transform 180ms ease, box-shadow 180ms
ease, border-color 180ms ease; } .button:hover { transform: translateY(-1px); } .button-primary { color: #fff;
background: var(--ink); box-shadow: 0 16px 34px rgba(16, 23, 37, 0.2); } .button-secondary { color: var(--ink); border:
1px solid rgba(15, 23, 42, 0.1); background: rgba(255, 255, 255, 0.64); } .hero-visual { position: relative; z-index: 1;
width: min(1040px, calc(100vw - 120px)); height: min(44vh, 520px); min-height: 300px; margin-top: 108px; display: grid;
place-items: center; pointer-events: none; } .visual-glow { position: absolute; inset: 12px 0 0; z-index: -1;
border-radius: 56px; background: radial-gradient(circle at 28% 20%, rgba(101, 177, 255, 0.66), transparent 32%),
radial-gradient(circle at 65% 22%, rgba(255, 128, 116, 0.54), transparent 28%), radial-gradient(circle at 78% 76%,
rgba(144, 119, 255, 0.38), transparent 32%), linear-gradient(135deg, rgba(239, 248, 255, 0.88), rgba(255, 251, 246,
0.8)); filter: blur(2px); opacity: 0.78; transform: perspective(1000px) rotateX(58deg) rotateZ(-5deg) translateY(38px);
} .dashboard-shell { width: 980px; height: 500px; overflow: hidden; border: 1px solid rgba(12, 19, 32, 0.08);
border-radius: 34px; background: linear-gradient(180deg, rgba(255, 255, 255, 0.92), rgba(250, 252, 255, 0.96)), #fff;
box-shadow: 0 58px 120px rgba(26, 39, 67, 0.2), 0 18px 48px rgba(93, 113, 152, 0.13), inset 0 1px 0 rgba(255, 255, 255,
0.88); transform: perspective(1400px) rotateX(4deg) rotateY(-7deg) rotateZ(1.5deg); } Also include CSS for: -
.window-bar - .dashboard-layout - .dashboard-sidebar - .dashboard-main - .dashboard-grid - .panel - .task-row -
.timeline-card - .bars - .info-slide - .info-panel - .feature-card - .demo-slide - .demo-frame - .demo-video -
.deck-controls - .icon-button - .slide-dot - responsive breakpoints at 980px and 700px Match these layout behaviors: -
Desktop nav: brand left, slide links centered, CTA right. - Mobile nav: hide centered slide links, keep brand and CTA. -
Hero CTA must remain clickable and visually above the hero mockup. - Hero mockup should sit below the CTA with clear
spacing. - Deck controls stay fixed at bottom center. - Demo video is framed in a large rounded rectangle with subtle
shadow. - Inactive slides should not receive interaction. - No scrollbars. For theme variance: - Keep the structure,
slide navigation, interaction model, and component names the same. - Only change CSS tokens, fonts, background
gradients, accent colors, and shadow intensity. - Do not replace the slide system with a different architecture. - Do
not turn the experience into a vertical landing page. When finished, summarize: - Which files were created. - How to
edit slides. - How to change the theme. - How to add or replace the demo video.
```
