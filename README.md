Revana Installation and Usage Walkthrough with demo data
================
Elias Ulrich
December 5, 2022

This document will guide you through the installation of the *Revana* R
package and show you how to use it with the help of the demonstration
data provided.

📎 Find this walkthrough as HTML rendered R markdown [here](about:blank)
or as Github README
[here](https://github.com/KiTZ-Heidelberg/revana-demo-data)

📁 Find the package repository and documentation of Revana
[here](https://github.com/KiTZ-Heidelberg/Revana)

📊 Find the Revana’s HTML results report of our demonstration cohort
[here](https://kitz-heidelberg.github.io/revana-demo-report/report.html)

# Installation

## Requirements

Running and installing **Revana** requires a current installation of **R
(version 4.2.1 or greater)**. No dependencies/R packages need to be
installed in advance, as the installation process will provide the
required dependencies.

Also, please make sure your system has the software installed that R
requires for **package compilation**. View the R documentation of
*install.packages”* for details:

**For Windows users**

> \[To successfully install packages that need compilation\] on
> **Windows**, you will need to have installed the **Rtools collection**
> as described in the ‘R for Windows FAQ’ and you must have the ‘PATH’
> environment variable set up as required by Rtools.

For more details see

<https://cran.r-project.org/bin/windows/base/howto-R-4.2.html>

or

<https://cran.r-project.org/doc/manuals/R-admin.html#Windows-packages>

**For Linux/MacOS users**

> On **Unix-alikes**, when the package contains C/C++/Fortran code that
> needs compilation, suitable compilers and related tools need to be
> installed. On macOS you need to have installed the ‘Command-line tools
> for Xcode’ (see the ‘R Installation and Administration’ manual) and if
> needed by the package a Fortran compiler, and have them in your path.

<https://cran.r-project.org/doc/manuals/R-admin.html#macOS-packages>

or

<https://cran.r-project.org/doc/manuals/R-admin.html#Essential-and-useful-other-programs-under-a-Unix_002dalike>

:warning: **Attention users with Apple M1 devices** :warning:

Mac users with the new Apple M1 chip must use the Intel 64-bit build of
R, as Bioconductor does not currently support the Apple silicon-specific
arm64 build of R. Installation of Revana on the arm64 build of R will
fail due to incompability with Bioconductor dependencies.

See <http://bioconductor.org/news/bioc_3_15_release/> for details.

As R itself is usually being compiled with these tools upon its
installation, the required compiler software will already be available
on your system in most cases.

## Installer: “Devtools” R-package

**Revana** can be easily installed from inside the R console with the
help of “**Remotes**”. **Remotes** is a widely used R package that
provides an easy way to install other R packages from sources outside of
CRAN. We consider it the **most reliable and convenient way to install
Revana**, which is hosted on github.com.

*Additional information: Remotes is a more lightweight isolation of the
installation utilities provided by devtools. Therefore, we recommend it
for the installation of Revana. Nevertheless, if you have **devtools**
installed and are accustomed to its usage, feel free to install Revana
with its **install_github** function:
devtools::install_github(“<https://github.com/KiTZ-Heidelberg/revana>”)*

## How to install Revana

First install **remotes**, if you haven’t installed it already.

You can do this by executing the following code inside the R console:

``` r
if (!requireNamespace("remotes", quietly = TRUE))
  install.packages("remotes")
```

After that you can proceed with the **installation of Revana**.

Executing the following code inside the R console will install Revana
and all its dependencies:

``` r
remotes::install_github("https://github.com/KiTZ-Heidelberg/revana")
```

Revana relies on a large number of dependencies. While this is necessary
to provide you with its rich functionality, it does not come without any
caveats.

-   Depending on the number of previously installed packages, the
    download and installation process might take some time. You might
    want to grab a coffee while the installation proceeds.

-   While Revana **does not need to be compiled** throughout
    installation, some of its dependencies do. Depending on your system
    this compilation can take some time or even fail. Use the following
    steps to troubleshoot:

    -   Make sure you have the required compilers and software installed
        on your system. See **“Requirements”** above for details.

    -   Setting *options(pkgType = “both”)* makes R preferably install
        binary versions of the packages if available. Also, select to
        install binary versions of the packages, when prompted. This can
        greatly increase installation speed under Windows and MacOS
        systems. Unfortunately, this option is **not available for Linux
        distributions** and users will be forced to await package
        compilation.

    -   Please do not hesitate to contact us
        (<elias.ulrich@kitz-heidelberg.de>), if you are facing any
        problems throughout the installation of Revana.

# Demo Report

In the following, we will run you through a small demonstration. To view
the results of our **entire demonstration cohort** and see some of the
**more advanced features** in action, open
<https://kitz-heidelberg.github.io/revana-demo-report/report.html> .

# Running Revana with demo data

To make the usage of Revana as accessible as possible, this guide will
run you through its usage step by step. We provide you with a **small
sample of genomic data of patients with medulloblastoma** (group 3 and
4) and will use it to **demonstrate the workflow of Revana**.

First download the demo data from github:
<https://github.com/KiTZ-Heidelberg/revana-demo-data/raw/released/demo_data.zip>

Unpack the directory.

Revana uses a **paths.txt** file as input. This file is used to provide
the paths to your input data.

To help you understand how the paths.txt file can be created, we have
not provided it with the demo data. Instead we will create it manually.

To do this open an R session and execute the following code:

``` r
# insert the path to the unzipped folder containing the demo data here
# also: please use an absolute path starting with "/" (on Linux and MacOS)
demo_data_path <- "/path/to/demo/data/folder"

# this will create the required paths.txt files for medulloblastoma subgroups (Group 3 and Group 4)
for(GROUP_NAME in c("group_3","group_4")){

  # get the sample ids from the demo data
  sample_ids <- list.dirs(file.path(demo_data_path, GROUP_NAME), full.names = FALSE, recursive = FALSE)

  # create a data frame containing the paths
  paths <- data.frame(
    sample_id = sample_ids,
    marker_file =file.path(demo_data_path, GROUP_NAME, sample_ids, paste0(sample_ids, ".markers.txt")),
    CNA_file =file.path(demo_data_path, GROUP_NAME, sample_ids, paste0(sample_ids, ".CNA.txt")),
    somatic_SNV_file =file.path(demo_data_path, GROUP_NAME, sample_ids, paste0(sample_ids, ".somatic_SNV.txt")),
    expression_file =file.path(demo_data_path, GROUP_NAME, sample_ids, paste0(sample_ids, ".expression.txt")),
    SV_file =file.path(demo_data_path, GROUP_NAME, sample_ids, paste0(sample_ids, ".SVs.txt")),
    copy_number_file =file.path(demo_data_path, GROUP_NAME, sample_ids, paste0(sample_ids, ".copy_number.txt"))
  )

  # use readr (should be available after installing Revana!) to create a text file with the paths
  readr::write_tsv(paths, file.path(demo_data_path,GROUP_NAME,"paths.txt"))
}
```

*Side Note:*

*The function file.path merges its arguments into a path.* *For example:
file.path(“/path/to/my”,“custom”,“directory”) will output
“/path/to/my/custom/directory”*

**Now it is time to let Revana run its analysis!**

Please note, that in order to keep this demonstration as simple as
possible, we omitted some of the features and data integration
capabilities in this demonstration. To find out everything about the
more sophisticated features of Revana, please view the documentation.

Run the following code:

``` r
library(revana)

# !!! Please adjust the following paths !!!
# you can find the demo references in the subfolder "references" within the unzipped demo data folder

demo_data_path <- "/path/to/demo/data/folder"
demo_references_path <- file.path(demo_data_path, "references")


# you have to specify an output directory to store your result data
# in this case we create a output folder within the demo data folder

results_folder <- file.path(demo_data_path, "output")
dir.create(results_folder)


# Also, within that folder we create a directory for each analysed subgroup,
# just to keep everything tidy.

results_folder_group_3 <- file.path(results_folder, "group_3")
dir.create(results_folder_group_3)

results_folder_group_4 <- file.path(results_folder, "group_4")
dir.create(results_folder_group_4)



# run Revana for Group 3 (you can also loop over the groups like above)
GROUP_NAME <- "group_3"

run(
  # path to the created paths file
  paths_file_path = file.path(demo_data_path, GROUP_NAME, "paths.txt"),

  # output directory to store results
  output_dir = results_folder_group_3,

  # path to the gene annotation file
  gene_annotation_ref_file_path = file.path(demo_references_path, "gene_ref.txt"),

  # path to the exon annotation file
  gene_annotation_exons_ref_file_path = file.path(demo_references_path, "exon_ref.txt"),

  # path to the TAD file
  TAD_file_path = file.path(demo_references_path, "TADs.txt"),

  #name of the included subgroup or cohort in this case: "group_3"
  subgroup_name = GROUP_NAME
)




# run Revana for Group 4
GROUP_NAME <- "group_4"

run(
  # path to the created paths file
  paths_file_path = file.path(demo_data_path, GROUP_NAME, "paths.txt"),

  # output directory to store results
  output_dir = results_folder_group_4,

  # path to the gene annotation file
  gene_annotation_ref_file_path = file.path(demo_references_path, "gene_ref.txt"),

  # path to the exon annotation file
  gene_annotation_exons_ref_file_path = file.path(demo_references_path, "exon_ref.txt"),

  # path to the TAD file
  TAD_file_path = file.path(demo_references_path, "TADs.txt"),

  #name of the included subgroup or cohort in this case: "group_4"
  subgroup_name = GROUP_NAME
)
```

After analysing the demo data for regulatory variants, let’s **create
the summarizing HTML report**.

As you might have noticed our output directory contains many different
files, with all kind of result data. For each subgroup there is also a
file results.paths.txt file, that contains the paths to the result
files. It will serve as input for our report building function.

As we want to include the result data of medulloblastoma groups 3 and 4,
we will first have to merge the files.

``` r
group_3_results_file <- file.path(results_folder_group_3,"results.paths.txt")
group_4_results_file <- file.path(results_folder_group_4,"results.paths.txt")

all_groups_results_file <- file.path(results_folder,"all_groups.results.paths.txt")

merge_results_paths_files(
  list_of_results_paths_files = list(
    group_3_results_file,
    group_4_results_file
  ),

  # where the merged file will be stored
  # you can replace this line with any custom path
  output_path_merged_results_file = all_groups_results_file
)
```

To create the report, first create an output folder for it. This folder
will be portable as long as you don’t change its structure. Then run the
report builder:

``` r
# Path to the created folder - again: you can use any directory you desire
HTML_report_output_dir_path <- file.path(results_folder,"report")

# create the folder, as it doesn't exist yet
dir.create(HTML_report_output_dir_path)

# create the HTML report
create_HTML_report_new(
  # output directory, where the HTML report should be stored
  HTML_report_output_dir_path,
  # results paths file - see above
  output_paths_file_path = all_groups_results_file
)
```

That’s it! Now you should be able to view the report by opening the
created *report.html* file with an up to date version of your favorite
browser like Google Chrome.

To view the results of our entire demonstration cohort and see some of
the more advanced features in action, open
<https://kitz-heidelberg.github.io/revana-demo-report/report.html>
