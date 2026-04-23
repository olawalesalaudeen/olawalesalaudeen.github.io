```{=html}
<%
function talkKind(t) {
  const k = t.kind != null ? t.kind : t.type;
  return String(k || '').toLowerCase().trim();
}
const invited = items.filter(t => talkKind(t) === 'invited');
const contributed = items.filter(t => talkKind(t) === 'contributed');
%>

<div class="talk-timeline">

<% if (invited.length) { %>
<div class="talk-section">
  <h2 class="talk-section-title"><span class="talk-type-badge talk-type--invited">Invited</span> Talks</h2>
  <div class="talk-year-entries">
  <% for (const t of invited) { %>
    <div class="talk-entry">
      <div class="talk-dot"></div>
      <div class="talk-content">
        <div class="talk-header">
          <div class="talk-title"><%= t.title %></div>
          <% if (t.note && talkKind(t) === 'invited') { %><span class="talk-note"><%= t.note %></span><% } %>
        </div>
        <div class="talk-venues">
          <% if (t.venues) { for (const v of t.venues) { %>
          <div class="talk-venue-row">
            <% if (v.link) { %><a href="<%= v.link %>" class="talk-venue-link" target="_blank" rel="noopener"><%= v.event %></a><% } else { %><span class="talk-venue-name"><%= v.event %></span><% } %><% if (v.date && !String(v.event).includes(v.date)) { %><span class="talk-venue-date">, <%= v.date %></span><% } %>
          </div>
          <% } } %>
        </div>
      </div>
    </div>
  <% } %>
  </div>
</div>
<% } %>

<% if (contributed.length) { %>
<div class="talk-section">
  <h2 class="talk-section-title"><span class="talk-type-badge talk-type--contributed">Contributed</span> Talks</h2>
  <div class="talk-year-entries">
  <% for (const t of contributed) { %>
    <div class="talk-entry">
      <div class="talk-dot"></div>
      <div class="talk-content">
        <div class="talk-header">
          <div class="talk-title"><%= t.title %></div>
          <% if (t.note && talkKind(t) === 'invited') { %><span class="talk-note"><%= t.note %></span><% } %>
        </div>
        <div class="talk-venues">
          <% if (t.venues) { for (const v of t.venues) { %>
          <div class="talk-venue-row">
            <% if (v.link) { %><a href="<%= v.link %>" class="talk-venue-link" target="_blank" rel="noopener"><%= v.event %></a><% } else { %><span class="talk-venue-name"><%= v.event %></span><% } %><% if (v.date && !String(v.event).includes(v.date)) { %><span class="talk-venue-date">, <%= v.date %></span><% } %>
          </div>
          <% } } %>
        </div>
      </div>
    </div>
  <% } %>
  </div>
</div>
<% } %>

</div>
```
