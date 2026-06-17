---
layout: page
title: About
permalink: /about/
weight: 3
---

# **About Me**

Hi! I am **{{ site.author.name }}** :wave:,<br>
I am an Electrical Engineering student at the University of Calgary's Schulich School of Engineering. I enjoy taking projects from a blank schematic to a working prototype, whether that means writing and debugging modules of code or tracking down a stubborn PCB noise issue with an oscilloscope in hand. I'm equally comfortable supporting a team through planning and documentation, or jumping into hands-on debugging myself. Outside of engineering, I spend my time bouldering, playing badminton, and staying active in the gym.

<div class="row">
{% include about/skills.html title="Programming Skills" source=site.data.programming-skills %}
{% include about/skills.html title="Other Skills" source=site.data.other-skills %}
</div>

<div class="row">
{% include about/timeline.html %}
</div>
