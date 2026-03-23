<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Build Log | Projects & Experiments</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <!-- Using Inter for body text as a clean companion to Astron -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
  
  <style>
    /* 
       INSTRUCTIONS FOR ASTRON FONT:
       Place your 'Astron.woff2' or 'Astron.ttf' file in the same folder as this HTML file.
       If the font file is elsewhere, update the url() path below.
    */
    @font-face {
      font-family: 'Astron';
      src: url('Astron.woff2') format('woff2'),
           url('Astron.ttf') format('truetype');
      font-weight: normal;
      font-style: normal;
      font-display: swap;
    }

    :root {
      --bg: #F7F7F2;
      --bg-alt: #EFEFEA;
      --fg: #1A1A1A;
      --muted: #6B6B6B;
      --accent: #00FF88;
      --accent-dim: #00CC6A;
      --card: #FFFFFF;
      --border: #E0E0DC;
    }

    * {
      box-sizing: border-box;
    }

    html {
      scroll-behavior: smooth;
    }

    body {
      font-family: 'Inter', sans-serif;
      background-color: var(--bg);
      color: var(--fg);
      line-height: 1.6;
    }

    /* Apply Astron to headings and displays */
    h1, h2, h3, h4, .font-display {
      font-family: 'Astron', sans-serif;
      letter-spacing: 0.02em;
    }

    .font-mono {
      font-family: 'JetBrains Mono', monospace;
    }

    /* Timeline styles */
    .timeline-line {
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
      width: 2px;
      height: 100%;
      background: linear-gradient(to bottom, transparent, var(--border) 5%, var(--border) 95%, transparent);
    }

    .timeline-node {
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
      width: 12px;
      height: 12px;
      background: var(--bg);
      border: 2px solid var(--accent);
      border-radius: 50%;
      z-index: 10;
    }

    .timeline-node.active {
      background: var(--accent);
      box-shadow: 0 0 20px var(--accent);
    }

    /* Card hover effects */
    .project-card {
      transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1), 
                  box-shadow 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .project-card:hover {
      transform: translateY(-4px);
      box-shadow: 0 20px 40px rgba(0, 0, 0, 0.08);
    }

    /* Status badges */
    .status-live {
      background: var(--accent);
      color: var(--fg);
    }

    .status-building {
      background: #FFE566;
      color: var(--fg);
    }

    .status-sunset {
      background: #E0E0DC;
      color: var(--muted);
    }

    /* Reveal animations */
    .reveal {
      opacity: 0;
      transform: translateY(30px);
      transition: opacity 0.6s cubic-bezier(0.4, 0, 0.2, 1),
                  transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .reveal.visible {
      opacity: 1;
      transform: translateY(0);
    }

    .reveal-left {
      opacity: 0;
      transform: translateX(-40px);
      transition: opacity 0.6s cubic-bezier(0.4, 0, 0.2, 1),
                  transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .reveal-left.visible {
      opacity: 1;
      transform: translateX(0);
    }

    .reveal-right {
      opacity: 0;
      transform: translateX(40px);
      transition: opacity 0.6s cubic-bezier(0.4, 0, 0.2, 1),
                  transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .reveal-right.visible {
      opacity: 1;
      transform: translateX(0);
    }

    /* Button styles */
    .btn-primary {
      background: var(--accent);
      color: var(--fg);
      transition: all 0.2s ease;
    }

    .btn-primary:hover {
      background: var(--accent-dim);
      box-shadow: 0 0 20px rgba(0, 255, 136, 0.4);
    }

    .btn-secondary {
      background: transparent;
      border: 2px solid var(--fg);
      color: var(--fg);
      transition: all 0.2s ease;
    }

    .btn-secondary:hover {
      background: var(--fg);
      color: var(--bg);
    }

    /* Metric counter animation */
    .metric-value {
      transition: transform 0.3s ease;
    }

    .metric-card:hover .metric-value {
      transform: scale(1.05);
    }

    /* Floating decoration */
    @keyframes float {
      0%, 100% { transform: translateY(0) rotate(0deg); }
      50% { transform: translateY(-20px) rotate(5deg); }
    }

    .float-element {
      animation: float 6s ease-in-out infinite;
    }

    @keyframes pulse-glow {
      0%, 100% { opacity: 0.5; }
      50% { opacity: 1; }
    }

    .glow-pulse {
      animation: pulse-glow 2s ease-in-out infinite;
    }

    /* Nav link underline */
    .nav-link {
      position: relative;
    }

    .nav-link::after {
      content: '';
      position: absolute;
      bottom: -2px;
      left: 0;
      width: 0;
      height: 2px;
      background: var(--accent);
      transition: width 0.3s ease;
    }

    .nav-link:hover::after,
    .nav-link.active::after {
      width: 100%;
    }

    /* Changelog item */
    .changelog-item {
      border-left: 2px solid var(--border);
      transition: border-color 0.3s ease;
    }

    .changelog-item:hover {
      border-color: var(--accent);
    }

    /* Tag styles */
    .tag {
      transition: all 0.2s ease;
    }

    .tag:hover {
      background: var(--accent);
      color: var(--fg);
    }

    /* Focus styles */
    a:focus-visible,
    button:focus-visible,
    input:focus-visible {
      outline: 2px solid var(--accent);
      outline-offset: 2px;
    }

    /* Reduced motion */
    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
      }
    }

    /* Mobile timeline adjustment */
    @media (max-width: 768px) {
      .timeline-line {
        left: 20px;
      }
      .timeline-node {
        left: 20px;
      }
    }

    /* Scrollbar */
    ::-webkit-scrollbar {
      width: 8px;
    }

    ::-webkit-scrollbar-track {
      background: var(--bg);
    }

    ::-webkit-scrollbar-thumb {
      background: var(--border);
      border-radius: 4px;
    }

    ::-webkit-scrollbar-thumb:hover {
      background: var(--muted);
    }
  </style>
</head>
<body class="min-h-screen">
  <!-- Navigation -->
  <nav class="fixed top-0 left-0 right-0 z-50 bg-[var(--bg)]/90 backdrop-blur-md border-b border-[var(--border)]">
    <div class="max-w-6xl mx-auto px-6 py-4 flex items-center justify-between">
      <a href="#" class="flex items-center gap-3 group" aria-label="Build Log Home">
        <div class="w-10 h-10 bg-[var(--accent)] rounded flex items-center justify-center group-hover:shadow-[0_0_20px_rgba(0,255,136,0.5)] transition-shadow">
          <svg class="w-6 h-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
            <path d="M12 2L2 7l10 5 10-5-10-5z"/>
            <path d="M2 17l10 5 10-5"/>
            <path d="M2 12l10 5 10-5"/>
          </svg>
        </div>
        <span class="font-display font-bold text-xl tracking-wide">BUILD LOG</span>
      </a>
      
      <div class="hidden md:flex items-center gap-8">
        <a href="#projects" class="nav-link text-[var(--muted)] hover:text-[var(--fg)] transition-colors text-sm font-medium">Projects</a>
        <a href="#updates" class="nav-link text-[var(--muted)] hover:text-[var(--fg)] transition-colors text-sm font-medium">Weekly Updates</a>
        <a href="#metrics" class="nav-link text-[var(--muted)] hover:text-[var(--fg)] transition-colors text-sm font-medium">Metrics</a>
      </div>

      <div class="flex items-center gap-3">
        <a href="https://github.com" target="_blank" rel="noopener" class="p-2 hover:bg-[var(--bg-alt)] rounded-lg transition-colors" aria-label="GitHub">
          <svg class="w-5 h-5" viewBox="0 0 24 24" fill="currentColor">
            <path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z"/>
          </svg>
        </a>
        <button id="menuBtn" class="md:hidden p-2 hover:bg-[var(--bg-alt)] rounded-lg transition-colors" aria-label="Menu">
          <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M3 12h18M3 6h18M3 18h18"/>
          </svg>
        </button>
      </div>
    </div>

    <!-- Mobile menu -->
    <div id="mobileMenu" class="hidden md:hidden border-t border-[var(--border)] bg-[var(--bg)]">
      <div class="px-6 py-4 flex flex-col gap-4">
        <a href="#projects" class="text-[var(--muted)] hover:text-[var(--fg)] transition-colors">Projects</a>
        <a href="#updates" class="text-[var(--muted)] hover:text-[var(--fg)] transition-colors">Weekly Updates</a>
        <a href="#metrics" class="text-[var(--muted)] hover:text-[var(--fg)] transition-colors">Metrics</a>
      </div>
    </div>
  </nav>

  <!-- Hero Section -->
  <header class="pt-32 pb-20 px-6 relative overflow-hidden">
    <!-- Decorative elements -->
    <div class="absolute top-20 right-10 w-64 h-64 bg-[var(--accent)]/10 rounded-full blur-3xl float-element"></div>
    <div class="absolute bottom-10 left-10 w-48 h-48 bg-[var(--accent)]/5 rounded-full blur-2xl float-element" style="animation-delay: -3s;"></div>
    
    <div class="max-w-4xl mx-auto text-center relative z-10">
      <div class="inline-flex items-center gap-2 px-3 py-1 bg-[var(--bg-alt)] border border-[var(--border)] rounded-full text-sm text-[var(--muted)] mb-6 reveal">
        <span class="w-2 h-2 bg-[var(--accent)] rounded-full glow-pulse"></span>
        Currently building in public
      </div>
      
      <h1 class="text-4xl md:text-6xl lg:text-7xl font-bold mb-6 reveal font-display tracking-wide" style="transition-delay: 0.1s;">
        Documenting the<br>
        <span class="relative inline-block">
          Journey
          <svg class="absolute -bottom-2 left-0 w-full" viewBox="0 0 200 8" fill="none">
            <path d="M0 4C50 0 150 8 200 4" stroke="var(--accent)" stroke-width="3" stroke-linecap="round"/>
          </svg>
        </span>
      </h1>
      
      <p class="text-lg md:text-xl text-[var(--muted)] max-w-2xl mx-auto mb-10 reveal" style="transition-delay: 0.2s;">
        Projects, experiments, and lessons learned while building products in public. 
        Follow along as I ship, fail, learn, and iterate.
      </p>
      
      <div class="flex flex-col sm:flex-row items-center justify-center gap-4 reveal" style="transition-delay: 0.3s;">
        <button id="subscribeBtn" class="btn-primary px-6 py-3 rounded-lg font-medium flex items-center gap-2">
          <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
            <polyline points="22,6 12,13 2,6"/>
          </svg>
          Subscribe to Updates
        </button>
        <a href="#projects" class="btn-secondary px-6 py-3 rounded-lg font-medium">
          View Projects
        </a>
      </div>
    </div>
  </header>

  <!-- Metrics Panel -->
  <section id="metrics" class="py-16 px-6 bg-[var(--bg-alt)]">
    <div class="max-w-5xl mx-auto">
      <div class="flex items-center gap-2 mb-8 reveal">
        <svg class="w-5 h-5 text-[var(--accent)]" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M18 20V10M12 20V4M6 20v-6"/>
        </svg>
        <span class="text-sm font-mono text-[var(--muted)]">OPEN METRICS</span>
      </div>
      
      <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
        <div class="metric-card bg-[var(--card)] p-6 rounded-xl border border-[var(--border)] reveal" style="transition-delay: 0.1s;">
          <p class="text-sm text-[var(--muted)] mb-1 font-mono">Visitors</p>
          <p class="text-3xl md:text-4xl font-bold metric-value font-display" data-target="12847">0</p>
          <p class="text-xs text-[var(--accent)] mt-2">+12% this month</p>
        </div>
        
        <div class="metric-card bg-[var(--card)] p-6 rounded-xl border border-[var(--border)] reveal" style="transition-delay: 0.2s;">
          <p class="text-sm text-[var(--muted)] mb-1 font-mono">Subscribers</p>
          <p class="text-3xl md:text-4xl font-bold metric-value font-display" data-target="892">0</p>
          <p class="text-xs text-[var(--accent)] mt-2">+47 this week</p>
        </div>
        
        <div class="metric-card bg-[var(--card)] p-6 rounded-xl border border-[var(--border)] reveal" style="transition-delay: 0.3s;">
          <p class="text-sm text-[var(--muted)] mb-1 font-mono">Projects Shipped</p>
          <p class="text-3xl md:text-4xl font-bold metric-value font-display" data-target="23">0</p>
          <p class="text-xs text-[var(--muted)] mt-2">3 in progress</p>
        </div>
        
        <div class="metric-card bg-[var(--card)] p-6 rounded-xl border border-[var(--border)] reveal" style="transition-delay: 0.4s;">
          <p class="text-sm text-[var(--muted)] mb-1 font-mono">Coffee Consumed</p>
          <p class="text-3xl md:text-4xl font-bold metric-value font-display" data-target="847">0</p>
          <p class="text-xs text-[var(--muted)] mt-2">cups and counting</p>
        </div>
      </div>
    </div>
  </section>

  <!-- Projects Timeline -->
  <section id="projects" class="py-20 px-6">
    <div class="max-w-5xl mx-auto">
      <div class="text-center mb-16">
        <p class="text-sm font-mono text-[var(--accent)] mb-2 reveal">PROJECTS</p>
        <h2 class="text-3xl md:text-4xl font-bold reveal font-display" style="transition-delay: 0.1s;">What I've Been Building</h2>
      </div>
      
      <div class="relative">
        <div class="timeline-line hidden md:block"></div>
        
        <div id="projectsContainer" class="space-y-12">
          <!-- Projects will be rendered here -->
        </div>
      </div>
    </div>
  </section>

  <!-- Weekly Updates -->
  <section id="updates" class="py-20 px-6 bg-[var(--bg-alt)]">
    <div class="max-w-4xl mx-auto">
      <div class="flex items-center justify-between mb-12">
        <div>
          <p class="text-sm font-mono text-[var(--accent)] mb-2 reveal">CHANGELOG</p>
          <h2 class="text-3xl md:text-4xl font-bold reveal font-display" style="transition-delay: 0.1s;">Weekly Updates</h2>
        </div>
        <a href="#" class="text-sm text-[var(--muted)] hover:text-[var(--fg)] transition-colors flex items-center gap-1 reveal" style="transition-delay: 0.2s;">
          View all
          <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M5 12h14M12 5l7 7-7 7"/>
          </svg>
        </a>
      </div>
      
      <div id="updatesContainer" class="space-y-4">
        <!-- Updates will be rendered here -->
      </div>
    </div>
  </section>

  <!-- Newsletter CTA -->
  <section class="py-20 px-6">
    <div class="max-w-3xl mx-auto">
      <div class="bg-[var(--fg)] text-[var(--bg)] rounded-2xl p-8 md:p-12 relative overflow-hidden">
        <div class="absolute top-0 right-0 w-64 h-64 bg-[var(--accent)]/20 rounded-full blur-3xl"></div>
        
        <div class="relative z-10">
          <h2 class="text-2xl md:text-3xl font-bold mb-4 reveal font-display">Stay in the Loop</h2>
          <p class="text-[var(--bg)]/70 mb-8 max-w-lg reveal" style="transition-delay: 0.1s;">
            Get weekly updates on what I'm building, breaking, and learning. No spam, just maker content.
          </p>
          
          <form id="newsletterForm" class="flex flex-col sm:flex-row gap-3 reveal" style="transition-delay: 0.2s;">
            <input 
              type="email" 
              placeholder="you@example.com" 
              required
              class="flex-1 px-4 py-3 bg-[var(--bg)]/10 border border-[var(--bg)]/20 rounded-lg text-[var(--bg)] placeholder:text-[var(--bg)]/50 focus:border-[var(--accent)] transition-colors"
            >
            <button type="submit" class="btn-primary px-6 py-3 rounded-lg font-medium whitespace-nowrap">
              Subscribe
            </button>
          </form>
          
          <p class="text-xs text-[var(--bg)]/50 mt-4 reveal" style="transition-delay: 0.3s;">
            Join 892 builders. Unsubscribe anytime.
          </p>
        </div>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="py-12 px-6 border-t border-[var(--border)]">
    <div class="max-w-5xl mx-auto">
      <div class="flex flex-col md:flex-row items-center justify-between gap-6">
        <div class="flex items-center gap-6">
          <div class="flex items-center gap-2">
            <div class="w-6 h-6 bg-[var(--accent)] rounded flex items-center justify-center">
              <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
                <path d="M12 2L2 7l10 5 10-5-10-5z"/>
                <path d="M2 17l10 5 10-5"/>
                <path d="M2 12l10 5 10-5"/>
              </svg>
            </div>
            <span class="font-display font-medium tracking-wide">BUILD LOG</span>
          </div>
          
          <div class="flex items-center gap-4">
            <a href="https://github.com" target="_blank" rel="noopener" class="text-[var(--muted)] hover:text-[var(--fg)] transition-colors" aria-label="GitHub">
              <svg class="w-5 h-5" viewBox="0 0 24 24" fill="currentColor">
                <path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z"/>
              </svg>
            </a>
            <a href="https://twitter.com" target="_blank" rel="noopener" class="text-[var(--muted)] hover:text-[var(--fg)] transition-colors" aria-label="Twitter">
              <svg class="w-5 h-5" viewBox="0 0 24 24" fill="currentColor">
                <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
              </svg>
            </a>
            <a href="mailto:hello@buildlog.dev" class="text-[var(--muted)] hover:text-[var(--fg)] transition-colors" aria-label="Email">
              <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                <polyline points="22,6 12,13 2,6"/>
              </svg>
            </a>
          </div>
        </div>
        
        <div class="flex items-center gap-6 text-sm text-[var(--muted)]">
          <span>MIT License</span>
          <span>Made with intention</span>
        </div>
      </div>
    </div>
  </footer>

  <!-- Project Modal -->
  <div id="projectModal" class="fixed inset-0 z-50 hidden" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
    <div class="absolute inset-0 bg-[var(--fg)]/50 backdrop-blur-sm" id="modalBackdrop"></div>
    <div class="absolute inset-4 md:inset-10 lg:inset-20 bg-[var(--card)] rounded-2xl overflow-hidden flex flex-col">
      <div class="flex items-center justify-between p-4 md:p-6 border-b border-[var(--border)]">
        <h3 id="modalTitle" class="text-xl md:text-2xl font-bold font-display"></h3>
        <button id="closeModal" class="p-2 hover:bg-[var(--bg-alt)] rounded-lg transition-colors" aria-label="Close modal">
          <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M18 6L6 18M6 6l12 12"/>
          </svg>
        </button>
      </div>
      <div id="modalContent" class="flex-1 overflow-y-auto p-4 md:p-6">
        <!-- Content will be injected here -->
      </div>
    </div>
  </div>

  <!-- Toast Notification -->
  <div id="toast" class="fixed bottom-6 right-6 bg-[var(--fg)] text-[var(--bg)] px-6 py-4 rounded-lg shadow-lg transform translate-y-20 opacity-0 transition-all duration-300 z-50">
    <p id="toastMessage">Subscribed successfully!</p>
  </div>

  <script>
    // Data
    const projects = [
      {
        id: 1,
        title: "ShipFast Clone",
        description: "A boilerplate for launching SaaS products quickly. Authentication, payments, and email all configured.",
        longDescription: "Built over 3 weekends, this project helped me understand the full stack of a modern SaaS. Includes user authentication with magic links, Stripe integration for subscriptions, and a complete admin dashboard.",
        status: "live",
        date: "2024-01",
        stack: ["Next.js", "Stripe", "PostgreSQL", "Tailwind"],
        image: "linear-gradient(135deg, #667eea 0%, #764ba2 100%)",
        metrics: { users: 234, revenue: "$1.2k/mo" },
        lessons: ["Start with the hardest problem first", "Documentation is a feature", "Ship early, iterate often"]
      },
      {
        id: 2,
        title: "AI Writing Assistant",
        description: "GPT-powered tool for generating marketing copy, blog posts, and social content.",
        longDescription: "An experiment in building with LLMs. Features include tone adjustment, content repurposing, and a prompt library for common use cases.",
        status: "building",
        date: "2024-02",
        stack: ["Python", "FastAPI", "OpenAI", "React"],
        image: "linear-gradient(135deg, #00FF88 0%, #00CC6A 100%)",
        metrics: { users: 89, requests: "12k/mo" },
        lessons: ["Prompt engineering is an art", "Rate limiting is essential", "Users want control over output"]
      },
      {
        id: 3,
        title: "Focus Timer",
        description: "Minimalist Pomodoro timer with analytics and session tracking.",
        longDescription: "A simple tool born out of my own need for better focus. Tracks productive hours, break patterns, and generates weekly reports.",
        status: "live",
        date: "2023-11",
        stack: ["Vue.js", "Supabase", "Chart.js"],
        image: "linear-gradient(135deg, #f093fb 0%, #f5576c 100%)",
        metrics: { users: 1205, sessions: "45k total" },
        lessons: ["Simple solutions often win", "Mobile-first matters", "Notifications need careful tuning"]
      },
      {
        id: 4,
        title: "Code Snippet Manager",
        description: "Personal library for storing and organizing frequently used code snippets.",
        longDescription: "Tired of searching through old projects for that one useful function, I built this tool. Supports syntax highlighting, tagging, and quick search.",
        status: "sunset",
        date: "2023-08",
        stack: ["React", "IndexedDB", "Monaco Editor"],
        image: "linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)",
        metrics: { users: 67, snippets: "2.3k saved" },
        lessons: ["Know when to sunset", "Local-first has tradeoffs", "Sync is harder than it looks"]
      },
      {
        id: 5,
        title: "RSS Reader",
        description: "Clean, fast RSS reader with keyboard navigation and offline support.",
        longDescription: "A return to the open web. Aggregates feeds, provides a distraction-free reading experience, and caches articles for offline access.",
        status: "building",
        date: "2024-03",
        stack: ["SvelteKit", "SQLite", "Service Worker"],
        image: "linear-gradient(135deg, #fa709a 0%, #fee140 100%)",
        metrics: { feeds: 45, articles: "1.2k/mo" },
        lessons: ["In progress..."]
      }
    ];

    const weeklyUpdates = [
      {
        week: "Week 12, 2024",
        date: "Mar 18-24",
        changes: [
          { type: "feature", text: "Added dark mode toggle to Focus Timer" },
          { type: "fix", text: "Fixed authentication flow on mobile devices" },
          { type: "progress", text: "RSS Reader now supports OPML import" },
          { type: "metric", text: "Crossed 1,200 users on Focus Timer" }
        ]
      },
      {
        week: "Week 11, 2024",
        date: "Mar 11-17",
        changes: [
          { type: "feature", text: "Launched email digest for Build Log" },
          { type: "fix", text: "Resolved database connection pooling issues" },
          { type: "progress", text: "Started refactoring AI Writing Assistant backend" },
          { type: "learning", text: "Deep dive into rate limiting strategies" }
        ]
      },
      {
        week: "Week 10, 2024",
        date: "Mar 4-10",
        changes: [
          { type: "ship", text: "Released ShipFast Clone v1.0" },
          { type: "feature", text: "Added webhook testing to SaaS boilerplate" },
          { type: "fix", text: "Fixed Stripe webhook handling edge cases" },
          { type: "metric", text: "First paying customer on ShipFast Clone" }
        ]
      }
    ];

    // DOM Elements
    const projectsContainer = document.getElementById('projectsContainer');
    const updatesContainer = document.getElementById('updatesContainer');
    const projectModal = document.getElementById('projectModal');
    const modalBackdrop = document.getElementById('modalBackdrop');
    const modalTitle = document.getElementById('modalTitle');
    const modalContent = document.getElementById('modalContent');
    const closeModalBtn = document.getElementById('closeModal');
    const menuBtn = document.getElementById('menuBtn');
    const mobileMenu = document.getElementById('mobileMenu');
    const subscribeBtn = document.getElementById('subscribeBtn');
    const newsletterForm = document.getElementById('newsletterForm');
    const toast = document.getElementById('toast');
    const toastMessage = document.getElementById('toastMessage');

    // Render Projects Timeline
    function renderProjects() {
      projectsContainer.innerHTML = projects.map((project, index) => {
        const isEven = index % 2 === 0;
        const statusClass = {
          live: 'status-live',
          building: 'status-building',
          sunset: 'status-sunset'
        }[project.status];
        
        return `
          <div class="relative">
            <div class="timeline-node ${project.status === 'live' ? 'active' : ''}" style="top: 40px;"></div>
            
            <div class="md:grid md:grid-cols-2 md:gap-8">
              <div class="${isEven ? 'md:text-right md:pr-12' : 'md:col-start-2 md:pl-12'}">
                <div class="project-card bg-[var(--card)] rounded-xl border border-[var(--border)] overflow-hidden cursor-pointer reveal-${isEven ? 'left' : 'right'}" 
                     data-project-id="${project.id}"
                     style="transition-delay: ${index * 0.1}s;">
                  <div class="h-40 md:h-48" style="background: ${project.image};"></div>
                  <div class="p-6">
                    <div class="flex items-center gap-2 mb-3 ${isEven ? 'md:justify-end' : ''}">
                      <span class="${statusClass} px-2 py-1 text-xs font-mono uppercase rounded">${project.status}</span>
                      <span class="text-xs text-[var(--muted)] font-mono">${project.date}</span>
                    </div>
                    <h3 class="text-xl font-bold mb-2 font-display">${project.title}</h3>
                    <p class="text-[var(--muted)] text-sm mb-4">${project.description}</p>
                    <div class="flex flex-wrap gap-2 ${isEven ? 'md:justify-end' : ''}">
                      ${project.stack.map(tech => `<span class="tag px-2 py-1 text-xs font-mono bg-[var(--bg-alt)] rounded">${tech}</span>`).join('')}
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        `;
      }).join('');

      // Add click handlers
      document.querySelectorAll('.project-card').forEach(card => {
        card.addEventListener('click', () => {
          const projectId = parseInt(card.dataset.projectId);
          openProjectModal(projectId);
        });
      });
    }

    // Render Weekly Updates
    function renderUpdates() {
      updatesContainer.innerHTML = weeklyUpdates.map((update, index) => {
        return `
          <div class="changelog-item bg-[var(--card)] rounded-lg p-6 pl-8 reveal" style="transition-delay: ${index * 0.1}s;">
            <div class="flex items-center justify-between mb-4">
              <h3 class="font-bold font-display">${update.week}</h3>
              <span class="text-sm text-[var(--muted)]">${update.date}</span>
            </div>
            <div class="space-y-2">
              ${update.changes.map(change => {
                const iconMap = {
                  feature: '<svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="var(--accent)" stroke-width="2"><path d="M12 5v14M5 12h14"/></svg>',
                  fix: '<svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="#FFE566" stroke-width="2"><path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"/></svg>',
                  progress: '<svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="#4facfe" stroke-width="2"><path d="M12 2v4M12 18v4M4.93 4.93l2.83 2.83M16.24 16.24l2.83 2.83M2 12h4M18 12h4M4.93 19.07l2.83-2.83M16.24 7.76l2.83-2.83"/></svg>',
                  metric: '<svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="var(--accent)" stroke-width="2"><path d="M18 20V10M12 20V4M6 20v-6"/></svg>',
                  ship: '<svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="var(--accent)" stroke-width="2"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>',
                  learning: '<svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="#f093fb" stroke-width="2"><path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"/><path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"/></svg>'
                };
                return `
                  <div class="flex items-start gap-3 text-sm">
                    <span class="mt-0.5">${iconMap[change.type]}</span>
                    <span class="text-[var(--muted)]">${change.text}</span>
                  </div>
                `;
              }).join('')}
            </div>
          </div>
        `;
      }).join('');
    }

    // Open Project Modal
    function openProjectModal(projectId) {
      const project = projects.find(p => p.id === projectId);
      if (!project) return;

      modalTitle.textContent = project.title;
      modalContent.innerHTML = `
        <div class="grid md:grid-cols-2 gap-8">
          <div>
            <div class="h-48 md:h-64 rounded-lg mb-6" style="background: ${project.image};"></div>
            <div class="flex items-center gap-4 mb-4">
              <span class="${project.status === 'live' ? 'status-live' : project.status === 'building' ? 'status-building' : 'status-sunset'} px-3 py-1 text-sm font-mono uppercase rounded">${project.status}</span>
              <span class="text-[var(--muted)] font-mono">${project.date}</span>
            </div>
          </div>
          <div>
            <h4 class="font-bold mb-3 font-display">About</h4>
            <p class="text-[var(--muted)] mb-6">${project.longDescription}</p>
            
            <h4 class="font-bold mb-3 font-display">Tech Stack</h4>
            <div class="flex flex-wrap gap-2 mb-6">
              ${project.stack.map(tech => `<span class="tag px-3 py-1 text-sm font-mono bg-[var(--bg-alt)] rounded cursor-pointer">${tech}</span>`).join('')}
            </div>
            
            <h4 class="font-bold mb-3 font-display">Metrics</h4>
            <div class="grid grid-cols-2 gap-4 mb-6">
              ${Object.entries(project.metrics).map(([key, value]) => `
                <div class="bg-[var(--bg-alt)] p-4 rounded-lg">
                  <p class="text-xs text-[var(--muted)] font-mono uppercase mb-1">${key}</p>
                  <p class="text-xl font-bold font-display">${value}</p>
                </div>
              `).join('')}
            </div>
          </div>
        </div>
        
        <div class="mt-8 pt-8 border-t border-[var(--border)]">
          <h4 class="font-bold mb-4 font-display">Lessons Learned</h4>
          <ul class="space-y-2">
            ${project.lessons.map(lesson => `
              <li class="flex items-start gap-2 text-[var(--muted)]">
                <svg class="w-5 h-5 text-[var(--accent)] flex-shrink-0 mt-0.5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/>
                  <polyline points="22 4 12 14.01 9 11.01"/>
                </svg>
                ${lesson}
              </li>
            `).join('')}
          </ul>
        </div>
      `;

      projectModal.classList.remove('hidden');
      document.body.style.overflow = 'hidden';
    }

    // Close Modal
    function closeModal() {
      projectModal.classList.add('hidden');
      document.body.style.overflow = '';
    }

    // Show Toast
    function showToast(message) {
      toastMessage.textContent = message;
      toast.classList.remove('translate-y-20', 'opacity-0');
      
      setTimeout(() => {
        toast.classList.add('translate-y-20', 'opacity-0');
      }, 3000);
    }

    // Animate Metrics
    function animateMetrics() {
      const metricValues = document.querySelectorAll('.metric-value');
      
      metricValues.forEach(el => {
        const target = parseInt(el.dataset.target);
        const duration = 2000;
        const start = performance.now();
        
        function update(currentTime) {
          const elapsed = currentTime - start;
          const progress = Math.min(elapsed / duration, 1);
          const easeOut = 1 - Math.pow(1 - progress, 3);
          const current = Math.floor(target * easeOut);
          
          el.textContent = current.toLocaleString();
          
          if (progress < 1) {
            requestAnimationFrame(update);
          }
        }
        
        requestAnimationFrame(update);
      });
    }

    // Intersection Observer for reveal animations
    function setupRevealAnimations() {
      const observerOptions = {
        threshold: 0.1,
        rootMargin: '0px 0px -50px 0px'
      };

      const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            entry.target.classList.add('visible');
            
            // Trigger metric animation when metrics section is visible
            if (entry.target.closest('#metrics')) {
              animateMetrics();
            }
          }
        });
      }, observerOptions);

      document.querySelectorAll('.reveal, .reveal-left, .reveal-right').forEach(el => {
        observer.observe(el);
      });
    }

    // Event Listeners
    closeModalBtn.addEventListener('click', closeModal);
    modalBackdrop.addEventListener('click', closeModal);

    document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape' && !projectModal.classList.contains('hidden')) {
        closeModal();
      }
    });

    menuBtn.addEventListener('click', () => {
      mobileMenu.classList.toggle('hidden');
    });

    subscribeBtn.addEventListener('click', () => {
      document.querySelector('#newsletterForm input').focus();
      document.querySelector('#newsletterForm').scrollIntoView({ behavior: 'smooth' });
    });

    newsletterForm.addEventListener('submit', (e) => {
      e.preventDefault();
      const email = e.target.querySelector('input').value;
      showToast(`Thanks for subscribing with ${email}!`);
      e.target.reset();
    });

    // Mobile menu links close menu
    mobileMenu.querySelectorAll('a').forEach(link => {
      link.addEventListener('click', () => {
        mobileMenu.classList.add('hidden');
      });
    });

    // Initialize
    function init() {
      renderProjects();
      renderUpdates();
      setupRevealAnimations();
    }

    // Run after DOM is ready
    if (document.readyState === 'loading') {
      document.addEventListener('DOMContentLoaded', init);
    } else {
      init();
    }
  </script>
</body>
</html>
