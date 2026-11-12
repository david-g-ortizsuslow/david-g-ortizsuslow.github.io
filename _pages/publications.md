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
  /* ---- Filter chips ---- */
  .pub-key { display: flex; flex-wrap: wrap; gap: .5rem .75rem; align-items: center; margin: 0 0 1.5rem; }
  .pub-chip { display: inline-block; padding: .2rem .8rem; border: 0; border-radius: 999px; color: #fff; font: inherit; font-size: .85rem; line-height: 1.5; cursor: pointer; opacity: .55; transition: opacity .15s; }
  .pub-chip:hover, .pub-chip:focus-visible { opacity: .85; }
  .pub-chip[aria-pressed="true"] { opacity: 1; outline: 2px solid var(--global-text-color, #000); outline-offset: 2px; }
  .pub-chip[disabled] { opacity: .3; cursor: default; }
  .pub-count { opacity: .85; margin-left: .25rem; }
  .pub-status { font-size: .85rem; opacity: .75; }

  /* ---- Colors: one per publication type ---- */
  .chip-all { background: transparent; color: var(--global-text-color, #000); box-shadow: inset 0 0 0 1px currentColor; }
  .chip-journal, [data-pubtype="journal"] .abbr abbr, [data-pubtype="journal"] .abbr .badge { background-color: #0F766E !important; color: #fff !important; } /* teal   */
  .chip-conf,    [data-pubtype="conference"] .abbr abbr, [data-pubtype="conference"] .abbr .badge { background-color: #6D28D9 !important; color: #fff !important; } /* violet */
  .chip-report,  [data-pubtype="report"] .abbr abbr, [data-pubtype="report"] .abbr .badge { background-color: #B45309 !important; color: #fff !important; } /* amber  */
  .chip-edit,    [data-pubtype="editorial"] .abbr abbr, [data-pubtype="editorial"] .abbr .badge { background-color: #BE185D !important; color: #fff !important; } /* rose   */
  .chip-pending, [data-pubtype="pending"] .abbr abbr, [data-pubtype="pending"] .abbr .badge { background-color: #6B7280 !important; color: #fff !important; } /* grey   */

  /* ---- Hidden by the filter (also hides empty year headings) ---- */
  .pub-hidden { display: none !important; }
</style>

<!-- Bibsearch Feature -->

{% include bib_search.liquid %}

<div class="pub-key" role="group" aria-label="Filter publications by type">
  <strong>Filter:</strong>
  <button type="button" class="pub-chip chip-all" data-filter="all" aria-pressed="true">All <span class="pub-count"></span></button>
  <button type="button" class="pub-chip chip-journal" data-filter="journal" aria-pressed="false">Peer-reviewed <span class="pub-count"></span></button>
  <button type="button" class="pub-chip chip-conf" data-filter="conference" aria-pressed="false">Conference papers <span class="pub-count"></span></button>
  <button type="button" class="pub-chip chip-report" data-filter="report" aria-pressed="false">Reports <span class="pub-count"></span></button>
  <button type="button" class="pub-chip chip-edit" data-filter="editorial" aria-pressed="false">Editorial <span class="pub-count"></span></button>
  <button type="button" class="pub-chip chip-pending" data-filter="pending" aria-pressed="false">Developing <span class="pub-count"></span></button>
  <span class="pub-status" aria-live="polite"></span>
</div>

<div class="publications">

{% bibliography %}

</div>

<script>
(function () {
  /* Each entry's type comes from the "category" field of the .bib file, written into the page
     by _includes/hook/bib.liquid. Entries with a missing or unrecognized category stay visible
     under "All" only, and are listed in the browser console as a warning. */
  var TYPES = ['journal', 'conference', 'report', 'editorial', 'pending'];
  function init() {
    var key = document.querySelector('.pub-key');
    var root = document.querySelector('.publications');
    if (!key || !root) return;
    var entries = [];
    var problems = [];
    root.querySelectorAll('.bibliography > li').forEach(function (li) {
      var marker = li.querySelector('.pub-type');
      var raw = marker ? (marker.getAttribute('data-pubtype') || '').trim().toLowerCase() : '';
      var cat = TYPES.indexOf(raw) !== -1 ? raw : 'other';
      if (cat === 'other') {
        var idEl = li.querySelector('[id]');
        problems.push((idEl ? idEl.id : '(unknown entry)') + (raw ? ' [category=' + raw + ']' : ' [no category]'));
      } else {
        li.setAttribute('data-pubtype', cat);
      }
      entries.push({ el: li, cat: cat });
    });
    if (!entries.length) { console.warn('pub-filter: no publication entries found'); return; }
    if (problems.length) console.warn('pub-filter: entries without a valid category (shown under All only): ' + problems.join(', '));
    var buttons = Array.prototype.slice.call(key.querySelectorAll('button[data-filter]'));
    var status = key.querySelector('.pub-status');
    var current = 'all';
    function count(f) { return f === 'all' ? entries.length : entries.filter(function (e) { return e.cat === f; }).length; }
    buttons.forEach(function (b) {
      var n = count(b.getAttribute('data-filter'));
      b.querySelector('.pub-count').textContent = '(' + n + ')';
      if (n === 0) b.disabled = true;
    });
    function apply(f) {
      current = f;
      entries.forEach(function (e) { e.el.classList.toggle('pub-hidden', f !== 'all' && e.cat !== f); });
      var lists = [];
      entries.forEach(function (e) { if (lists.indexOf(e.el.parentElement) === -1) lists.push(e.el.parentElement); });
      lists.forEach(function (ol) {
        var any = Array.prototype.some.call(ol.children, function (ch) { return !ch.classList.contains('pub-hidden'); });
        ol.classList.toggle('pub-hidden', !any);
        var h = ol.previousElementSibling;
        if (h && /^H[1-6]$/.test(h.tagName)) h.classList.toggle('pub-hidden', !any);
      });
      buttons.forEach(function (b) { b.setAttribute('aria-pressed', String(b.getAttribute('data-filter') === f)); });
      status.textContent = f === 'all' ? '' : 'Showing ' + count(f) + ' of ' + entries.length;
      /* A ?type=... query string (not a #hash: the theme's search box reads the hash as a search term). */
      if (window.history && history.replaceState) {
        history.replaceState(null, '', location.pathname + (f === 'all' ? '' : '?type=' + f) + location.hash);
      }
    }
    buttons.forEach(function (b) {
      b.addEventListener('click', function () {
        var f = b.getAttribute('data-filter');
        apply(f === current && f !== 'all' ? 'all' : f);
      });
    });
    var start = new URLSearchParams(location.search).get('type');
    if (start && TYPES.indexOf(start) !== -1 && count(start) > 0) apply(start);
  }
  if (document.readyState === 'loading') document.addEventListener('DOMContentLoaded', init); else init();
})();
</script>
