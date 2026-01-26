+++
title = "Anonymizing PDF files"
date = "2026-01-26T15:30:25+05:30"
url = "blog/anonymizing-pdfs"
tags = [
	"tech-notes",
	]
+++

Some PDF viewers allow you to add annotations which are embedded in the PDF file itself.
This is a problem if one later wants to share the PDF with other people.
Moreover, some publishers add text to downloaded PDF files that track who downloaded a particular file from their website.
This is just a short list of [pdftk](https://gitlab.com/pdftk-java/pdftk) invocations to strip unwanted information from a PDF.

<!--more-->

## Annotations

To strip annotations added by [Okular](https://okular.kde.org/):
```bash
pdftk PDF_FILE.pdf output - uncompress | sed '/^\/Annot *okular*/d/' | pdftk - output STRIPPED.pdf compress
```

## Trackers

Something like the below should suffice to remove publisher-added tracking information:
```bash
pdftk PDF_FILE.pdf output - uncompress | sed "s|INTER-UNIVERSITY CENTRE FOR ASTRONOMY AND ASTROPHYSICS|Aaron|g" | sed "s|IUCAA|MIT|g" | sed "s|on ../../..|on 11/Jan/13|g" | pdftk - output JACK_SPARROW.pdf compress
```

## Original source
<https://stackoverflow.com/questions/49598797/remove-pdf-annotations-via-command-line#49614525>
