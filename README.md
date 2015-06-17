LaTeX2Exp
=========

*latex2exp* is an R function that parses and converts LaTeX math formulas to R's [plotmath expressions](http://stat.ethz.ch/R-manual/R-patched/library/grDevices/html/plotmath.html). Plotmath expressions are used to enter mathematical formulas and symbols to be rendered as text, axis labels, etc. throughout R's plotting system. I find plotmath expressions to be quite opaque and fiddly; LaTeX is a de-facto standard for mathematical expressions, so this script might be useful to others as well.

*Note that at the moment, this script is at very early stages. It /will/ fail for even very straightforward LaTeX formulas. It may improve in the future.*

Usage
-----

Clone this repository and source the script. The script's only dependence is the stringr package.

``` r
source("./latex2exp.r")
```

The `latex2exp` function takes a LaTeX string and returns a plotmath expression suitable for use in plotting, e.g.,

``` r
latex2exp('\\alpha^\\beta')
```

(note it is *always* necessary to escape the backslash, hence the double backslash).

The return value of latex2exp can be used anywhere a plotmath expression is accepted, including plot labels, legends, and text. For example:

``` r
x <- seq(0, 4, length.out=100)
alpha <- 1:5
plot(x, xlim=c(0, 4), ylim=c(0, 10), xlab='x', ylab=latex2exp('\\alpha  x^\\alpha\\text{, where }\\alpha \\in \\text{1:5}'), type='n')
for (a in alpha)
  lines(x, a*x^a, col=a)
legend('topleft', legend=latex2exp(sprintf("\\alpha = %d", alpha)), lwd=1, col=alpha)
```

![](README_files/figure-markdown_github/unnamed-chunk-3-1.png)

"Supported" LaTeX
-----------------

Only a subset of LaTeX is supported, and not 100% correctly. The following should be supported:

``` r
print(noquote(latex2exp.supported()))
```

    ##  [1] \\div        \\pm         \\neq        \\geq        \\leq       
    ##  [6] \\approx     \\sim        \\propto     \\equiv      \\cong      
    ## [11] \\in         \\notin      \\cdot       \\times      \\subset    
    ## [16] \\subseteq   \\nsubset    \\supset     \\supseteq   \\rightarrow
    ## [21] \\leftarrow  \\Rightarrow \\Leftarrow  \\sqrt       \\sum       
    ## [26] \\prod       \\int        \\frac       \\text       \\textbf    
    ## [31] \\textit     \\mathbf     \\mathit     \\mathrm     \\infty     
    ## [36] \\partial    \\cdots      \\ldots      \\degree     \\prime     
    ## [41] \\tilde      \\hat        \\widehat    \\widetilde  \\bar       
    ## [46] \\dot        \\underline  \\,          \\;

are supported. Their rendering depends on R's interpretation of the plotmath expression.

To render text with spaces or punctuation interspersed in the formula, embed it in `\\text{My text}`.

A few examples:

``` r
latex2exp.examples()
```

![](README_files/figure-markdown_github/unnamed-chunk-5-1.png)

FAQ
---

### It's not working even though my LaTeX string is correct

This script will get easily confused by even very simple LaTeX formulas (as I mentioned, it's a work in progress!). Please file a bug.

### The translation is incorrect/Why is this not a proper R package?

The script, at the present stage, is just a quick hack to let me use LaTeX formulas in my scripts.

At some point, I will rewrite this with more sound code and share it as an R package. Stay tuned, and feel free to file bugs.
