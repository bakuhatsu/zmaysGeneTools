# zmaysGeneTools
Database of Zea mays gene identifiers, gene ontology information, gene symbols, etc, for use with R bioconductor.

This will give you `org.Zmays.eg.db`.

While this package will work with the default Rgui application that comes with R, it is highly recommended to use RStudio for data analysis in R.  Using RStudio will make things like saving output to vector images (like EPS), tiff, or PDF, effortless, and provide code completion, as well as a wealth of other functionalities.  RStudio is freely available for Windows, Mac, or Linux from [https://www.rstudio.com](https://www.rstudio.com). 

### Installing and loading the `zmaysGeneTools` package:
Installing through GitHub requires the `devtools` package, so instructions for installing and loading that package are provided below.
```r
## Install and load devtools for loading packages from GitHub
install.packages("devtools") # to allow us to install packages from GitHub
library(devtools)
```
Since GitHub packaged are compiled on your machine to run, you may be prompted to "install build tools" or something similar.  Follow the instructions, which will install the tools needed to compile this package.

`zmaysGeneTools` also relies on the `AnnotationDbi` and `S4Vectors` packages from Bioconductor. Bioconductor packages cannot be automatically installed like other R dependencies in an easy way, so you need to install them manually.  Install via the code below (if you do not already have bioconductor the first install takes a while, so be prepared).
```r
# try http:// if https:// URLs are not supported
source("https://bioconductor.org/biocLite.R")
## If you haven't previously installed Bioconductor, run the next line.
biocLite() # Installs Bioconductor base packages, will take a long time for a fresh install.  
## To install the ath1121501.db package
biocLite("AnnotationDbi")
biocLite("S4Vectors")
```
Now you should be ready to install and then load the `zmaysGeneTools` package
```r
## Installs org.Zmays.eg.db database of genome wide annotation for Zea mays
install_github("bakuhatsu/zmaysGeneTools") # syntax for installing from GitHub: username/library
library(org.Zmays.eg.db) # To load the package (This should be correct... 
# if the package is called zmaysGeneTools in RStudio and above doesn't work, then try: 
library(zmaysGeneTools)
```
That's it!  I may add further Zea mays genomic or transcriptomic tools to this repository in the future.  
