---
layout: page
permalink: /publications/
title: publications
description: My published works.
nav: true
nav_order: 2
---

<!-- _pages/publications.md -->

<style>
  /* ---- Color key ---- */
  .pub-key { display: flex; flex-wrap: wrap; gap: .5rem 1rem; align-items: center; margin: 0 0 1.75rem; }
  .pub-key a { text-decoration: none; }
  .pub-chip { display: inline-block; padding: .15rem .75rem; border-radius: 999px; color: #fff; font-size: .85rem; line-height: 1.5; }

  /* ---- Colors: one per publication type ---- */
  .chip-journal, .pub-journal .abbr abbr, .pub-journal .abbr .badge { background-color: #0F766E !important; color: #fff !important; } /* teal   */
  .chip-conf,    .pub-conf    .abbr abbr, .pub-conf    .abbr .badge { background-color: #6D28D9 !important; color: #fff !important; } /* violet */
  .chip-report,  .pub-report  .abbr abbr, .pub-report  .abbr .badge { background-color: #B45309 !important; color: #fff !important; } /* amber  */
  .chip-edit,    .pub-edit    .abbr abbr, .pub-edit    .abbr .badge { background-color: #BE185D !important; color: #fff !important; } /* rose   */
  .chip-pending, .pub-pending .abbr abbr, .pub-pending .abbr .badge { background-color: #6B7280 !important; color: #fff !important; } /* grey   */
</style>

<!-- Bibsearch Feature -->

{% include bib_search.liquid %}

<div class="pub-key">
  <strong>Key:</strong>
  <a href="#journal"><span class="pub-chip chip-journal">Peer-reviewed journal</span></a>
  <a href="#conference"><span class="pub-chip chip-conf">Conference paper</span></a>
  <a href="#report"><span class="pub-chip chip-report">Technical report</span></a>
  <a href="#editorial"><span class="pub-chip chip-edit">Editorial / community</span></a>
  <a href="#pending"><span class="pub-chip chip-pending">In review / in preparation</span></a>
</div>

<h2 id="journal">Peer-reviewed journal articles</h2>
<div class="publications pub-journal">

{% bibliography --query @*[category=journal] %}

</div>

<h2 id="conference">Conference papers</h2>
<div class="publications pub-conf">

{% bibliography --query @*[category=conference] %}

</div>

<h2 id="report">Technical reports</h2>
<div class="publications pub-report">

{% bibliography --query @*[category=report] %}

</div>

<h2 id="editorial">Editorial and community pieces</h2>
<div class="publications pub-edit">

{% bibliography --query @*[category=editorial] %}

</div>

<h2 id="pending">In review and in preparation</h2>
<div class="publications pub-pending">

{% bibliography --query @*[category=pending] %}

</div>
