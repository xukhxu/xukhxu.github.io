---
layout: default
---

<style>
.toggle { cursor: pointer; margin: 6px 0; user-select: none; color: #159957; }
.hidden { display: none; }
.indent { margin-left: 20px; }
.arrow { display: inline-block; transition: transform 0.2s ease; margin-right: 5px; }
.arrow.open { transform: rotate(90deg); }

.year-title { font-size: 1.4em; font-weight: bold; color: #1e6bb8; } /* 年份设大一点，蓝色 */
.month-title { font-size: 1.1em; font-weight: bold; color: #606c71; } /* 月份设小一点，灰色 */
</style>

<script>
function toggle(id, arrowId) {
  var el = document.getElementById(id);
  var arrow = document.getElementById(arrowId);

  if (el.classList.contains("hidden")) {
    el.classList.remove("hidden");
    arrow.classList.add("open");
  } else {
    el.classList.add("hidden");
    arrow.classList.remove("open");
  }
}
</script>

{% assign current_year = site.time | date: "%Y" %}
{% assign current_month = site.time | date: "%Y-%m" %}

{% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}

{% for year in posts_by_year %}
{% assign year_id = "year-" | append: year.name %}
{% assign year_arrow = "arrow-year-" | append: year.name %}

<div class="toggle year-title" onclick="toggle('{{ year_id }}', '{{ year_arrow }}')">
  <span id="{{ year_arrow }}" class="arrow {% if year.name == current_year %}open{% endif %}">▶</span>
  {{ year.name }} 年
</div>

<div id="{{ year_id }}" class="indent {% unless year.name == current_year %}hidden{% endunless %}">

  {% assign posts_by_month = year.items | group_by_exp: "post", "post.date | date: '%Y-%m'" %}
  {% for month in posts_by_month %}
  {% assign month_id = "month-" | append: month.name %}
  {% assign month_arrow = "arrow-month-" | append: month.name %}

  <div class="toggle month-title" onclick="toggle('{{ month_id }}', '{{ month_arrow }}')">
    <span id="{{ month_arrow }}" class="arrow {% if month.name == current_month %}open{% endif %}">▶</span>
    {{ month.name | append: "-01" | date: "%-m 月" }}
  </div>

  <ul id="{{ month_id }}" class="indent {% unless month.name == current_month %}hidden{% endunless %}">
    {% for post in month.items %}
    <li>
      <a href="{{ post.url }}">
        {{ post.date | date: "%Y-%m-%d" }}：{{ post.title }}
      </a>
    </li>
    {% endfor %}
  </ul>

  {% endfor %}
</div>

{% endfor %}
