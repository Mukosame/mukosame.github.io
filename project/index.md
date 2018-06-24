---
layout: home
---

<div class="index-content project">
    <div class="section">
        <ul class="artical-cate">
            <li><a href="/"><span>技术</span></a></li>
            <li style="text-align:center"><a href="/dump"><span>随笔</span></a></li>
            <li class="on" style="text-align:right"><a href="/project"><span>关于我</span></a></li>
        </ul>

        <ul class="artical-list">
        {% for post in site.categories.project limit:1 %}
            <li>
                <p>
                {{post.content}}
                </p>
            </li>
        {% endfor %}
        </ul>
    </div>
    <div class="aside">
    </div>
</div>
