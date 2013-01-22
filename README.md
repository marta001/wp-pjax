wp-pjax
=======

Wordpress plugin to add pjax

Read more about [pjax](https://github.com/defunkt/jquery-pjax), right now the plugin calls pjax on the body element and we have wordpress serve up the page trimmed after the body element up until where the `wp_footer()` call.

---

#### Usage

After installing the plugin, all you have to do is activate it run it at default settings. 

By default, we use the body element as the container, you can, however, easily change this (see below). Once the plugin is activated, we self instantiate the `WP_Pjax` class and assign it to `$wp_pjax`.

A note on body classes:

By default, pjax is set up to replace the body content. This is all well and good but you need to keep in mind that since it's replacing the body content it isn't replacing those useful body classes that some page structure styles depend on. The easiest way around this is to just put a div wrapper as a direct descendant and output the `<?php body_class(); ?>` on that. If you do this, verify your styles that target body classes are generic `.class-name` selectors instead of `body.class-name`.

---

#### Customization

If you'd like to customize the container that pjax will work on you have to do two things:
- set the `$wp_pjax->container_el` (Default set to `'body'`)
- Place the delimeters properly within said container.

`$wp_pjax->container_el` is what it sounds like, it's a jquery selector friendly string, anything that would work between `$()` for pjax.
An example of this is as follows, you'd place this some place like `functions.php`.
```php
if(class_exists('WP_pjax')){

  $wp_pjax->container_el = '#main';

}
```

After customizing the container element, you'll also need to add the delimeter into the right place for pjax responses. I've made a method available to do this: `$wp_pjax->output_delim();`. 

For example, add this into your template files just inside your container element. Once just after it opens, and again just before you close it.
```php
<?

if(class_exists('WP_pjax')){
  global $wp_pjax;
  $wp_pjax->output_delim();
}

?>
```


There are some other customizable features as well:
- `$wp_pjax->filters`: If you want to not use pjax to serve links with a particular href, set this, it's an array that is by default set to `array(".jpg", ".png", ".pdf")`.
- `$wp_pjax->delim`: In an effort to not step on any toes, you have access to the delimeter that we use to chop up the buffer content to server a pjax request, though you shouldn't have to touch this. Default set to `"@@@PJAXBREAK@@@"`.
- `$wp_pjax->pjax_target`: Set this if you want to tell pjax to watch something else other than the default `'a'`.

You can always just write your own site-pjax, just deregister and then reregister yours. Take a look at `/assets/js/site-pjax.js` for available `pjaxData`.

---

#### Coming up

Taking well thought out requests.

---

#### Contributing

If you'd like to contribute, you're in luck because I'm accepting pull requests. I'll review and then tie them in if they make sense. Thanks!
