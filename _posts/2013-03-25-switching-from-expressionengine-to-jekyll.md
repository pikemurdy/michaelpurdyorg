---
layout: post
title: "Switching from ExpressionEngine to Jekyll"
categories:
- technology
- design
- web

date: 2013-03-25
summary: Now every article and post on this site is completely static and created with Jekyll. There is no CMS to maintain. No server requirements. No plugins to get working right. No caching. As simple as it gets. Here&rsquo;s the how and why.
---

Changing domains was a big deal for me. My reasons were mostly conceptual. After mulling it over for quite a while, I took the leap and moved from pikemurdy.com to the more approriate michaelpurdy.org. &ldquo;PikeMurdy&rdquo; always felt like an inside-joke. &ldquo;MichaelPurdy&rdquo; is immediately apparent.

But before I did that, I had to also consider what content management system (CMS) to use. Whenever any redesign happens or certainly any shift in content it&rsquo;s worth reconsidering what you&rsquo;re using. *Is there anything better*? *Is it worth the change*? *What do you need to do*?

Since this site has been a blog, it has been run by some kind of CMS. For about a month in 2004, I maintained a blog based upon my own CMS (bad idea). At quickly switched to [WordPress](http://wordpress.org), I dabbled with [Drupal](http://www.drupal.com) (not for long though) and for the past two years I&rsquo;ve run it with [ExpressionEngine](http://ellislab.com/expressionengine/).

But ultimately you must look at what the site is. And this is a simple site, with simple needs. There is one person who updates for it (me). There is one person who writes and edits for it (me). There is one person maintains it (me). And that person (me) is more comfortable writing in HTML in Coda than typing text into a control panel. That person (me) is no expert in databases, and simply getting a local copy of the site running on my own machine that mirrors that of the server is a wee bit more than I am really happy fiddling with. In addition to that, the architecture and structure of the site is very simple. In short, most of the advantages of a content management system are outweighed by the costs of managing a content management system.

*So I axed content management*.

I still think ExpressionEngine is tremendously good software, and I will happily recommend it to anyone who will listen. However, for the needs of this particular site, it is too much.

The site is now a collection of good old-fashioned HTML files. Of course, that is nuts on its own. Nearly impossible to practically maintain. So I am now using Jekyll to create the HTML files (written in Markdown) locally, and checking that everything is to my liking, before publishing to my server.

I&rsquo;m not a Ruby expert. And I&rsquo;m not a command-line wizard. So at first using Jekyll sounded scary. After I did one or two tests of templates and creating a file that I pretended to publish, my fears subsided. *It turned out to be easy*.

To set up Jekyll on my local machine, [I simply followed the instructions](https://github.com/mojombo/jekyll/wiki/Install).

When it came to building my own site, [I looked at how others were doing it](https://github.com/mojombo/jekyll/wiki/Sites). I also caught up on a few liquid template conventions. Having been a long time user of ExpressionEngine and WordPress, I was quick to adapt. The site took no time to build. To publish I use rsync, following [instructions](http://henrik.nyh.se/octopress/2009/04/jekyll/) that are all over the internet. Using Jekyll turned out to be a way less technical than I feared. And it is certainly well documented.

## Moving from ExpressionEngine to Jekyll

The most obvious hurdle in switching from a CMS to a collection text files, is getting the hundreds of posts out of a MySQL database and into a collection of files. The files themselves would need only to be tex files (to be saved as .md files) with a bit of [YAML](http://en.wikipedia.org/wiki/YAML) at the top.

### So here&rsquo;s how I did it

In my ExpressionEngine setup, I had three main channels for each content type: Collections (my channel for images), Blog (my channel for links elsewhere) and Articles (my channel for things I&rsquo;ve written).

For a Jekyll post, you must have a text (Markdown) file, with YAML at the top. So I wrote an ExpressionEngine template that would show all of my posts in this format.


	{exp:channel:entries channel="articles" limit="9999" rdf="off"}
	---
	layout: post
	title: "{title}"
	categories:
	 	{categories}- {category_name}
	{/categories}
	date: {entry_date format='%Y-%m-%d'}
	summary: {article_excerpt}
	---
	 
	{article_body}
	
	<!-- end of post -->
	{/exp:channel:entries}


Notice that I added the channel-name to the categories. This is how I separate the types of content on the Jekyll site.

After the template was created, all I needed to do is visit the URL of the template in a browser and save the source as a text file. I did this for each of the Channels. I could have used one template, sure, but each of my channels had different fields and field types. This was just quicker.

All that was left to do was to clean up a bit of formatting with some simple find and replace. ExpressionEngine and WordPress wysiwyg editors had both wreaked a bit of HTML havoc on my writing.

To separate each post into a separate text file I followed [Funkatron&rsquo;s](http://funkatron.com/posts/migrating-from-expressionengine-to-jekyll.html) example. I created a PHP file and it did the work: 


	<?php
		$file_str = file_get_contents('./ee_articles.txt');
		 
		$split = explode("<!-- end of post -->", $file_str);
		 
		foreach($split as $post_str) {
		 
		       $post_str = trim($post_str);
		 
		       if ($post_str) {
		               $post_pieces = explode('---', $post_str);
		               $yaml = trim($post_pieces[1]);
		               $yaml_lines = explode("\n", $yaml);
		               foreach ($yaml_lines as $line) {
		                       if (preg_match("/^date:\s?(\d\d\d\d-\d\d-\d\d)$/i", $line, $matches)) {
		                               print_r($matches);
		                               $date = $matches[1];
		                       }
		                       if (preg_match("/^url_title:\s?([A-Za-z0-9_-]+)$/i", $line, $matches)) {
		                               print_r($matches);
		                               $slug = $matches[1];
		                       }
		               }
		               // write file
		               file_put_contents("./{$date}-{$slug}.md", $post_str);
		       }
		}
	?>

I did this for all three files (each ExpressionEngine channel was now a file) and found myself with a lot of markdown files with the proper YAML formatting. Exactly what I wanted.

## Redirecting from pikemurdy.com

The next hurdle was the URL structure. ExpressionEngine&rsquo;s URLs were written `domain/channel/post-title`. Jekyll has several URL structure options, but I opted for &ldquo;pretty&rdquo; because it was, well, pretty. Also the `domain/year/month/day/post-title` structure of post URLs is common and familiar.

The new URLs would have the dates, whereas the previous URLs did not at all. I&rsquo;m not an HTACCESS expert and I knew of no way to do it, other than redirect each and every old URL to the appropriate new one. But manually write each redirect? *No thanks*.

So I wrote another template in ExpressionEngine that would write it for me:


	RewriteEngine on
	{exp:channel:entries channel="blog|collections|articles|quentin" limit="9999" rdf="off"}
	
	{if channel_short_name == "blog"}Redirect 301 {title_permalink='blog/entry'}  http://michaelpurdy.org/{entry_date format='%Y/%m/%d'}/{url_title}{/if}
	{if channel_short_name == "articles"}Redirect 301 {title_permalink='articles/entry'}  http://michaelpurdy.org/{entry_date format='%Y/%m/%d'}/{url_title}{/if}
	{if channel_short_name == "collections"}Redirect 301 {title_permalink='collections/detail'}  http://michaelpurdy.org/{entry_date format='%Y/%m/%d'}/{url_title}{/if}
	
	{/exp:channel:entries}


Visiting this template wrote a file that listed each URL and a redirect to the new URL. I am sure there are all kinds of variables that will do this more efficiently in HTACCESS. But this works.

And with that. I was done. Now every article and post on this site is completely static. There is no CMS to maintain. No server requirements at all. No plugins to get working right. No caching. As simple as it gets. And pretty manageable.

Best of all, all of my content is in easy, familiar files. Everything is listed in a file system. No fiddling with with databases. No wondering about the purpose of a random file on the server.