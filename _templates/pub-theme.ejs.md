```{=html}
<%
const tag = (typeof templateParams !== 'undefined' && templateParams && templateParams.tag) ? templateParams.tag : '';
const filtered = items.filter(p => {
  if (!p.tags) return false;
  if (Array.isArray(p.tags)) return p.tags.includes(tag);
  return String(p.tags).split(',').map(t => t.trim()).includes(tag);
});
filtered.sort((a, b) => (b.year || 0) - (a.year || 0));
let bibIdx = 0;
%>

<div class="theme-pubs">
<% for (const p of filtered) { bibIdx++; %>
  <div class="theme-pub-entry">
    <div class="pub-title"><%= p.title %></div>
    <div class="pub-authors"><%= p.authors %></div>
    <div class="pub-venue">
      <%= p.venue %><% if (p.note) { %> <span class="pub-note">(<%= p.note %>)</span><% } %>
    </div>
    <div class="pub-links">
    <% if (p.links && typeof p.links === 'object') {
      const linkEntries = Object.entries(p.links).filter(([k, v]) => v && v !== '#');
      for (const [label, url] of linkEntries) { %>
        <a href="<%= url %>" class="pub-link" target="_blank" rel="noopener"><%= label %></a>
    <% } } %>
    <% if (p.bibtex) { %>
      <button class="pub-link bib-toggle" onclick="document.getElementById('bib-theme-<%= tag %>-<%= bibIdx %>').classList.toggle('bib-visible')" aria-label="Toggle BibTeX">bib</button>
    <% } %>
    </div>
    <% if (p.bibtex) { %>
    <div class="bib-dropdown" id="bib-theme-<%= tag %>-<%= bibIdx %>">
      <pre class="bib-content"><%= p.bibtex.trim() %></pre>
    </div>
    <% } %>
  </div>
<% } %>
</div>
```
