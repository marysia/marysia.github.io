---
layout: default
title: Notes
permalink: /notes/
footer_tagline: 'Notes are for the long stuff. Quick things live in [TIL](/til/).'
---

<p class="page-kicker">Course notes</p>
<h1 class="page-title">Notes</h1>
<p class="page-lede">Writing and summarizing helps me process and remember. Here, I collect longer write-ups, that are created mostly as I work through a course.</p>

{%- assign courses = site.notes | group_by: "course_id" | sort: "name" | reverse -%}
{%- if courses.size > 0 -%}
<div class="course-tabs" role="tablist">
  {%- for c in courses -%}
    {%- assign label = c.items | first -%}
    <button type="button" class="course-tab" role="tab"
            aria-selected="{% if forloop.first %}true{% else %}false{% endif %}"
            data-course="{{ c.name }}">{{ label.course }}</button>
  {%- endfor -%}
</div>

{%- for c in courses -%}
  {%- assign open_modules = "" -%}
  {%- if c.name == "eval" -%}
    {%- assign open_modules = "09,10" -%}
  {%- endif -%}
  {%- assign meta = "" -%}
  {%- if c.name == "eval" -%}
    {%- assign meta = "International Programme on AI Evaluation · ongoing" -%}
  {%- elsif c.name == "aises" -%}
    {%- assign meta = "Center for AI Safety · Summer 2025 cohort" -%}
  {%- endif -%}
  {%- include notes-course.html course_id=c.name first=forloop.first open_modules=open_modules course_meta=meta -%}
{%- endfor -%}

<script>
  const tabs = [...document.querySelectorAll('.course-tab')];
  const panels = [...document.querySelectorAll('.course-panel')];
  tabs.forEach(t => t.addEventListener('click', () => {
    tabs.forEach(x => x.setAttribute('aria-selected', x === t));
    panels.forEach(p => p.hidden = p.dataset.course !== t.dataset.course);
  }));
</script>
{%- endif -%}
