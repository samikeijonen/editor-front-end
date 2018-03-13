# Testing Gutenberg front-end

This is first draft what is happening in the front-end when you have written with Gutenberg. 

[Check the demo page](https://foxland.fi/demo/gutenberg/).

## Headings

- Nothing special here.
- You can set alignment to left, center, or right.
- Output is inline styles.
- Same as current editor.

`<h2>This is heading</h2>` or with inline style `<h2 style="text-align:center">This is heading</h2>`.

## Subheading

**Markup**

```php
<p class="wp-block-subhead">This is sub heading</p>
```

```css
p.wp-block-subhead {
	font-size: 1.1em;
	font-style: italic;
	opacity: 0.75;
}
```

- At the moment this can be used anywhere, not just under main title.

## Paragraps

- You can set alignment to left, center, or right.
- Output is inline styles.

```html
<p>This is normal paragraph. We know this is the Gutenberg, and we are testing it!</p>
```

Or with inline style:

```html
<p style="text-align:center">This is centered paragraph. We know this is the Gutenberg, and we are testing it!</p>
```

But we can also apply several settings to paragraph:

- Drop Cap.
- Background color.
- Text color.
- Block alignment.

**Markup**:

```php
<p style="background-color:#eee;font-size:28px;text-align:left" class="has-background has-drop-cap">
This is styled paragraph. We <a href="https://foxnet.fi">know</a> this is the Gutenberg, and we are testing it!
</p>
```

**CSS**:

```css
p.has-drop-cap {
	&:first-letter {
		float: left;
		font-size: 4.1em;
		line-height: 0.7;
		font-family: serif;
		font-weight: bold;
		margin: .07em .23em 0 0;
		text-transform: uppercase;
		font-style: normal;
	}
}

p.has-drop-cap:not( :focus )  {
	overflow: hidden;
}

p.has-background {
	padding: 20px 30px;
}
```

**conclusion**: Seems OK by me.

## Images

- There are some changes in the markup compared to current editor.

**Gutenberg Markup**:

```php
<figure class="wp-block-image">
	<img src="http://wordpress-svn/src/wp-content/uploads/2017/03/sandwich-2-1024x614.jpg" alt="Eating healthy">
</figure>
```

**Old Markup**:

```php
<p>
	<img src="URL" alt="Eating healthy">
</p>
```

**Gutenberg Markup with caption**:

```php
<figure class="wp-block-image">
	<img src="http://wordpress-svn/src/wp-content/uploads/2017/03/sandwich-2.jpg" alt="">
	<figcaption>
		This is <a href="http://wordpress-svn/src/">caption</a>
	</figcaption>
</figure>
```

**Old Markup with caption**:

```php
<figure id="attachment_6431" style="width: 1024px" class="wp-caption alignnone">
	<img class="wp-image-6431 size-large" src="URL" alt="Array Themes are ok" width="1024" height="907" srcset="stuff in here">
	<figcaption class="wp-caption-text">Array Themes are more than Okay</figcaption>
</figure>
```

- Markup is semantic HTML5, which is nice.
- But in old editor markup without caption is with `</p>` tags like this `<p><img scr="URL"></p>`. This might potentially have small design issues.
- Gutenberg also uses different classes for `<figure>` and `<figcaption>` so old styles won't work.
- Old editor also used several classes like `wp-image-{ID} size-large`.

**CSS**

```css
.wp-block-image figcaption {
	margin-top: 0.5em;
	color: $dark-gray-300;
	text-align: center;
	font-size: $default-font-size;
}
```

## Galleries

**Gutenberg Markup**:

```php
<ul class="wp-block-gallery alignnone columns-3 is-cropped">
	<li class="blocks-gallery-item>
		<figure"><img src="http://wordpress-svn/src/wp-content/uploads/2017/03/espresso-2.jpg" alt="Alt text" data-id="28" data-url="URL"></figure>
	</li>
	<li class="blocks-gallery-item>
		<figure><img src="http://wordpress-svn/src/wp-content/uploads/2017/03/coffee-2.jpg" alt="" data-id="30" data-url="URL"></figure>
	</li>
	<li class="blocks-gallery-item>
		<figure class="blocks-gallery-image"><img src="http://wordpress-svn/src/wp-content/uploads/2017/03/sandwich-2.jpg" alt="" data-id="29" data-url="URL"></figure>
	</li>
</ul>
```

**Old markup**:

```php
<div id="gallery-1" class="gallery galleryid-1976 gallery-columns-3 gallery-size-thumbnail">
	<figure class="gallery-item">
		<div class="gallery-icon landscape">
			<img width="150" height="150" src="URL" class="attachment-thumbnail size-thumbnail" alt="" aria-describedby="gallery-1-29" srcset="stuff" sizes="100vw">
		</div>
		<figcaption class="wp-caption-text gallery-caption" id="gallery-1-29">
			This is caption
		</figcaption>
	</figure>
	
	<figure class="gallery-item">
		<div class="gallery-icon landscape">
			<img width="150" height="150" src="URL" class="attachment-thumbnail size-thumbnail" alt="" srcset="stuff" sizes="100vw">
		</div>
	</figure>

</div>
```

- Pretty much the same comments as in images.
- Markup and classes are changing a little which means new Galleries styles are Gutenberg-looking and old galleries are theme-looking.

**CSS**

```css
.wp-block-gallery,
.wp-block-gallery.alignleft,
.wp-block-gallery.alignright,
.wp-block-gallery.aligncenter {
	display: flex;
	flex-wrap: wrap;
	list-style-type: none;

	.blocks-gallery-image,
	.blocks-gallery-item {
		margin: 8px;
		display: flex;
		flex-grow: 1;
		flex-direction: column;
		justify-content: center;
		position: relative;


		figure {
			margin: 0;
			height: 100%;
			display: flex;
			align-items: flex-end;
		}

		img {
			display: block;
			max-width: 100%;
			height: auto;
		}

		figcaption {
			padding-top: 3px;
			color: $white;
			text-align: center;
			font-size: $default-font-size;
			background-color: rgba($color: $black, $alpha: 0.7);
			position: absolute;
			width: 100%;
		}
	}

	// Cropped
	&.is-cropped .blocks-gallery-image,
	&.is-cropped .blocks-gallery-item {
      a,
      img {
			flex: 1;
			width: 100%;
			height: 100%;
			object-fit: cover;

		}

		// Alas, IE11+ doesn't support object-fit
		_:-ms-lang(x), figure {
			height: auto;
			width: auto;
		}
	}

	// Responsive fallback value, 2 columns
	& .blocks-gallery-image,
	& .blocks-gallery-item {
		width: calc( 100% / 2 - 16px );
	}

	&.columns-1 .blocks-gallery-image,
	&.columns-1 .blocks-gallery-item {
		width: calc(100% / 1 - 16px);
	}

	@include break-small {
		@for $i from 3 through 8 {
			&.columns-#{ $i } .blocks-gallery-image,
			&.columns-#{ $i } .blocks-gallery-item {
				width: calc(100% / #{ $i } - 16px );
			}
		}
	}
}
```

## Quotes

**Markup**:

```php
<blockquote class="wp-block-quote">
	<p>This is quote with nothing to say</p>
	<cite>From Xami K.</cite>
</blockquote>
```

**Option 2 markup**

```php
<blockquote class="wp-block-quote is-large">
	<p>This is quote with nothing to say</p>
	<cite>From Xami K.</cite>
</blockquote>
```

**CSS**

```css
.wp-block-quote {
	cite,
	footer {
		color: $dark-gray-300;
		margin-top: 1em;
		position: relative;
		font-size: $default-font-size;
		font-style: normal;
	}

	&.is-large {
		margin: 0 0 16px;
		padding: 0 1em;

		p {
			font-size: 24px;
			font-style: italic;
			line-height: 1.6;
		}

		cite,
		footer {
			font-size: 19px;
			text-align: right;
		}
	}
}
```
Seems OK by me.

## Lists

These are pure HTML without CSS classes.

## Cover image

**Markup**:

```php
<div class="wp-block-cover-image has-background-dim" style="background-image:url(http://wordpress-svn/src/wp-content/uploads/2017/09/abc.jpg)">
	<p class="wp-block-cover-image-text">Donate Now – We need you</p>
</div>
```

**CSS**

```css
.wp-block-cover-image {
	position: relative;
	background-size: cover;
	min-height: 430px;
	width: 100%;
	margin: 0 0 1.5em 0;
	display: flex;
	justify-content: center;
	align-items: center;

	&.has-left-content {
		justify-content: flex-start;

		h2,
		.wp-block-cover-image-text {
			margin-left: 0;
			text-align: left;
		}
	}

	&.has-right-content {
		justify-content: flex-end;

		h2,
		.wp-block-cover-image-text {
			margin-right: 0;
			text-align: right;
		}
	}

	h2,
	.wp-block-cover-image-text {
		color: white;
		font-size: 2em;
		line-height: 1.25;
		z-index: 1;
		margin-bottom: 0;
		max-width: $visual-editor-max-width;
		padding: $block-padding;
		text-align: center;

		a,
		a:hover,
		a:focus,
		a:active {
			color: white;
		}
	}

	&.has-parallax {
		background-attachment: fixed;
	}

	&.has-background-dim:before {
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		bottom: 0;
		right: 0;
		background-color: rgba( black, 0.5 );
	}

	@for $i from 1 through 10 {
		&.has-background-dim.has-background-dim-#{ $i * 10 }:before {
			background-color: rgba( black, $i * 0.1 );
		}
	}

	&.components-placeholder {
		height: inherit;
	}

}
```

- I think `<section>` is OK here since there is `<h2>`.
- At the moment you can't change the dim or text color in the block settings.
- Without the dim color there is big risk for color contrast issues.
- You can align (left, center, right) cover image but it doesn't have effect in the front end.
- With `add_theme_support( 'gutenberg', array( 'wide-images' => true ) )` I'd actually like the cover image behave like regular image (wide image, full width image). But there are no classes for that. [See issue 2650](https://github.com/WordPress/gutenberg/issues/2650).

## Video and audio

**Video markup**:

```php
<figure class="wp-block-video">
	<video controls="" src="http://wordpress-svn/src/wp-content/uploads/2013/12/2014-slider-mobile-behavior.mov"></video>
	<figcaption>Video caption</figcaption>
</figure>
```

**CSS**

```css
.wp-block-video figcaption {
	margin-top: 0.5em;
	color: $dark-gray-300;
	text-align: center;
	font-size: $default-font-size;
}
```

**Audio markup**:

```php
<figure class="wp-block-audio">
	<audio controls="" src="http://wordpress-svn/src/wp-content/uploads/2012/07/originaldixielandjazzbandwithalbernard-stlouisblues.mp3"></audio>
</figure>
```

**CSS**

```css
.wp-block-audio figcaption {
	margin-top: 0.5em;
	color: $dark-gray-300;
	text-align: center;
	font-size: $default-font-size;
}
```

## Pull quote

**Markup**:

```php
<blockquote class="wp-block-pullquote alignleft">
	<p>Left pullquote</p>
	<cite>Caption this is</cite>
</blockquote>
```

- Similar issues than regular blockquotes.

**CSS**

```css
.wp-block-pullquote {
	border-top: 4px solid $dark-gray-500;
	border-bottom: 4px solid $dark-gray-500;
	color: $dark-gray-600;
	padding: 3em 0;
	text-align: center;

	&.alignleft,
	&.alignright {
		max-width: 400px;

		> p {
			font-size: 20px;
		}
	}

	> p {
		font-size: 24px;
		font-weight: 900;
		line-height: 1.6;
	}

	cite,
	footer {
		color: $dark-gray-600;
		position: relative;
		font-weight: 900;
		text-transform: uppercase;
		font-size: 13px;
	}
}
```

## Tables

**Markup**:

```php
<table class="wp-block-table">
	<tr>
		<td>First name</td>
		<td>Last name</td>
	</tr>
	<tr>
		<td>Xami</td>
		<td>Gello</td>
	</tr>
	<tr>
		<td>Zanna</td>
		<td>Petronen</td>
	</tr>
</table>
```

- You can't put `caption` or `<th>` elements.
- That's accessibility issue.

**CSS**:

```css
.wp-block-table {
	overflow-x: auto;
	display: block;

	table {
		border-collapse: collapse;
		width: 100%;
	}
}
```

- CSS seems odd, why there is display: block which is messing the theme styles?

## Pre-formatted and code

**Markup**:

```php
<pre class="wp-block-preformatted">Is this pre-formatted</pre>
```

```php
<pre class="wp-block-code"><code>h2 {
   color: #000; // This is code.
}</code></pre>
```

- Seems OK by me.
- No styles in the front end which is good.

## Custom HTML

This can have any markup you want. Nice.

## Classic

Classic text seem to have more options like the old editor inside one block. Nice.

## Verse block

**Markup**:

```php
<pre class="wp-block-verse">Verse? What the heck is Verse?</pre>
```

Isn't this the same as pre-formatted with different class?

## Separator

**Markup**:

```php
<hr class="wp-block-separator">
```

**CSS**:

```css
.wp-block-separator {
	border: none;
	border-bottom: 2px solid $dark-gray-100;
	max-width: 100px;
	margin: 1.65em auto;
}
```

Many themes have styles for `<hr>` so this will have small styling issues with many themes.

## Buttons

**Markup**:

```php
<div class="wp-block-button alignnone"><a class="wp-block-button__link" href="http://wordpress-svn/src/">This is button</a></div>
```

**Markup with background-color**:

```php
<div class="wp-block-button aligncenter"><a class="wp-block-button__link" href="#" style="background-color:#0066cc;color:#fff">Hit me up</a></div>
```

**CSS**

```css
$blocks-button__height: 46px;
$blocks-button__line-height: $big-font-size + 6px;

.wp-block-button {
	margin-bottom: 1.5em;

	& .wp-block-button__link {
		background-color: $dark-gray-700;
		border: none;
		border-radius: $blocks-button__height / 2;
		box-shadow: none !important;
		color: $white;
		cursor: pointer;
		display: inline-block;
		font-size: $big-font-size;
		line-height: $blocks-button__line-height;
		margin: 0;
		padding: ( $blocks-button__height - $blocks-button__line-height ) / 2 24px;
		text-align: center;
		text-decoration: none !important;
		white-space: nowrap;
		word-break: break-word;

		&:hover,
		&:focus,
		&:active {
			background-color: $dark-gray-700;
			color: $white;
		}
	}

	&.aligncenter {
		text-align: center;
	}

	&.alignright {
		text-align: right;
	}
}
```

## Columns (nested blocks)

**Markup**:

```php
<div class="wp-block-columns has-2-columns">
        <h3 class="layout-column-1">Kappas vaan</h3>
	<p class="layout-column-1">Kappas vaan</p>
	
	<h3 class="layout-column-2">Kappas vaan</h3>
	<p class="layout-column-2">Kappas vaan</p>
</div>
```

This can leave to weird empty spaces, see issue https://github.com/WordPress/gutenberg/issues/5351

## Text columns

**Markup**:

```php
<section class="wp-block-text-columns alignundefined columns-2">
	<div class="wp-block-column">
		<p>Cras vel nunc auctor massa iaculis scelerisque in semper libero. Pellentesque tincidunt auctor egestas. Quisque a ligula vehicula, condimentum dui pretium, imperdiet erat. Aliquam molestie nisi a metus rhoncus pharetra. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas.</p>
	</div>
	<div class="wp-block-column">
		<p>Cras vel nunc auctor massa iaculis scelerisque in semper libero. Pellentesque tincidunt auctor egestas. Quisque a ligula vehicula, condimentum dui pretium, imperdiet erat. Aliquam molestie nisi a metus rhoncus pharetra. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas</p>
	</div>
</section>
```

- I'm not sure is the `<section>` correct here since there is no heading inside it and it's just for styling the grid.
- Grid is not responsive. I'd even go that far that we use CSS Grid here with solid flexbox markup.

## Latest posts

**Markup**

```html
<ul class="wp-block-latest-posts aligncenter">
	<li><a>...</a></li>
	<li><a>...</a></li>
	<li><a>...</a></li>
</ul>
```

- Markup is just a list as it should be.
- With Grid mode we could use similar idea than in text columns.

## Gategories block

This have extra `<div>` element around `<ul>` or `<select>`. For example:

```html
<div class="wp-block-categories wp-block-categories-dropdown aligncenter">
	<select>
		<option></option>
		<option></option>
		<option></option>
	</select>
</div>
```

## Embeds

All embeds have similar markup like this:

```php
<figure class="wp-block-embed-twitter">
	Markup for embed itself
</figure>
```

- It might be a good idea to have general class `wp-block-embed` for all embeds.
