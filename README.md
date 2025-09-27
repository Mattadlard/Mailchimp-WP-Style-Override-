# Mailchimp WordPress Override

High-contrast, accessible colour overrides for Mailchimp's embedded signup forms on WordPress.  
Keeps Mailchimp's structure, fixes the anaemic greys.

---

## Quick start (WordPress, no code)

1. Paste your Mailchimp embed form into a block or widget as usual.  
   It should include their stylesheet line:

   ```html
   <link href="//cdn-images.mailchimp.com/embedcode/classic-061523.css" rel="stylesheet" type="text/css">
   ```

2. Go to **Appearance → Customise → Additional CSS** and paste everything from:
   ```
   src/mailchimp-override.css
   ```

3. Publish. Refresh the page. Type in the fields:  
   - Labels and helper text should be black  
   - Input text should be black on white  
   - Subscribe button should be white with black text (inverts on hover)

---

## How to use your own Mailchimp link

The **form link (action URL)** is what tells Mailchimp which list (audience) to add subscribers to.  
This is unique to your account, and Mailchimp gives it to you when you generate the embed form.

Example from Mailchimp:

```html
<form action="https://YOUR-DATA-CENTER.list-manage.com/subscribe/post?u=YOUR_U_ID&amp;id=YOUR_LIST_ID" method="post" target="_blank">
```

### What each part means
- `YOUR-DATA-CENTER` → usually looks like `us20`, `us21`, etc. Depends on your account region.  
- `YOUR_U_ID` → your unique account identifier.  
- `YOUR_LIST_ID` → the specific audience (list) you’re connecting the form to.  

### Do I change the CSS for this?
No. The CSS has nothing to do with the form link.  
The CSS just fixes the **styling** of the form so it’s readable.  
You only copy the form code (with your action URL) from Mailchimp and drop it into WordPress (or your site). Then the override CSS makes it look good.

### Step-by-step
1. In Mailchimp, go to **Audience → Signup forms → Embedded forms**.  
2. Copy the embed code they give you. It already includes the correct `action="..."` URL for your account.  
3. Paste that embed code into your WordPress page, post, or widget.  
4. Add the override CSS from this repo (via WordPress → Customiser → Additional CSS).  
5. Done. Subscribers will go to your own Mailchimp audience, but the form won’t look like faded chalk on a dusty board.

---

## Alternative: enqueue a hosted CSS file

If you don’t want to use “Additional CSS,” host `src/mailchimp-override.css` and enqueue it in your theme or a tiny plugin so it loads **after** Mailchimp’s CSS.

**functions.php**

```php
function my_mailchimp_override() {
  wp_enqueue_style(
    'mailchimp-override',
    get_stylesheet_directory_uri() . '/css/mailchimp-override.css',
    array(),
    '1.0'
  );
}
add_action('wp_enqueue_scripts', 'my_mailchimp_override');
```

Then place `mailchimp-override.css` in your theme at `/wp-content/themes/YOUR-THEME/css/mailchimp-override.css`.

---

## Load order matters

- Mailchimp CSS must load first:
  ```html
  <link href="//cdn-images.mailchimp.com/embedcode/classic-061523.css" rel="stylesheet" type="text/css">
  ```
- Your override must load **after** it. That’s how your colours win.

---

## Troubleshooting

- **Some text still grey/white?**  
  Caches love stale styles. Hard refresh or purge your caching plugin/Cloudflare.  

- **Fields still unreadable?**  
  Your theme may be targeting inputs globally. Our selectors use `#mc_embed_signup ...` to win. If something slips through, temporarily try:  
  ```css
  #mc_embed_signup input { color: #000 !important; background: #fff !important; }
  ```

- **Button still off-brand?**  
  Edit the button rules in `src/mailchimp-override.css`:
  ```css
  #mc_embed_signup .button { background: #fff; color: #000; border: 1px solid #000; }
  #mc_embed_signup .button:hover { background: #000; color: #fff; }
  ```

---

## What this does (and doesn’t)

-  Makes labels, helper text, input text, and placeholders readable.  
-  Gives the subscribe button high-contrast states.  
-  Adds visible focus rings for keyboard users.  
X  Doesn’t change your layout or Mailchimp’s HTML.

---

## License

MIT. Do what you like, don’t sue anyone for doing the same.
