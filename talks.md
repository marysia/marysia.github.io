---
layout: default
title: Talks
permalink: /talks/
menu: true
footer_tagline: 'Want me to speak? [Get in touch.](mailto:hello@marysia.nl)'
---

<p class="page-kicker">Conferences &amp; public speaking</p>
<h1 class="page-title">Talks</h1>
<p class="page-lede">Conferences I've helped run, talks I've given, and competitions I've won.</p>

<div class="talks-sections">
{%- assign org = site.data.talks.organisation -%}
{%- capture org_count -%}{{ org.size }} editions{%- endcapture -%}
{%- include talks-section.html heading="Conference & event organisation" count=org_count open=true items=org note=site.data.talks.organisation_note -%}

{%- assign comps = site.data.talks.competitions -%}
{%- include talks-section.html heading="Competitions" count="selection" items=comps -%}

{%- assign talks_list = site.data.talks.talks -%}
{%- capture talks_count -%}selection · {{ talks_list.size }}{%- endcapture -%}
{%- include talks-section.html heading="Talks" count=talks_count items=talks_list -%}
<!-- 
{%- assign research = site.data.talks.research -%}
{%- include talks-section.html heading="Research" count="selection" items=research -%}

{%- assign trainings = site.data.talks.trainings -%}
{%- include talks-section.html heading="Trainings" count="selection" items=trainings -%} -->
</div>

<div class="talks-extra">
  <img src="{{ '/assets/misc/ballpit.gif' | relative_url }}" alt="PyData Amsterdam committee in the conference ballpit, 2019">
  <p class="tg-note">PyData Amsterdam committee at the 2019 conference in the conference ballpit!</p>
</div>
