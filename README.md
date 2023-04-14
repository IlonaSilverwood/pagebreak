pagebreak
=========

This filter converts paragraps containing only the LaTeX
`\newpage` or `\pagebreak` command into appropriate pagebreak
markup for other formats. The command must be the only contents
of a raw TeX block in order to be recognized. I.e., for Markdown
the following is sufficient:

    Paragraph before page break

    \newpage

    Paragraph after page break


Usage
-----

Fully supported output formats are:

- AsciiDoc / Asciidoctor,
- ConTeXt,
- Docx,
- EPUB,
- groff ms,
- HTML, and
- LaTeX.

ODT is supported, but requires additional settings in the
reference document (see below).

In all other formats, the page break is represented using the
form feed character.

Configuration
-------------

The filter can be configured through the `pagebreak` metadata
field. Currently supported options:

- `break-on.form-feed`: boolean value indicating whether
  the filter should replace paragraphs that contains nothing but
  form feed characters with page breaks. Enabling option can have
  a significant performance impact for large documents and is
  therefore *disabled by default*.

- `html-class`: If you want to use an HTML class rather than an
  inline style set the value of the metadata key `html-class` or
  the environment variable `PANDOC_PAGEBREAK_HTML_CLASS` (the
  metadata 'wins' if both are defined) to the name of the class
  and use CSS like this:

  ``` css
  @media all {
    .page-break	{ display: none; }
  }
  @media print {
    .page-break	{ display: block; page-break-after: always; }
  }
  ```

- `odt-style`: To use with ODT you must create a reference ODT
  with a named paragraph style called `Pagebreak` (or whatever you
  set the metadata field `odt-style` or the environment variable
  `PANDOC_PAGEBREAK_ODT_STYLE` to) and define it as having no
  extra space before or after but set it to have a pagebreak after
  it <https://help.libreoffice.org/Writer/Text_Flow>.

  (There will be an empty placeholder paragraph, which means some
  extra vertical space, and you probably want that space to go at
  the bottom of the page before the break rather than at the top
  of the page after the break!)


Example config in YAML frontmatter:

``` yaml
---
pagebreak:
  break-on:
    # Treat paragraphs that contain just a form feed
    # character as pagebreak markers.
    form-feed: true

  # Use a div with this class instead of hard-coded CSS
  html-class: 'page-break'

  # ODT style used for pagebreak paragraphs
  odt-style: 'Pagebreak'
---
```


Alternative syntax
------------------

The form feed character as the only element in a paragraph is
supported as an alternative to the LaTeX syntax described above.
See [Configuration](#configuration) for info on how to enable this
feature.
