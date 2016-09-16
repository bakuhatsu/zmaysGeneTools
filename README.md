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

## Usage:
Here is a quick example of how you can use this database to convert from gene names of the type `GRMZM2G089713` to other information like GO terms, gene descriptions, etc.

```r
columns(org.Zmays.eg.db)
# [1] "ACCNUM"      "ALIAS"       "CHR"         "ENTREZID"    "EVIDENCE"    "EVIDENCEALL"
# [7] "GENENAME"    "GID"         "GO"          "GOALL"       "ONTOLOGY"    "ONTOLOGYALL"
# [13] "PMID"        "REFSEQ"      "SYMBOL"      "UNIGENE"

# random example for the gene with key = 100277953
select(x = org.Zmays.eg.db, keys = "100277953", columns = c("ACCNUM", "ALIAS", "GENENAME"))

# Make a list of your own genes of interest
myGenes = c("GRMZM2G024993","GRMZM2G089713","GRMZM2G005066","GRMZM2G016241","GRMZM2G133398") 
# You can enter your own genes here or read in a dataset and pass the genes as a vector

# This time, save the result to a dataframe for easy viewing or exporting to csv/excel
exampleSet <- select(
        x = org.Zmays.eg.db, 
        keys = myGenes, 
        columns = c("ALIAS", "GENENAME","GO", "ONTOLOGY", "SYMBOL", "UNIGENE"), 
        keytype = "ALIAS")
```
You may notice that if you have certain columns like `GO` where there are more than 1 GO term for a given gene, that you get an output message that says "mapped 1:many" or something similer.  You will then see in your dataframe that you have multiple rows for the same gene, each corresponding to a different GO term for that gene.  To collape these rows together and put all GO terms (works for any of the columns) into the same "cell" seperated by columns, use the code below (modify based on the columns that you chose above).  

```r
library(plyr) # Install plyr if you don't already have it installed
exampleSet <- ddply(exampleSet, "ALIAS", summarize, 
      GENENAME = paste(GENENAME, collapse = ", "),
      GO = paste(GO, collapse = ", "),
      ONTOLOGY = paste(ONTOLOGY, collapse = ", "),
      SYMBOL = paste(SYMBOL, collapse = ", "),
      UNIGENE = paste(UNIGENE, collapse = ", ")
```


Good luck!

## Troubleshooting tip:

If you are getting an error from your `select()` call, you may have another package loaded that also has a `select()` function and R is trying to use the wrong one.  To specify that you use the correct select function from the `AnnotationDBi` package, use code in the form of `AnnotationDbi::select()` when you make your function calls.  So the code above would look like: 

```r
...
# random example for the gene with key = 100277953
AnnotationDbi::select(x = org.Zmays.eg.db, keys = "100277953", columns = c("ACCNUM", "ALIAS", "GENENAME"))
...
# This time, save the result to a dataframe for easy viewing or exporting to csv/excel
exampleSet <- AnnotationDbi::select(
        x = org.Zmays.eg.db, 
        keys = myGenes, 
        columns = c("ALIAS", "GENENAME","GO", "ONTOLOGY", "SYMBOL", "UNIGENE"), 
        keytype = "ALIAS")
```
