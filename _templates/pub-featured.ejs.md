```{=html}
<%
const featured = items.filter(p => p.featured);
featured.sort((a, b) => (b.year || 0) - (a.year || 0));
%>

<div class="featured-work">
<% for (const p of featured) { %>
  <div class="featured-entry">
    <div class="featured-title"><%= p.title %></div>
    <div class="featured-authors"><%= p.authors %></div>
    <div class="featured-venue">
      <%= p.venue %><% if (p.note) { %> <span class="pub-note">(<%= p.note %>)</span><% } %>
    </div>
    <% if (p.abstract) { %>
    <div class="featured-abstract"><%= p.abstract %></div>
    <% } %>
    <% if (p.links && typeof p.links === 'object') {
      const linkEntries = Object.entries(p.links).filter(([k, v]) => v && v !== '#');
      if (linkEntries.length > 0) { %>
    <div class="pub-links">
      <% for (const [label, url] of linkEntries) { %>
        <a href="<%= url %>" class="pub-link" target="_blank" rel="noopener"><%= label %></a>
      <% } %>
    </div>
    <% } } %>
  </div>
<% } %>
</div>
```
