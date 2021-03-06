HTML2TeX
========

This is a simple library and command-line tool to convert HTML documents into
LaTeX. The intended use is for preparing ebooks, but it might also be useful
for other purposes.

Command-line use
----------------

    Usage: html2tex [options] input.html [output.tex]
        -t, --title TITLE                Set (or override) the title of the document
        -a, --author AUTHOR              Set (or override) the author of the document
        -c, --document-class CLASS       Set the LaTeX document class to be used; default is book
        -h, --help                       Display this screen

If the output file is not specified, the program will create an output file
based on the input filename, replacing the extension with `.tex` – or, if there
is no extension, by appending `.tex`.

The title and author will, by default, be extracted from the HTML document,
using either Dublin Core metadata or the HTML title and an author META element.

Library use
-----------

    require "html2tex"
    
    # Return output as a string
    tex = HTML2TeX.new(html).to_tex
    
    # Write directly to a stream
    File.open("output.tex", "w") do |f|
      HTML2TeX.new(html).to_tex(f)
    end

A hash of options can be supplied as the second argument to `to_tex`; the
available keys are:

* `:author`
* `:title`
* `:class`

See the command-line section above for an explanation of these.

Dependencies
------------

* htmlentities, to decode character entities
* rubypants, to convert `'` and `"` into something more appealing

Limitations
-----------

The HTML recognised is currently limited to headings, paragraphs, and bold and
italic mark-up. This covers the majority of novels, but it's far from
comprehensive.

StringScanner is used to process the HTML, but cannot read from a stream
directly, so the entire input document must be read into memory as a string
first.

UTF-8 is assumed everywhere; other character encodings will produce odd
results. If the HTML file to be processed is not in UTF-8 encoding with unix
line endings (at least, on Linux/OS X/etc.), _fix that first_. The usual
suspects will help here:

    iconv -f windows-1252 -t utf-8 < somefile-win1252.html > somefile-utf8.html
    dos2unix somefile-utf8.html

Next steps
----------

If you have XeLaTex, you can easily turn the generated `.tex` file into a PDF:

    xelatex my-book.tex

For better results, tweak the font settings or use a custom class like [this][ebook.cls].

[ebook.cls]: http://github.com/threedaymonk/gutenberg2pdf/blob/master/ebook.cls
