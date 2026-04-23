```{=html}
<%
const sorted = items.slice().sort((a, b) => (b.year || 0) - (a.year || 0));
%>

<div class="writing-list">
<% for (const w of sorted) { %>
  <div class="writing-entry">
    <div class="writing-title">
      <% if (w.link) { %><a href="<%= w.link %>" target="_blank" rel="noopener"><%= w.title %></a><% } else { %><%= w.title %><% } %>
    </div>
    <div class="writing-date"><%= w.date %></div>
    <% if (w.summary) { %>
    <div class="writing-summary"><%= w.summary %></div>
    <% } %>
  </div>
<% } %>
</div>
```
