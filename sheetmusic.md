---
layout: page
title: Sheet Music
permalink: /sheetmusic/
nav_order: 4
---

<style>
*,*::before,*::after{box-sizing:border-box}
:root{
  --bg:#ffffff; --panel:#f8f9fa; --border:#ddd; --text:#222; --muted:#555; --accent:#d6336c;
  --shadow:0 1px 2px rgba(0,0,0,.06),0 8px 24px rgba(0,0,0,.06);
  --shadow-hover:0 2px 6px rgba(0,0,0,.08),0 12px 32px rgba(0,0,0,.12);
}
.page-content{background:var(--bg);padding-top:1.25rem}
.sheet-title{ text-align:center; color:var(--text); margin:1rem 0 1.25rem; font-size:clamp(1.6rem,2vw+1rem,2.25rem); font-weight:800 }

/* Filters */
.filters{ display:flex; gap:.5rem; justify-content:center; flex-wrap:wrap; margin:.25rem auto 1.5rem; padding:0 .75rem; max-width:980px }
.filter-btn{ background:#f0f0f0; border:1px solid var(--border); color:var(--muted); padding:.55rem .9rem; border-radius:.55rem; cursor:pointer; font-size:.95rem; line-height:1; transition:transform .12s, box-shadow .12s, filter .12s }
.filter-btn.active{ background:#e9ecef; color:var(--text); border-color:#ccc }
.filter-btn:focus-visible{ outline:2px solid var(--accent); outline-offset:2px }
.filter-btn:hover{ filter:brightness(.97); transform:translateY(-1px) }

/* Grid (always 2 columns on tablet/desktop; grows in rows as items increase) */
.grid{ display:grid; grid-template-columns:1fr; gap:1rem; max-width:980px; margin:0 auto 2rem; padding:0 1rem }
@media (min-width:560px){ .grid{ grid-template-columns:repeat(2,minmax(0,1fr)); gap:1.25rem } }

/* Card */
.card{ background:var(--panel); border:1px solid var(--border); border-radius:.8rem; padding:1rem; box-shadow:var(--shadow); transition:transform .16s, box-shadow .16s, border-color .16s; display:flex; flex-direction:column }
.card:hover{ transform:translateY(-2px); box-shadow:var(--shadow-hover); border-color:rgba(0,0,0,.08) }

/* Thumb */
.thumb{ background:#fff; border:1px solid var(--border); border-radius:.55rem; display:flex; align-items:center; justify-content:center; overflow:hidden; aspect-ratio:16/10 }
.thumb img{ width:100%; height:100%; object-fit:contain; display:block }

/* Text */
.title{ color:var(--text); font-weight:800; margin:.75rem 0 .4rem; line-height:1.3; font-size:clamp(1rem,.4vw+.9rem,1.1rem); min-height:2.6em }
.meta{ color:var(--muted); font-size:.9rem; margin-bottom:.6rem }

/* Actions */
.actions{ display:flex; gap:.6rem; margin-top:auto; flex-wrap:wrap }
.btn{ flex:1 1 180px; text-align:center; padding:.7rem .8rem; border-radius:.55rem; font-weight:700; font-size:.95rem; border:1px solid var(--border); background:#f1f3f5; color:var(--text); text-decoration:none; transition: filter .12s, transform .12s, box-shadow .12s }
.btn.accent{ border-color:var(--accent); background:#fff0f6; color:#a61e4d }
.btn:hover{ filter:brightness(.97); transform:translateY(-1px) }
.btn:focus-visible{ outline:2px solid var(--accent); outline-offset:2px }

/* Phones: stack buttons */
@media (max-width:420px){ .actions{ flex-direction:column } .btn{ width:100% } }

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
  {%- assign sheets = site.posts | where_exp: "p", "p.categories contains 'sheetmusic'" -%}
  {%- comment -%}
    Each sheetmusic post should have (in its front matter):
      title: "No.1 – Dance for MIDI"
      categories: [sheetmusic]
      tags: [Composed]   # or [Transcription]
      thumb: /assets/scores/dance-for-midi.png
      pdf:   /assets/scores/dance-for-midi.pdf
      youtube: https://youtube.com/...
  {%- endcomment -%}

  {%- for post in sheets -%}
  <article class="card" data-tags="{{ post.tags | join: ', ' }}">
    <div class="thumb">
      <img loading="lazy"
           src="{{ post.thumb | default: '/assets/scores/placeholder.png' | relative_url }}"
           alt="{{ post.title | escape }}">
    </div>
    <div class="title">{{ post.title }}</div>
    <div class="actions">
      {%- if post.pdf -%}
        <a class="btn accent" href="{{ post.pdf | relative_url }}" download>Download PDF</a>
      {%- endif -%}
      {%- if post.youtube -%}
        <a class="btn" href="{{ post.youtube }}" target="_blank" rel="noopener">Watch on YouTube</a>
      {%- else -%}
        <a class="btn" href="{{ post.url | relative_url }}">Open Post</a>
      {%- endif -%}
    </div>
  </article>
  {%- endfor -%}
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
