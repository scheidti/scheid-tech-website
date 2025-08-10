+++
title = "Create an llms.txt File with the Hugo Static Site Generator"
description = "The following steps show one way to create and render an /llms.txt file with Hugo, so it’s included automatically in your site’s output."
date = "2025-08-10"
images = []
tags = ["Hugo", "AI"]
summary = "The *llms.txt* file is a proposal to make websites more LLM-friendly with important, machine-readable info and Markdown page links. This post shows how to generate it with Hugo."
outputs = ["html", "markdown"]
[sitemap]
  changefreq = "monthly"
  priority = 0.5
+++

The *llms.txt* file is a [proposal](https://llmstxt.org/#proposal) to make websites more LLM-friendly by providing a simple Markdown file that highlights important, LLM-readable information and points to Markdown versions of key pages.

The following steps show one way to create and render an *llms.txt* file with Hugo, ensuring it’s included automatically in your site’s output.

## Requirements

- [Hugo](https://gohugo.io/) website project

## Steps

1. Add a new media type and output format in your *hugo.toml*:
   ```toml
   [mediaTypes]
     [mediaTypes.'text/plain']
       suffixes = ['txt']

   [outputFormats]
     [outputFormats.llms]
       mediaType = 'text/plain'
       isPlainText = true
       baseName  = "llms"
       root = true
   ```

2. Create a template for your new defined output format (*txt*) to render the llms.txt file:
   *layouts/_default/single.txt*
   ```markdown
   {{ .RawContent | safeHTML }}
   ## Pages
   {{ "\n" }}
   {{- range .Site.Pages -}}
     {{- $p := . -}}
     {{- with .OutputFormats.Get "markdown" -}}
   - [{{ $p.Title }}]({{ .Permalink }}){{ "\n" }}
     {{- end -}}
   {{- end -}}
   ```
   This renders the Markdown content from *content/llms.md* first, followed by a list of your pages
   (all pages with the *markdown* output format will be included).

3. Create a template to render pages in the *markdown* output format (if not already existing):
   *layouts/_default/single.md*
   ```markdown
   # {{ .Title }}
   {{ .RawContent | safeHTML }}
   ```
   This simply renders the plain Markdown content of your page, adding the title as a heading.

4. Create a *content/llms.md* file:
   ```markdown
   +++
   outputs = ['llms']
   +++
   # Title

   > Optional description goes here

   Optional details go here
   ```
   See the [llms.txt Format](https://llmstxt.org/#format) for more details.

5. Add the *markdown* output format to all pages that should be included in llms.txt:
   ```markdown
   +++
   title = "Page title"
   outputs = ["html", "markdown"]
   +++
   ```
   After these steps, Hugo will generate the file at *public/llms.txt*.