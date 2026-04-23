```{=html}
<%
const recent = items.filter(n => n.recent === true || n.recent === 'true');
const older = items.filter(n => n.recent === false || n.recent === 'false');

const iconMap = {
  paper: 'bi-file-earmark-text',
  service: 'bi-people',
  honors: 'bi-trophy',
  position: 'bi-briefcase',
  talk: 'bi-mic'
};

const labelMap = {
  paper: 'paper',
  service: 'service',
  honors: 'honors',
  position: 'position',
  talk: 'talk'
};
%>

<div class="news-timeline">
<% for (const n of recent) {
  const icon = iconMap[n.type] || 'bi-circle';
  const label = labelMap[n.type] || '';
%>
  <div class="news-entry" data-type="<%= n.type || '' %>">
    <div class="news-dot"><i class="bi <%= icon %>"></i></div>
    <div class="news-body">
      <span class="news-date"><%= n.period %>.</span>
      <% if (label) { %><span class="news-type-badge news-type--<%= n.type %>"><%= label %></span><% } %>
      <span class="news-text"><%= n.text %></span>
    </div>
  </div>
<% } %>
</div>

<% if (older.length > 0) { %>
<details class="older-news">
  <summary>Older News</summary>
  <div class="news-timeline">
  <% for (const n of older) {
    const icon = iconMap[n.type] || 'bi-circle';
    const label = labelMap[n.type] || '';
  %>
    <div class="news-entry" data-type="<%= n.type || '' %>">
      <div class="news-dot"><i class="bi <%= icon %>"></i></div>
      <div class="news-body">
        <span class="news-date"><%= n.period %>.</span>
        <% if (label) { %><span class="news-type-badge news-type--<%= n.type %>"><%= label %></span><% } %>
        <span class="news-text"><%= n.text %></span>
      </div>
    </div>
  <% } %>
  </div>
</details>
<% } %>
```
