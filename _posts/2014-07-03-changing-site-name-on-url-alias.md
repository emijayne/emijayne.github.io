---
title: Changing Site Name on URL Alias
author: emijayne
layout: post
permalink: /changing-site-name-on-url-alias/
image: drupal.jpg
post_views_count:
  - 3193
categories:
  - random code
tags:
  - drupal
  - hooks
---
I set up a Drupal site with [Context][1] so that it would change the logo depending on the url alias. For instance all pages with `one` would have a different logo than all pages with `two`, and so on:

  * www.example.com/one/main
  * www.example.com/one/subpage
  * www.example.com/two/main
  * www.example.com/two/subpage
  * www.example.com/three/main
  * www.example.com/three/subpage

Well, the aforementioned logo was actually just a stylized site name &emdash; not the most accessible, and certainly not the most smaller-device-friendly. So, in trying to make this a bit better for the web, I worked out this preprocess function that I thought I'd share with the you all:

{% highlight php %}
function YOURTHEME_preprocess_page(&$variables, $hook) {
  if (theme_get_setting('toggle_name')) {
    $path = drupal_get_path_alias();
    if (drupal_match_path($path, 'one*')) {
      $variables['site_name'] = filter_xss_admin('Site Name One');
    } else if (drupal_match_path($path, 'two*')) {
      $variables['site_name'] = filter_xss_admin('Site Name Two');
    } else if (drupal_match_path($path, 'three*')) {
      $variables['site_name'] = filter_xss_admin('Site Name Three');
    }
  }
}
{% endhighlight %}

Basically, this first checks to make sure the Site Name is desired (checked off) in the theme. It then grabs the alias and checks it against the pattern I gave it inside of `drupal_match_path`. If it matches a pattern, then it changes the `site_name` accordingly... if not, it stays the Site Name that is set in the theme.

 [1]: https://www.drupal.org/project/context "Drupal: Context"