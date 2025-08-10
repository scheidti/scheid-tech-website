+++
title = "Erzeugen einer llms.txt-Datei mit dem Hugo Static Site Generator"
description = "Die folgenden Schritte zeigen einen möglichen Weg, eine llms.txt-Datei mit Hugo zu erzeugen."
date = "2025-08-10"
images = []
tags = ["Hugo", "AI"]
summary = "Die *llms.txt*-Datei ist ein vorgeschlagener Standard um Webseiten LLM-freundlicher zu gestalten, indem eine Markdown-Datei bereitgestellt wird, die wichtige, für LLMs lesbare Informationen enthält. Dieser Beitrag zeigt eine Möglichkeit, wie man sie mit Hugo erstellen kann."
outputs = ["html", "markdown"]
[sitemap]
  changefreq = "monthly"
  priority = 0.5
+++

Die *llms.txt*-Datei ist ein vorgeschlagener [Standard](https://llmstxt.org/#proposal) um Webseiten LLM-freundlicher zu gestalten, indem eine Markdown-Datei bereitgestellt wird, die wichtige, für LLMs lesbare Informationen enthält und auf weitere Markdown-Seiten verweisen kann.

Die folgenden Schritte zeigen einen möglichen Weg, eine llms.txt-Datei mit Hugo zu erzeugen.

## Voraussetzungen

- [Hugo](https://gohugo.io/) Website-Projekt

## Schritte

1. Media Type und Output Format in der *hugo.toml* neu hinzufügen:
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

2. Ein Template für das neu definierte Output Format (*txt*) erstellen, um die llms.txt-Datei zu rendern:
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
   Dieses Template rendert zuerst den Markdown-Inhalt aus *content/llms.md* und fügt anschließend eine Liste der Seiten hinzu (Seiten mit dem Output Format *markdown*).

3. Ein neues Template erstellen, um Seiten im Output Format *markdown* zu rendern (falls noch nicht vorhanden):
   *layouts/_default/single.md*
   ```markdown
   # {{ .Title }}
   {{ .RawContent | safeHTML }}
   ```
   Dieses Template rendert einfach den unformatierten Markdown-Inhalt einer Seite und fügt den Titel als Überschrift hinzu.

4. Die Datei *content/llms.md* anlegen:
   ```markdown
   +++
   outputs = ['llms']
   +++
   # Title

   > Optional description goes here

   Optional details go here
   ```
   Weitere Informationen sind unter [llms.txt Format](https://llmstxt.org/#format) zu finden.

5. Füge das Output Format *markdown* zu den Seiten hinzu, die in der llms.txt enthalten sein sollen:
   ```markdown
   +++
   title = "Page title"
   outputs = ["html", "markdown"]
   +++
   ```
   Hugo generiert die Datei nun unter *public/llms.txt*.