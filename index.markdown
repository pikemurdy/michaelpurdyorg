---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---



<h1 class="intro">Hello. <span class="muted">I am Michael Purdy and this is my website.</span></h1>

<p>Actually, I am kind of &ldquo;in between&rdquo; websites right now. I took a bit of a hiatus that ended up much more permanent than I would have thought. So I&rsquo;ve stripped it down, sort of like putting the garden to bed for winter.</p>
<p>The most recent thing I&rsquo;ve written and published here is {% for post in site.posts offset: 0 limit: 1  %}<a href="{{ post.url }}"><em>{{ post.title }}</em></a>{% endfor %}. If you keep reading down this page, you&rsquo;ll see a short list of other recent posts. You can always find a complete list of the posts on the <a href="/archives/">blog page</a> and a few more details about me on <a href="/about/">the about page</a>.</p>

<section id="begin">
  <div id="home-page-recent">
    <h3 class="latest">Latest Posts</h3>
    <ol class="archive">
        {% for post in site.posts offset: 0 limit: 6  %}
          <li class="archive-item">
            <a href="{{ post.url }}"><span>{{ post.title }}</span></a>
         {% include excerpt.html %}
        {% comment %}
         {% include postcats.html %}
        {% endcomment %}
         <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-alarm-fill" viewBox="0 0 16 16">
  <path d="M6 .5a.5.5 0 0 1 .5-.5h3a.5.5 0 0 1 0 1H9v1.07a7.001 7.001 0 0 1 3.274 12.474l.601.602a.5.5 0 0 1-.707.708l-.746-.746A6.97 6.97 0 0 1 8 16a6.97 6.97 0 0 1-3.422-.892l-.746.746a.5.5 0 0 1-.707-.708l.602-.602A7.001 7.001 0 0 1 7 2.07V1h-.5A.5.5 0 0 1 6 .5zm2.5 5a.5.5 0 0 0-1 0v3.362l-1.429 2.38a.5.5 0 1 0 .858.515l1.5-2.5A.5.5 0 0 0 8.5 9V5.5zM.86 5.387A2.5 2.5 0 1 1 4.387 1.86 8.035 8.035 0 0 0 .86 5.387zM11.613 1.86a2.5 2.5 0 1 1 3.527 3.527 8.035 8.035 0 0 0-3.527-3.527z"/>
</svg><time datetime="{{ post.date | date: "%Y-%m-%d" }}">{{ post.date | date_to_string }}</time>
          </li>
        {% endfor %}
    </ol>
  </div>
</section>
