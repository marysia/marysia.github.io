---
layout: default
title: TIL
permalink: /til/
footer_tagline: 'Short by design. If it grows, it becomes a [note](/notes/).'
---

<p class="page-kicker">Today I learned</p>
<h1 class="page-title">TIL</h1>
<p class="page-lede">Writing things down helps me actually remember them, so I figured I’d share! <br> This page is basically where I capture quick summaries or takeaways from talks, papers, or courses without the formality of a full blog post.
</p>


<div class="til-feed">
{%- assign items = site.tils | where_exp: "t", "t.published != false" | sort: "date" | reverse -%}
{%- for t in items -%}
  <details class="til-acc">
    <summary class="til-sum">
      <span class="til-sum-main">
        <span class="tt">{{ t.title | escape }}</span>
        <span class="td">{{ t.date | date: "%-d %b %Y" }}</span>
        {%- if t.tag -%}<span class="tag">{{ t.tag | escape }}</span>{%- endif -%}
      </span>
      <span class="chev" aria-hidden="true"></span>
    </summary>
    <div class="til-body">
      {{ t.content }}
    </div>
  </details>
{%- endfor -%}
</div>

<script>
  const tilItems = [...document.querySelectorAll('.til-acc')];
  tilItems.forEach(item => {
    item.addEventListener('toggle', () => {
      if (!item.open) return;
      tilItems.forEach(other => {
        if (other !== item) other.open = false;
      });
    });
  });
</script>
