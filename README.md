Parse LaTeX but not equations– a Lua filter
==================================================================

This is a fork from [tarleb's parse-latex filter](https://github.com/tarleb/parse-latex/) to make it work with the [mathjax3eqno quarto extension](https://github.com/ute/mathjax3eqno/). 


A filter to use when the input contains raw LaTeX that should be
included in other output formats. The filter uses pandoc's LaTeX
reader to parse raw snippets.

[CI badge]: https://img.shields.io/github/workflow/status/tarleb/parse-latex/CI?logo=github
[CI workflow]: https://github.com/tarleb/parse-latex/actions/workflows/ci.yaml

Functionality (from original)
------------------------------------------------------------------

The intended use for this filter are cases in which a Markdown
document contains LaTeX snippets that are not just formatting
additions, but a part of the content. Any raw LaTeX snippet, such
as `\textcolor{red}{lorem ipsum}`, will be parsed as LaTeX. The
result is then re-inserted into the document, replacing the
snippet. The above will yield `<span style="color: red">lorem
ipsum</span>` when converting to HTML.

The snippets will be passed through unchanged when converting to
LaTeX/PDF.

The filter is particularly useful with tables: it becomes possible
to use some of extra power of LaTeX, while still getting sensible
output with other formats. E.g.:

````
```{=latex}
\begin{tabular}{|l|l|}
 \hline
 one & two \\
 \hline
 three & four \\
 \hline
\end{tabular}
```
````

The PDF output will have horizontal and vertical table lines,
something that's otherwise difficult to accomplish with pandoc.¹

The filter uses pandoc's LaTeX parser, so if pandoc cannot parse a
LaTeX snippet, then neither can this filter.

¹ The reason for this is that vertical lines in tables are
  considered as ugly and bad style by most typographers.


Usage
------------------------------------------------------------------

This version of the filter is meant to be used with [`mathjax3eqno`](https://github.com/ute/mathjax3eqno/) in Quarto.

Add this filter to a project

    quarto add ute/parse-latex-noeq

and use it by adding `parse-latex` to the `filters` entry
in their YAML header.

Since it is meant to work together with `mathjax3eqno`, also add

    quarto add ute/mathjax3eqno
Use 
``` yaml
---
filters:
  - parse-latex-noeq
  - mathjax3eqno
---
```

Caveat
----------------------------------
This filter does not process references `\ref` anymore, since this is taken care of by mathjax, via filter `mathjax3eqno`. As a consequence, you can no longer refer to tables or headers the LaTeX way, but have to use quartos cross-referencing mechanism, however quarto is great for this purpose and allows preview on mouse-hover :-).


License
------------------------------------------------------------------

This pandoc Lua filter is published under the MIT license, see
file `LICENSE` for details.
