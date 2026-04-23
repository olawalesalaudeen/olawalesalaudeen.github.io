```{=html}
<%
const preprints = items.filter(p => p.status === 'preprint');
const published = items.filter(p => p.status !== 'preprint');

preprints.sort((a, b) => (b.year || 0) - (a.year || 0));

const byYear = {};
for (const item of published) {
  const y = item.year || 'Other';
  if (!byYear[y]) byYear[y] = [];
  byYear[y].push(item);
}
const years = Object.keys(byYear).sort((a, b) => b - a);
let bibIdx = 0;

const tagLabels = {
  measurement: 'Measurement',
  generalization: 'Generalization',
  intervention: 'Intervention',
  applications: 'Applications'
};

function tagsAttr(p) {
  return Array.isArray(p.tags) ? p.tags.join(' ') : '';
}
%>

<div class="pub-filter" role="group" aria-label="Filter papers by area">
  <button type="button" class="pub-filter-chip is-active" data-filter="all">All</button>
  <button type="button" class="pub-filter-chip" data-filter="measurement">Measurement</button>
  <button type="button" class="pub-filter-chip" data-filter="generalization">Generalization</button>
  <button type="button" class="pub-filter-chip" data-filter="intervention">Intervention</button>
  <button type="button" class="pub-filter-chip" data-filter="applications">Applications</button>
</div>

<div class="pub-list">

<% if (preprints.length > 0) { %>
  <h2 class="pub-year">Preprints</h2>
  <% for (const p of preprints) { bibIdx++; %>
  <div class="pub-entry<%= p.featured ? ' pub-featured' : '' %>" id="<%= p.id %>" data-tags="<%= tagsAttr(p) %>">
    <div class="pub-title"><%= p.title %></div>
    <div class="pub-authors"><%= p.authors %></div>
    <div class="pub-venue">
      <%= p.venue %><% if (p.note) { %>
        <% const noteLower = p.note.toLowerCase(); const isAward = !noteLower.startsWith('preliminary') && !noteLower.startsWith('extended') && !noteLower.startsWith('abstract') && !noteLower.includes('review'); %>
        <span class="<%= isAward ? 'pub-note' : 'pub-note-info' %>">(<%= p.note %>)</span>
      <% } %><% if (p.venue_suffix) { %> <%= p.venue_suffix %><% } %>
    </div>
    <% if (p.tags && p.tags.length > 0) { %><div class="pub-tags"><% for (const t of p.tags) { %><span class="pub-tag pub-tag--<%= t %>" data-tag="<%= t %>"><%= tagLabels[t] || t %></span><% } %></div><% } %>
    <div class="pub-links">
    <% if (p.links && typeof p.links === 'object') {
      const linkEntries = Object.entries(p.links).filter(([k, v]) => v && v !== '#');
      for (const [label, url] of linkEntries) { %>
        <a href="<%= url %>" class="pub-link" target="_blank" rel="noopener"><%= label %></a>
    <% } } %>
    <% if (p.bibtex) { %>
      <button class="pub-link bib-toggle" onclick="document.getElementById('bib-<%= bibIdx %>').classList.toggle('bib-visible')" aria-label="Toggle BibTeX">bib</button>
    <% } %>
    </div>
    <% if (p.bibtex) { %>
    <div class="bib-dropdown" id="bib-<%= bibIdx %>">
      <pre class="bib-content"><%= p.bibtex.trim() %></pre>
    </div>
    <% } %>
  </div>
  <% } %>
<% } %>

<% for (const year of years) { %>
  <h2 class="pub-year" data-year="<%= year %>"><%= year %></h2>
  <% for (const p of byYear[year]) { bibIdx++; %>
  <div class="pub-entry<%= p.featured ? ' pub-featured' : '' %>" id="<%= p.id %>" data-tags="<%= tagsAttr(p) %>">
    <div class="pub-title"><%= p.title %></div>
    <div class="pub-authors"><%= p.authors %></div>
    <div class="pub-venue">
      <%= p.venue %><% if (p.note) { %>
        <% const noteLower = p.note.toLowerCase(); const isAward = !noteLower.startsWith('preliminary') && !noteLower.startsWith('extended') && !noteLower.startsWith('abstract') && !noteLower.includes('review'); %>
        <span class="<%= isAward ? 'pub-note' : 'pub-note-info' %>">(<%= p.note %>)</span>
      <% } %><% if (p.venue_suffix) { %> <%= p.venue_suffix %><% } %>
    </div>
    <% if (p.tags && p.tags.length > 0) { %><div class="pub-tags"><% for (const t of p.tags) { %><span class="pub-tag pub-tag--<%= t %>" data-tag="<%= t %>"><%= tagLabels[t] || t %></span><% } %></div><% } %>
    <div class="pub-links">
    <% if (p.links && typeof p.links === 'object') {
      const linkEntries = Object.entries(p.links).filter(([k, v]) => v && v !== '#');
      for (const [label, url] of linkEntries) { %>
        <a href="<%= url %>" class="pub-link" target="_blank" rel="noopener"><%= label %></a>
    <% } } %>
    <% if (p.bibtex) { %>
      <button class="pub-link bib-toggle" onclick="document.getElementById('bib-<%= bibIdx %>').classList.toggle('bib-visible')" aria-label="Toggle BibTeX">bib</button>
    <% } %>
    </div>
    <% if (p.bibtex) { %>
    <div class="bib-dropdown" id="bib-<%= bibIdx %>">
      <pre class="bib-content"><%= p.bibtex.trim() %></pre>
    </div>
    <% } %>
  </div>
  <% } %>
<% } %>

</div>

<script>
(function() {
  const filterBar = document.querySelector('.pub-filter');
  const list = document.querySelector('.pub-list');
  if (!filterBar || !list) return;

  const chips = filterBar.querySelectorAll('.pub-filter-chip');
  const entries = list.querySelectorAll('.pub-entry');
  const yearHeaders = list.querySelectorAll('.pub-year');

  function applyFilter(filter) {
    entries.forEach(e => {
      const tags = (e.getAttribute('data-tags') || '').split(/\s+/).filter(Boolean);
      const show = filter === 'all' || tags.includes(filter);
      e.classList.toggle('pub-hidden', !show);
    });
    yearHeaders.forEach(h => {
      let next = h.nextElementSibling;
      let hasVisible = false;
      while (next && !next.classList.contains('pub-year')) {
        if (next.classList.contains('pub-entry') && !next.classList.contains('pub-hidden')) {
          hasVisible = true; break;
        }
        next = next.nextElementSibling;
      }
      h.classList.toggle('pub-hidden', !hasVisible);
    });
  }

  chips.forEach(chip => {
    chip.addEventListener('click', () => {
      chips.forEach(c => c.classList.remove('is-active'));
      chip.classList.add('is-active');
      applyFilter(chip.getAttribute('data-filter'));
    });
  });

  list.querySelectorAll('.pub-tag').forEach(tag => {
    tag.addEventListener('click', () => {
      const t = tag.getAttribute('data-tag');
      const target = filterBar.querySelector('.pub-filter-chip[data-filter="' + t + '"]');
      if (target) target.click();
    });
  });
})();
</script>
```
