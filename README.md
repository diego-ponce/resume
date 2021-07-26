# Basic Rmarkdown Resume to HTML and PDF

In my professional life, I have become an enthusiastic advocate for a plain-text documentation environment, and have adopted Markdown and Rmarkdown as the basic file formats that I use to generate documentation. In my personal life, I have an archive of Word docs which get converted to pdf when I need a resume. This past weekend I decided to see what kind of simple workflow I could put together which would align my resume to my documentation standards.

## Requirements

- [Rstudio with Rmarkdown](https://rmarkdown.rstudio.com/)

```{r}
install.packages('rmarkdown')
```
- Other package dependencies to allow for dynamic content sourced in the `content.csv` and `contact.yaml` files:
```{r}
install.packages("magrittr")
install.packages("dplyr")
install.packages("glue")
install.packages("readr")
install.packages("purrr")
install.packages("yaml")
```
- [wkhtmltopdf](https://wkhtmltopdf.org/) to generate the PDF

## How to run

- Clone this repo
- You can generate a sample resume from the `.rmd` file in Rstudio by opening the file
- clicking the `knit` option at the top of the file.
- in the background, a pdf is generated using wkhtmltopdf in the same working directory as the rmarkdown and html files.
- if you run into errors, check to make sure that you can generate an Rmarkdown file and run wkhtmltopdf separately.

## Alternatives, inspiration, and design decisions

I was inspired by [pagedown](https://pagedown.rbind.io/) which provides a feature rich resume from Rmarkdown. There were a few issues that I wrestled with:

1. I prefer a rather bare resume, much closer to out-of-the-box Rmarkdown file
1. Both the `chrome_print` method and the 'using the browser' to PDF functionality converted my links to raw markdown `[friendly link](https://www.url.com)`. From the pagedown documentation, this is a feature not a bug since someone printing the PDF would need the fully qualified url to find the resource. In my case, I imagined those viewing my resume would be viewing it on a computer, so I wanted the links to be clickable. In the case in which they printed the resume, the links would likely provide enough information for my readers to find the correct pages.

Enter `wkhtmltopdf`! With a simple `$ wkhtmltopdf [OPTIONS] <resume_filename>.html <resume_filename>.pdf` I had a resume which used Rmarkdowns CSS, and retained clickable links.

To increase efficiency, I added a custom `knit` function to the Rmarkdown front matter YAML so that the `wkhtmltopdf` call was made when I knit the file.

Code for making the resume feed from a csv and generate formatted markdown is largely based off this extremely helpful blog on [building a data-driven cv with R](https://livefreeordichotomize.com/2019/09/04/building_a_data_driven_cv_with_r/)
