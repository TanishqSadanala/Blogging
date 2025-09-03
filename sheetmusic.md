---
layout: page
title: Sheet Music
permalink: /sheetmusic/
nav_order: 4
---

<style>
:root{
  --bg:#ffffff; --panel:#f8f9fa; --border:#ddd; --text:#222; --muted:#555; --accent:#d6336c;
  --shadow: 0 1px 2px rgba(0,0,0,.06), 0 8px 24px rgba(0,0,0,.06);
  --shadow-hover: 0 2px 6px rgba(0,0,0,.08), 0 12px 32px rgba(0,0,0,.12);
}
@media (prefers-color-scheme: dark){
  :root{
    --bg:#0f1115; --panel:#161a22; --border:#2a2f3a; --text:#e6e6e6; --muted:#a6adbb; --accent:#ff4d8d;
    --shadow: 0 1px 2px rgba(0,0,0,.4), 0 8px 24px rgba(0,0,0,.25);
    --shadow-hover: 0 2px 8px rgba(0,0,0,.5), 0 14px 40px rgba(0,0,0,.35);
  }
}
.page-content{background:var(--bg);padding-top:1.25rem}

.sheet-title{ text-align:center; color:var(--text); margin: 1rem 0 1.25rem; font-size: clamp(1.6rem, 2vw + 1rem, 2.25rem); font-weight:700 }

/* Filters */
.filters{
  display:flex; gap:.5rem; justify-content:center; margin:.25rem auto 1.5rem; flex-wrap:wrap; padding:0 .75rem; max-width:980px
}
.filter-btn{
  background:#f0f0f0; border:1px solid var(--border); color:var(--muted);
  padding:.5rem .85rem; border-radius:.6rem; cursor:pointer; font-size:.95rem; line-height:1; transition:transform .12s ease, box-shadow .12s ease, filter .12s ease
}
.filter-btn.active{ background:#e9ecef; color:var(--text); border-color:#ccc }
@media (prefers-color-scheme: dark){
  .filter-btn{ background:#202532; color:var(--muted) }
  .filter-btn.active{ background:#262c3a; border-color:#364055 }
}
.filter-btn:focus-visible{ outline:2px solid var(--accent); outline-offset:2px }
.filter-btn:hover{ filter:brightness(.97) }
@media (hover:hover){
  .filter-btn:hover{ transform: translateY(-1px) }
}

/* Grid */
.grid{
  display:grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap:1rem;
  max-width:1100px;
  margin:0 auto 2rem;
  padding:0 1rem;
}
@media (min-width: 980px){
  .grid{ gap:1.25rem }
}

/* Card */
.card{
  background:var(--panel);
  border:1px solid var(--border);
  border-radius: .8rem;
  padding:1rem;
  box-shadow: var(--shadow);
  transition: transform .16s ease, box-shadow .16s ease, border-color .16s ease;
  display:flex; flex-direction:column
}
.card:hover{ transform: translateY(-2px); box-shadow: var(--shadow-hover); border-color: rgba(0,0,0,.08) }
@media (prefers-reduced-motion: reduce){
  .card, .filter-btn{ transition:none }
}

/* Thumb */
.thumb{
  background:#fff;
  border:1px solid var(--border);
  border-radius:.55rem;
  display:flex; align-items:center; justify-content:center;
  overflow:hidden;
  aspect-ratio: 16/10;
}
@media (prefers-color-scheme: dark){ .thumb{ background:#0e1015 } }
.thumb img{ max-width:100%; max-height:100%; display:block; width:auto; height:auto }

/* Text */
.title{
  color:var(--text); font-weight:800; margin:.75rem 0 .4rem; line-height:1.3;
  font-size: clamp(1rem, .4vw + .9rem, 1.1rem);
  min-height:2.6em;
}
.meta{ color:var(--muted); font-size:.9rem; margin-bottom:.6rem }

/* Actions */
.actions{
  display:flex; gap:.6rem; margin-top:auto; flex-wrap:wrap
}
.btn{
  flex:1 1 180px; text-align:center; padding:.65rem .75rem; border-radius:.55rem; font-weight:700; font-size:.95rem;
  border:1px solid var(--border); background:#f1f3f5; color:var(--text); text-decoration:none; transition: filter .12s ease, transform .12s ease, box-shadow .12s ease
}
.btn.accent{ border-color:var(--accent); background:color-mix(in srgb, var(--accent) 8%, transparent); color:var(--accent) }
@supports not (color-mix(in srgb, white 10%, black)){
  .btn.accent{ background:#fff0f6 }
}
.btn:hover{ filter:brightness(.97) }
@media (hover:hover){
  .btn:hover{ transform: translateY(-1px) }
}
.btn:focus-visible{ outline:2px solid var(--accent); outline-offset:2px }

/* Narrow screens: stack buttons full width */
@media (max-width: 520px){
  .actions{ flex-direction:column }
  .btn{ flex:1 1 auto; width:100% }
}

/* Utility */
.hidden{ display:none !important }
</style>

<h1 class="sheet-title">Sheet Music</h1>

<div class="filters" role="tablist" aria-label="Filter scores">
  <button type="button" class="filter-btn active" data-filter="all" role="tab" aria-selected="true">All</button>
  <button type="button" class="filter-btn" data-filter="Composed" role="tab" aria-selected="false">Composed</button>
  <button type="button" class="filter-btn" data-filter="Transcription" role="tab" aria-selected="false">Transcription</button>
</div>

<div class="grid" id="sheetGrid">

  <article class="card" data-tags="Composed">
    <div class="thumb">
      <img loading="lazy" src="{{ '/assets/scores/dance-for-midi.png' | relative_url }}" alt="No.1 - Dance for MIDI">
    </div>
    <div class="title">No.1 – Dance for MIDI</div>
    <div class="actions">
      <a class="btn accent" href="{{ '/assets/scores/dance-for-midi.pdf' | relative_url }}" download>Download PDF</a>
      <a class="btn" href="https://youtube.com/" target="_blank" rel="noopener">Watch on YouTube</a>
    </div>
  </article>

  <!-- Duplicate the <article> block per score; set data-tags="Composed" or "Transcription" or comma-separated like "Composed, Solo" -->
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
      buttons.forEach(b=>{ b.classList.remove('active'); b.setAttribute('aria-selected','false'); });
      btn.classList.add('active'); btn.setAttribute('aria-selected','true');
      applyFilter(btn.dataset.filter);
    });
    btn.addEventListener('keydown', e=>{
      if(e.key === 'Enter' || e.key === ' '){ e.preventDefault(); btn.click(); }
    });
  });

  applyFilter('all');
})();
</script>
