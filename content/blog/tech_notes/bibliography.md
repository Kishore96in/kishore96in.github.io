+++
title = "Bibliography management in Hugo"
date = "2025-05-31T13:35:34+05:30"
url = "blog/bibliography-hugo"
tags = [
	"tech-notes",
	]

[params]
	bib = "global_refs.json"
+++

The content of this website is generated from a set of Markdown files using [Hugo](https://gohugo.io/).
To manage citations in blog posts, the simplest option is to manually add footnotes, links, etc., which is a pain.

I have implemented a set of shortcodes to manage citations of papers in my blog posts.
<!--more-->
These started off as a copy of the shortcodes implemented in the '[third hemisphere](https://github.com/pprevos/hugo-thirdhemisphere/)' Hugo theme[^note_third_hemi], but have now been extensively modified to suit my needs.


[^note_third_hemi]: Described at <https://lucidmanager.org/productivity/hugo-bibliography/>

## The code

You can view the code at the [Git repository](https://github.com/Kishore96in/kishore96in.github.io/) associated with this website, under the folder `layouts/partials/cite`.
A small wrapper to allow the use of the cite function in markdown files is at `shortcodes/cite.html`.
Automatic printing of the bibliography is done thorough a line at the end of `layouts/_default/single.html`.

## Creating a bibliography file

If you have your references in a [bibtex](https://en.wikipedia.org/wiki/BibTeX) file, you can generate the required [CSL-JSON](https://citeproc-js.readthedocs.io/en/latest/csl-json/markup.html) file by running
```bash
pandoc refs.bib -t csljson -o global_refs.json
```
Copy this JSON file to the `assets` folder of your website.

##  Usage

In the preamble ([TOML](https://toml.io/en/) format) of your blog post, add
```toml
[params]
	bib = "global_refs.json"
```
You can then cite references by using the following shortcodes (corresponding biblatex macros are given on the left):
```markdown
\textcite: {{</* cite t CITE_KEY */>}}
\parencite: {{</* cite p CITE_KEY */>}}
\cite (\citealp in natbib): {{</* cite alp CITE_KEY */>}}
\fullcite: {{</* cite f CITE_KEY */>}}
```
Entries cited in this way (except for fullcite) will be added to the bibliography automatically).
The bibliography is implemented as a partial which can be included in the HTML templates using
```html
{{ partial "bibliography.html" . }}
```

## Examples

- Here is a text citation: {{< cite t kapyla2019Lambda >}}.
- And here is the same citation in brackets: {{< cite p kapyla2019Lambda >}}.
- Let us also cite {{< cite t LeiSte81 >}} and {{< cite t Chr02 >}}.
- For the sake of completeness, `alp` looks like this: {{< cite alp kapyla2019Lambda >}}.
- Here is another work by Käpylä: {{< cite t Kap19 >}}.
Note how a suffix has been added to the year to distinguish it from another (previously cited) study by the same author in the same year {{< cite p kapyla2019Lambda >}}.

All these entries will appear in the [bibliography](#bibliography).

Here is a full citation which will not be added to the bibliography:
{{< cite f MoffattMagFieldGenBook >}}
