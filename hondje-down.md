---
layout: standalone
title: Hondje Down?
permalink: /hondje-down
menu: false
# Use quoted "yes" or "no". Bare no/yes are YAML booleans and break the template.
answer: "yes"
---

<h1 style="text-align: center; margin-bottom: 0.35em;">Hondje down?</h1>

{% if page.answer == false %}
{% assign show_no = true %}
{% else %}
{% assign a = page.answer | strip | downcase %}
{% if a == 'no' %}
{% assign show_no = true %}
{% else %}
{% assign show_no = false %}
{% endif %}
{% endif %}

{% if show_no %}
<p style="text-align: center; font-size: clamp(3.5rem, 14vw, 9rem); font-weight: 700; margin: 0.15em 0 0.02em; line-height: 1; color: #c62828; letter-spacing: 0.06em;">NO</p>
<figure style="text-align: center; margin: 0; margin-top: 0;">
  <img src="../assets/hondje-down/hondje-down-no.JPG" alt="Hondje down? Nee." style="max-width: min(100%, 440px); height: auto; display: block; margin: 0 auto; border-radius: 8px;">
</figure>
{% else %}
<p style="text-align: center; font-size: clamp(3.5rem, 14vw, 9rem); font-weight: 700; margin: 0.15em 0 0.02em; line-height: 1; color: #2e7d32; letter-spacing: 0.06em;">YES</p>
<figure style="text-align: center; margin: 0; margin-top: 0;">
  <img src="../assets/hondje-down/hondje-down-yes.JPG" alt="Hondje down? Ja." style="max-width: min(100%, 440px); height: auto; display: block; margin: 0 auto; border-radius: 8px;">
</figure>
{% endif %}
