---
layout: page
title: Sheet Music
permalink: /sheetmusic/
nav_order: 4
---

<style>
/* ---- Light theme ---- */
:root { --bg:#ffffff; --panel:#f8f9fa; --border:#ddd; --text:#222; --muted:#555; --accent:#d6336c; }
.page-content { background: var(--bg); padding-top: 1.25rem; }

/* ---- Heading ---- */
.sheet-title { text-align:center; color:var(--text); margin: 1rem 0 1.25rem; font-size: 2rem; font-weight: 700; }

/* ---- Filter chips ---- */
.filters { display:flex; gap:.5rem; justify-content:center; margin: .25rem 0 1.5rem; }
.filter-btn{ background:#f0f0f0;border:1px solid var(--border);color:var(--muted);padding:.35rem .75rem;border-radius:.4rem;cursor:pointer;font-size:.9rem }
.filter-btn.active{ background:#e9ecef;color:var(--text); border-color:#ccc }

/* ---- Grid ---- */
.grid{ display:grid; grid-template-columns: repeat(2,minmax(0,1fr)); gap:1rem; max-width:980px; margin:0 auto 2rem; padding:0 1rem; }
@media (min-width: 980px){ .grid{ grid-template-columns: repeat(3,minmax(0,1fr)); gap:1.25rem;} }
.card{ background:var(--panel); border:1px solid var(--border); border-radius:.6rem; padding:.9rem; }
.thumb{ background:#fff; border:1px solid var(--border); border-radius:.4rem; height:165px; display:flex; align-items:center; justify-content:center; overflow:hidden; }
.thumb img{ max-width:100%; max-height:100%; display:block; }
.title{ color:var(--text); font-weight:700; margin:.7rem 0 .35rem; line-height:1.3; min-height:2.6em; }
.meta{ color:var(--muted); font-size:.9rem; margin-bottom:.6rem; }

/* ---- Buttons ---- */
.actions{ display:flex; gap:.5rem; }
.btn{ flex:1 1 auto; text-align:center; padding:.5rem .6rem; border-radius:.4rem; font-weight:600; font-size:.9rem; border:1px solid var(--border); background:#f1f3f5; color:var(--text); text-decoration:none; }
.btn.accent{ border-color:#d6336c; background:#fff0f6; color:#a61e4d; }
.btn:hover{ filter: brightness(0.95); }

/* ---- Hide during filter ---- */
.hidden{ display:none !important; }
</style>

<div class="filters">
  <button class="filter-btn active" data-filter="all">All</button>
  <button class="filter-btn" data-filter="Composed">Composed</button>
  <button class="filter-btn" data-filter="Transcription">Transcription</button>
</div>

<div class="grid" id="sheetGrid">

  <!-- Example Card -->
  <article class="card" data-tags="Composed">
    <div class="thumb">
      <img src="{{ '/assets/scores/dance-for-midi.png' | relative_url }}" alt="No.1 - Dance for MIDI">
    </div>
    <div class="title">No.1 – Dance for MIDI</div>
    <div class="actions">
      <a class="btn accent" href="{{ '/assets/scores/dance-for-midi.pdf' | relative_url }}" download>Download PDF</a>
      <a class="btn" href="https://youtube.com/" target="_blank" rel="noopener">Watch on YouTube</a>
    </div>
  </article>

  <!-- Add more cards like above -->
</div>

<script>
(function(){
  const buttons = document.querySelectorAll('.filter-btn');
  const cards   = document.querySelectorAll('#sheetGrid .card');

  function applyFilter(tag){
    cards.forEach(c=>{
      const tags = (c.getAttribute('data-tags')||'').split(',').map(s=>s.trim());
      c.classList.toggle('hidden', !(tag==='all' || tags.includes(tag)));
    });
  }

  buttons.forEach(btn=>{
    btn.addEventListener('click', ()=>{
      buttons.forEach(b=>b.classList.remove('active'));
      btn.classList.add('active');
      applyFilter(btn.dataset.filter);
    });
  });

  // default “All”
  applyFilter('all');
})();
</script>
