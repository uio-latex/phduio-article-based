# PHDUIO – Article based thesis
This LaTeX template is used for article based doctoral theses at the University of Oslo.
For monographic theses,
see the template [PHDUIO – Monograph](https://github.com/martinhelso/phduio-monograph).

Available on [Overleaf](https://www.overleaf.com/latex/templates/phduio-article-based-thesis/vnhkzpyyddvh).

The main features of the template are:
* Theses at the University of Oslo are printed in the book format 17 x 24 cm.
This is about 81% of an A4 paper.
In `phduio`,
you work directly in the 17 x 24 cm format,
so you do not have to worry about whether the font and figures will be legible after being shrunk for printing.
* It defines a custom title page.
* The class provides tools for incorporating separate papers into the thesis.

[![Example document](https://i.imgur.com/dGvvQkE.png)](https://www.duo.uio.no/bitstream/handle/10852/81172/PhD-Helsø-2020.pdf)

## Contents
* [Documentation](#documentation)
  * [Title page](#title-page)
    * [Colophon](#colophon)
  * [Screen mode](#screen-mode)
  * [Incorporating papers](#incorporating-papers)
    * [Including `.tex` files](#including-tex-files)
      * [Unmarked footnotes](#unmarked-footnotes)
      * [Tables of contents](#tables-of-contents)
      * [Appendices](#appendices)
      * [Bibliographies](#bibliographies)
    * [Including `.pdf`files](#including-pdf-files)
  * [Miscellaneous](#miscellaneous)
  * [Tips for converting to the `phduio` template](#tips-for-converting-to-the-phduio-template)
* [FAQ](#faq)
* [Contact information](#contact-information)

## Documentation
This text is about the specifics of the template.  
The more general [guide to large documents](https://github.com/martinhelso/Introduction-to-LaTeX/blob/master/large-documents.md)
is also considered part of the documentation.

At the core,
`phduio` consists of `memoir`,
so in addition to what is mentioned here,
you have all the functionality of `memoir`.

### Title page
A custom title page is printed when `\uiotitle` is invoked.
Just like `\maketitle`,
it collects data from `\author` and `\title`.
In addition,
you can add a subtitle using `\subtitle`.
Also,
you must specify your affiliation at the university with `\department` and `\faculty` before calling `\uiotitle`.

Optionally,
other affiliations can be added with `\affiliation`.
Use `\and` or `\AND` to separate the affiliations.
The use of `\and` is intended for separating subdepartments of a single institution:
```latex
\affiliation
{
    Center for Biomedical Computing
    \and
    Simula Research Laboratory
}
```
Whereas `\AND` is intended for separating two different institutions:
```latex
\affiliation
{
    Simula Research Laboratory
    \AND
    Oslo University Hospital
}
```

#### Colophon
If the class option `[colophon]` is used,
`\uiotitle` will also print a colophon page with copyright and printing information.
The colophon collects information from the commands `\author`, `\faculty`, `\dissertationseries` and `\ISSN`
or – if you belong to the Faculty of Medicine – `\ISBN`.

If needed,
the credits for cover design and printing can be changed with `\cover` and `\printinghouse`.

You can request the dissertation series number and ISSN/ISBN
by sending an e-mail to the University Print Centre at [repro@uio.no](mailto:repro@uio.no)
shortly before submitting the thesis.
They are also able to edit in the correct numbers directly into the `.pdf` for you.

### Screen mode
By default,
`phduio` is set up for printing.
If you want a version of the thesis that is more suitable for viewing on a screen,
pass the option `[screen]` to the document class.
This will colour clickable links and make the inner and outer margins equal.

### Incorporating papers
Mimicking the commands `\appendix` and `\appendixpage`,
the class `phduio` defines `\paper` and `\paperpage`.
The macro `\paper` renames the following chapters to papers,
and `\paperpage` prints a page with the text "Papers" in the style of `\part*{Papers}`.

The cross reference commands `\cref` and `\Cref` print "Paper" instead of "Chapter"
for chapters initiated after the `\paper` call.
If this does not work automatically,
it means that you are using an old version of `memoir` where `\memendofchapterhook` is undefined.
Then you have two options:
Either update your installation of `memoir`
(You can download the newest version from [CTAN](https://ctan.org/pkg/memoir?lang=en),
but you are probably better off updating your entire [TeX distribution](https://www.tug.org/texlive/)),
or  label the relevant chapters with `\label[paper]{key}` instead of `\label{key}`.

Even though an included paper is in reality a chapter,
you can use `\maketitle` combined with `\title`, `\author` and `\thanks`
instead of `\chapter` for the title of the article.
Moreover,
`phduio` provides `\metadata`.
It should be called *before* `\maketitle`.
The macro `\metadata` is used to specify any relevant information about the paper,
such as
```latex
\metadata{Published in...}
\metadata{To appear in...}
\metadata{Submitted to...}
\metadata{Preprint}
```

Sometimes the chapter title is too long for the running header.
In that case you have to use `\chapter` instead of `\maketitle`
to provide a shorter title for the header.
You can control the title in the header and the table of contents in one of the following two ways:
```latex
\chapter[TOC and running header]{Chapter page}
\chapter[TOC][Running header]{Chapter page}
```
Using this method,
you must use `\paperauthors` instead of `\author`.
Authors are separated with `\and` in the usual fashion.
If the list of authors does not break prettily across lines,
consider passing `\raggedright` along the argument to `\paperauthors`
or control the line breaks manually with `\par`:
```latex
\paperauthors
{
    \raggedright
    First Author
    \and
    ...
    \and
    Last Author
}
\paperauthors
{
    First Author
    \and
    Second Author
    \and\par
    Third Author
}
```

When using `\chapter` you have to add the thumb index manually with `\paperthumb`
*directly after* `\chapter` at the start of each paper.
Compile twice in order to position the thumb index correctly.
If the thesis consists of n papers,
the height of each thumb index should be `\paperheight` divded by n.
You can specify the number of papers by calling `\numberofpapers{n}`
*before* the first invocation of `\paperthumb`.

#### Including `.tex` files
It is notoriously difficult to combine the `.pdf` files of different papers into a thesis.
Not because it is a technical challenge,
but because of inconsistency in style and layout and – more importantly –
the thesis is printed on a smaller paper than many articles,
so they have to be shrunk in order to fit.
This can cause the text to have a size which is nearly impossible to read.
Therefore,
we recommend that you include the source code for the papers directly into the thesis whenever possible.

To do this,
we recommend that you change the document class of your paper `filename.tex` to `standalone`.
When you include the paper file into [`main.tex`](main.tex) with `\include{filename}`,
the preamble and the `document` environment in `filename.tex` is ignored.
If you have placed `\author`, `\title` and `\thanks` before `\begin{document}`,
make sure to move them out of the preamble.
All packages and custom macros used in the paper must also be declared in the preamble of the main thesis file.

##### Unmarked footnotes
Some document classes offer macros to print information
as unmarked footnotes on the front page of the article.
In particular,
`amsart` provides `subjclass` and `\keywords`.
Unmarked footnotes can be specified in `phduio` with `\papernote`.
To ensure that the footnotes appear on the first page of the paper,
issue `\papernote` *shortly after* `\maketitle`.

##### Tables of contents
The package `titletoc` lets you add a small table of contents for each chapter with
```latex
\chapter{Title}
\startcontents[chapters]
\printcontents[chapters]{}{1}{}
```
You can delimit the content with the commands `\stopcontents[chapters]` and `\resumecontents[chapters]`,
but typically it suffices to put in another call to `\startcontents[chapters]` at the start of the next chapter.

If you want subsections to be included in the partial table of contents, print it with
```latex
\printcontents[chapters]{}{1}{\setcounter{tocdepth}{2}}
```

##### Appendices
If you want appendices at the end of a chapter,
place them inside the environment `subappendices`.
If you put the declaration `\unnamedsubappendices` *before* the `subappendices` environment,
the subappendix number in the body of the document will *not* be preceded by the value of `\appendixname`.
That is,
only the section number will appear,
just as for a regular section.

If you are using the package `cleveref`,
label sections inside `subappendices` with `\label[appendix]{key}` instead of `\label{key}`,
so that `\cref` and `\Cref` correctly print "Appendix" instead of "Section".

```latex
\unnamedsubappendices
\begin{subappendices}
    \section{Title}
    \label[appendix]{key}
    ...
\end{subappendices}
```

##### Bibliographies
In order to have one bibliography for each paper,
`biblatex` is loaded with with the option `[refsection = chapter]` in [`phdstyle.sty`](phdstyle.sty).
This limits the scope of `\printbibliography` to the current chapter.
Alternatively,
if you only want to limit the scope for some chapters,
use the `refsection` environment for the relevant chapters:
```latex
\chapter{Title}
\begin{refsection}
    \cite{key}
    ...
    \printbibliography[heading = subbibliography]
\end{refsection}
```
The optional argument `[heading = subbibliography]` is there to print the bibliography as a section instead of a chapter.

You do not have to compile all the `.bib` files you used for the different papers into one file.
You can include all of them by repeated calls to `\addbibresource`:
```latex
\usepackage[refsection = chapter]{biblatex}
\addbibresource{file1.bib}
\addbibresource{file2.bib}
```

##### Including `.pdf` files
If for some reason you have to use the typeset `.pdf` of an article in your thesis,
or you do not want to adapt its `.tex` file,
then you can import the `.pdf` by passing the file name to `\includearticle`.
If necessary,
the article is shrunk in order to fit inside the thesis,
which can make the text difficult to read.
The article begins on the next right-hand page of the thesis.

By default,
`includearticle` prints the normal page numbers on top of the article,
but omits the running header.
If the page numbers in the thesis are in close proximity to the page numbers in the article,
you can lower the thesis page numbers by giving `\includearticle` the option `[numbers = low]`,
or remove them with `[numbers = none]`.

### Miscellaneous
The university's official colours are available under the names `uiored`, `uiogrey` and `uiolink`.
The colon from the university logo can be accessed with `\uiocolon`.

### Tips for converting to the `phduio` template
If you have written a chuck of your thesis prior to using `phduio`,
it can take some effort to convert the files to the template.
Here is some advise that can make the transition easier:
1. Change one file at the time.
Errors are expect at first,
and you can be overwhelmed at the number of warnings
if you try to convert all files at the same time.

2. Together,
`memoir` and [`phdstyle.sty`](phdstyle.sty) import
most of the normal packages.
Therefore,
to avoid chaos by importing the same packages multiple times,
you should start by not importing any additional packages.
Then check your error message for missing packages and undefined macros,
and add those until you have no more errors.
    
3. To get all the template specific code correct
and to avoid code specific for other classes,
you can copy text into one of the existing files in the template instead of modifying your own file.
    
4. Suppose you have written two papers that use the same macro name,
but the macros are defined differently.
Then you can redefine the macro with `\renewcommand` at the start of the second of those papers,
after the preamble.

5. Suppose that you have written two papers that use the same label.
This is not necessarily an issue,
but you should search and replace the label in one of the papers to be safe.

## FAQ
1. **How do I remove labels from the margin?**  
  Use the document class option [`[final]`](https://github.com/martinhelso/Introduction-to-LaTeX/blob/master/large-documents.md#draft-and-final-options):
  ```latex
  \documentclass[final]{phduio}
  ```

2. **How do I compile the bibliography?**  
The file [`phdstyle.sty`](phdstyle.sty) imports the package `biblatex` with the option `[backend = biber]`.
To compile the bibliography,
run `biber` on `main.bcf` or change to `[backend = bibtex]` and run `bibtex`.
In the latter case,
you must run `bibtex` on every `.aux` file created by [`refsection`](#bibliographies).

3. **How do I change the reference and citation style?**  
The file [`phdstyle.sty`](phdstyle.sty) imports the package `biblatex` with the option `[style = alphabetic]`.
Replace `alphabetic` with another [style name](https://www.overleaf.com/learn/latex/Biblatex_citation_styles).

4. **Why do I get an error saying `giveninits` is undefined?**  
The file [`phdstyle.sty`](phdstyle.sty) imports the package `biblatex` with the option `[giveninits = true]`.
The error indicates that you have installed an old version of `biblatex`.
The best solution is to update your TeX distribution to the latest version.
Alternatively, change `giveninits` to `firstinits`.

5. **Why is `\citet` and `\citep` not working?**  
These commands are not defined in `biblatex`;
their analogues are called `\cite` and `\parencite`,
respectively.
If you use an author-year citation style,
you can define `\citet` and `\citep` to issue their counterparts by adding the following to the preamble:
```latex
\let\citet\cite
\let\citep\parencite
```
If you are using a numerical citation style,
`\citet`behaves differently than `\cite`.
In this case,
add instead the following to the preamble:
```latex
\RequirePackage{xparse}
\NewDocumentCommand{\citet}{oom}
{
    \citeauthor{#3}~%
    \IfValueTF{#1}
    {%
        \IfValueTF{#2}
        {\cite[#1][#2]{#3}}
        {\cite[#1]{#3}}
    }
    {\cite{#3}}
}
\let\citep\parencite
```

6. **How can I use *fragile* macros inside `\title` or `\author`?**  
Add `\protect` before the fragile macro.

7. **Why is the paper number reset to I?**  
Most likely the previous paper initialises appendices with the global macro `\appendix`,
which resets the chapter counter.
Instead,
the appendices should be placed inside the local environment [`\begin{subappendices} ... \end{subappendices}`](#appendices).

8. **Why are some chapters preceded by a blank page?**  
By the formal [layout requirements](https://www.mn.uio.no/english/research/phd/thesis-adjudication/layout/),
chapters should start on a recto page.
A blank page is inserted if this does not occur naturally.

9. **Why is some text missing from included articlces?**
This issue occurs if the fonts in the article PDF are not embedded. You must ensure that all fonts in article PDF are available. The inclusion of all fonts from the PDF can be forced by setting `\pdfinclusioncopyfonts = 1`. Note that this will increase the file size of your thesis.

## Contact information
If you need further assistance with the template,
you may send an e-mail to [latexguru@ub.uio.no](mailto:latexguru@ub.uio.no).
