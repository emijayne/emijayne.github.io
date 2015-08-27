---
title: Changing Site Name on URL Alias
author: emijayne
layout: post
permalink: /changing-site-name-on-url-alias/
post_views_count:
  - 3193
categories:
  - random code
tags:
  - drupal
  - hooks
---
I set up a site with [Context][1] so that it would change the logo depending on the url alias. For instance all pages with &#8220;one&#8221; would have a different logo than all pages with &#8220;two&#8221;, and so on:

  * www.example.com/one/main
  * www.example.com/one/subpage
  * www.example.com/two/main
  * www.example.com/two/subpage
  * www.example.com/three/main
  * www.example.com/three/subpage

Well, the aforementioned logo was actually just a stylized site name &#8211; not the most accessible, and certainly not the most smaller-device-friendly. So, in trying to make this a bit better for the web, I worked out this preprocess function that I thought I&#8217;d share with the you all:

<div id="snippet">
  <div id="snipplr_embed_75039" class="snipplr_embed">
    <a href="http://snipplr.com/view/75039/for-changing-drupal-site-name-on-url-alias-change/">Code snippet &#8211; For Changing Drupal Site Name on URL Alias change</a> on Snipplr
  </div>
  
  <p>
  </p>
</div>

Basically, this first checks to make sure the Site Name is desired (checked off) in the theme. It then grabs the alias and checks it against the pattern I gave it inside of `drupal_match_path`. If it matches a pattern, then it changes the `site_name` accordingly.. if not, it stays the Site Name that is set in the theme.

 [1]: https://www.drupal.org/project/context "Drupal: Context"