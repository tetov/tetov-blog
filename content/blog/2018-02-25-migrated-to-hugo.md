+++
title = "Migrated to Hugo"
tags = [ "blog", "meta", "hugo" ]
date = "2018-02-25T21:38:24+01:00"
+++
I've decided to decouple my [portfolio](https://tetov.se/) from my [blog](https://tetov.se/blog/), mainly to avoid maintaining a Jekyll template that works with both my portfolio content and my blog posts. It felt like this was a good time to try out [Hugo](https://gohugo.io/). Confusing but refreshing.

I've decided to use [page bundles](https://gohugo.io/content-management/organization/#page-bundles) and worked out a shortcode based on [Hugo's embedded figure shortcode](https://gohugo.io/content-management/shortcodes/#use-hugo-s-built-in-shortcodes). `glob` is either the file name of the image, or a [glob](https://en.wikipedia.org/wiki/Glob_%28programming%29).

{{< highlight go>}}
// /layouts/shortcodes/pbfig.html
{{ $img := $.Page.Resources.GetMatch (.Get "glob")}}
<figure{{ with .Get "class" }} class="{{.}}"{{ end }}>
    {{ if .Get "link"}}<a href="{{ .Get "link" }}"{{ with .Get "target" }} target="{{ . }}"{{ end }}{{ with .Get "rel" }} rel="{{ . }}"{{ end }}>{{ end }}
        <img src="{{ $img.RelPermalink }}" {{ if or (.Get "alt") (.Get "caption") }}alt="{{ with .Get "alt"}}{{.}}{{else}}{{ .Get "caption" }}{{ end }}" {{ end }}{{ with .Get "width" }}width="{{.}}" {{ end }}{{ with .Get "height" }}height="{{.}}" {{ end }}/>
    {{ if .Get "link"}}</a>{{ end }}
    {{ if or (or (.Get "title") (.Get "caption")) (.Get "attr")}}
    <figcaption>{{ if isset .Params "title" }}
        <h4>{{ .Get "title" }}</h4>{{ end }}
        {{ if or (.Get "caption") (.Get "attr")}}<p>
        {{ .Get "caption" }}
        {{ with .Get "attrlink"}}<a href="{{.}}"> {{ end }}
            {{ .Get "attr" }}
        {{ if .Get "attrlink"}}</a> {{ end }}
        </p> {{ end }}
    </figcaption>
    {{ end }}
</figure>
{{< /highlight >}}

And to use it in your markdown.

{{< highlight go>}}
{{</* pbfig glob="" alt="" */>}}
{{< /highlight >}}

The post [Hugo Page Resources and how to use them](https://regisphilibert.com/blog/2018/01/hugo-page-resources-and-how-to-use-them/) is a great resource to understand Page Bundles.

**Update 2019-05-01:**
I've put the sites back together, and replace my own shortcode with [Kaushal Modi's](https://gitlab.com/kaushalmodi/hugo-theme-refined/blob/master/layouts/shortcodes/figure.html).
