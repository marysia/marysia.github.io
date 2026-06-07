---
layout: default
title: CV
permalink: /cv/
footer_tagline: 'Download the PDF for the full version with publications and details.'
---

<p class="page-kicker">Curriculum vitae</p>
<h1 class="page-title">CV</h1>
<p class="page-lede">A short version of my professional career.</p>

<div class="cv-dl">
  <a href="{{ site.data.cv.pdf_url | relative_url }}">Download full CV (PDF)</a>
  <a href="{{ site.data.cv.linkedin_url }}">LinkedIn</a>
</div>

{%- for section in site.data.cv.sections -%}
  {%- include cv-section.html section=section -%}
{%- endfor -%}
