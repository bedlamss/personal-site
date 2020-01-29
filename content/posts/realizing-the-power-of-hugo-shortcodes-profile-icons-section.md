---
title: 'Realizing the power of Hugo shortcodes - Profile icons section '
date: '2019-10-04T17:30:27+03:00'
draft: false
tags:
  - hugo
  - go
  - shortcode
  - tutorial
---
Hugo shortcodes offer a great method to modularize a website, separating content from other code. My website has some hacky code snippets for certain parts of it. I know that being sloppy from the beggining is never a good omen so I try to be as strict as possible with myself. 

At the homepage, I include a simple section with fontawesome icons about my profiles in other social networks:

![null](/images/uploads/profiles.png)

 This is the hacky code that was used for this:

```html
{{</* rawhtml */>}}
<h3>
    	<a href="mailto:arvchristos@protonmail.com" target="_blank" title="email"><i class="fas fa-envelope"></i></a><span>&#32;|&#32;</span>
    	<a href="https://www.linkedin.com/in/arvchristos/" target="_blank" title="LinkedIn"><i class="fab fa-linkedin"></i></a><span>&#32;|&#32;</span>
    	<a href="https://www.facebook.com/arvchristos" target="_blank" title="Facebook"><i class="fab fa-facebook"></i></a><span>&#32;|&#32;</span>
		<a href="https://www.github.com/arvchristos" target="_blank" title="GitHub"><i class="fab fa-github"></i></a><span>&#32;|&#32;</span>
		<a href="https://www.gitlab.com/arvchristos" target="_blank" title="GitLab"><i class="fab fa-gitlab"></i></a>   
    </h3>   
{{</* rawhtml */>}}
```

*Notice the span content. We use the special code for space character to preserve it after minification on build.*

However, merging content with HTML is against any static generator's purpose. So, I decided to create a simple shortcode for this. Our goal is to use something like the following:

```html
<h3>
{{</* profiles 
	mail="mailto:arvchristos@protonmail.com"
	linkedin="https://www.linkedin.com/in/arvchristos/"
	facebook="https://www.facebook.com/arvchristos"
	github="https://www.github.com/arvchristos"
	gitlab="https://www.gitlab.com/arvchristos"
*/>}}
</h3>
```

All of the previous parameters should be optional. The following shortcode file implements the this functionality:

```html
<!-- profiles -->

{{ if .Get "mail" }}
	<a href="{{.Get "mail" }}" target="_blank" title="email"><i class="fas fa-envelope"></i></a>
{{end}}

{{ if .Get "linkedin" }}
	<span>&#32;|&#32;</span><a href="{{ .Get "linkedin" }}" target="_blank" title="LinkedIn"><i class="fab fa-linkedin"></i></a>
{{end}}

{{ if .Get "facebook" }}
	<span>&#32;|&#32;</span><a href="{{ .Get "facebook" }}" target="_blank" title="Facebook"><i class="fab fa-facebook"></i></a>
{{end}}

{{ if .Get "github" }}
	<span>&#32;|&#32;</span><a href="{{ .Get "github" }}" target="_blank" title="GitHub"><i class="fab fa-github"></i></a>
{{end}}

{{ if .Get "gitlab" }}
	<span>&#32;|&#32;</span><a href="{{ .Get "gitlab" }}" target="_blank" title="GitLab"><i class="fab fa-gitlab"></i></a>
{{end}}
```

The only requirement for this shortcode is fontawesome although I am sure there is a way to conditionally include it should our profiles shortcode is used.
