---
title: 'Drupal &#038; fckeditor'
author: emijayne
layout: post
permalink: /drupal-fckeditor/
post_views_count:
  - 345
categories:
  - random code
---
<p style="color: #000000;">
  I have around 10 <a style="color: #9d3707;" href="http://drupal.org/" target="_blank" rel="nofollow">drupal</a> installations for various work projects.  I love them.  Drupal *is* the awesomeness. As awesome as Drupal is, using a WYSIWYG editing module makes life so much easier for posts and pages.  My choice is <a style="color: #9d3707;" title="fckeditor" href="http://drupal.org/project/fckeditor" target="_blank" rel="nofollow">fckeditor</a>.  It has everything I need and could want.. it has the perfect auto exceptions (for putting limited or no editing on admin pages).. and in my opinion is better than other editors out there.
</p>

<p style="color: #000000;">
  .. well, except when it decided (on my special themed sites) to take my background image and put it <a style="color: #9d3707;" href="http://emijayne.com/wp/wp-content/uploads/fckeditor2.png" rel="nofollow">in the iframe</a> for the editor.  As in this example, on a number of the sites it is a rather dark image which makes the editing process a bit harder.
</p>

<p style="color: #000000;">
  Happily, upon searching, I found a <a style="color: #9d3707;" href="http://drupal.fckeditor.net/tricks?page=4#comment-2271" target="_blank" rel="nofollow">comment </a>in the &#8220;Tips & Tricks&#8221; section of <a style="color: #9d3707;" href="http://drupal.fckeditor.net/" target="_blank" rel="nofollow">drupal.fckeditor.net</a> that I don&#8217;t think had anything to do with my weird image issue.  They mentioned adding the following line to the &#8220;Advanced options&#8221; section of the FCKEditor administration:
</p>

<pre>EditorAreaStyles = "body{background:#FFFFFF;text-align:left;}";</pre>

<p style="color: #000000;">
  &#8230; and <a style="color: #9d3707;" href="http://emijayne.com/wp/wp-content/uploads/fckeditor.png" rel="nofollow">voila</a>! White iframe happiness.  <img src="http://www.emijayne.com/wp/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />
</p>