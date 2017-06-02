# PubListConverter
A set of scripts that use Bib2Bib and Bibtex2HTML to produce neatly formatted HTML reference lists that are organized based on metadata.
This project was created largely to simplify creating academic websites with links to full texts, hopefully you will find it helpful too. Examples of the resulting output can be found at my [PIxL Lab Publications Page](https://pixl.nmsu.edu/publications).

## Capabilities:

Starting with a BibTeX file of publications, the user can create a bibliography HTML page with links to:

1. ACM Digital Library Authorizer link
1. local full text
1. local poster file
1. raw BibTeX

The scripts can produce a list of references, according to a format you specify. Each reference ends with a set of access links, e.g.:

> [ bib | doi | full text via authorizer ]

Publications can also be categorized, facilitating, for example, having a single page (which is necessary for ACM Authorizer) that includes a full publication list, then linkable sub lists based on project or topic. 

## Requirements:

These bash scripts are intended to be run at the Unix command line. The following need to be installed:

1. bib2bib
1. bibtex2html

## Using the Scripts

### Necessary Environment

There are a few prerequisites to make use of the scripts. 

1. The setup assumes you will have one or more bibliography pages and that these can link to a separate page that will serve to provide raw BibTeX data. The BibTeX page contents will also be generated as part of execution, but the URL where they will reside is needed in advance.
1. You will need a .BST file to drive the creation of the references page. Most fields already have one that is in wide use.

### Optional Modifications to Source BibTeX

By default, the scripts can generate references with links to a separate BibTeX page (described above). In addition, you can provide access links and projects (or categories).

#### Access Links

The scripts recognize and automatically include several types of access links. These will appear after each reference in square brackets, separated by pipes. You could modify the scripts to change this behavior, if desired. The [ bib ] link will appear automatically, linking to the appropriate location in the separate BibTeX page. In addition, if provided as BibTeX fields, the following will automatically be generated as links:

1. `Doi` will produce a link that says `[ doi ]` and should include the document's object identifier; a link via [doi.org](http://dx.doi.org) will be generated.
1. `Local-Full-Text` will produce a link that says `[ full text ]` and should be a complete URL pointing to where the full text of the paper is located.
1. `Local-Poster` will produce a link that says `[ poster ]` and should be a complete URL pointing to where the appropriate poster is located.
1. `Authorizer` will produce a link that says `[ full text via authorizer ]` and should be the ACM-provided Authorizer link for the paper. Note that the page that contains the link *must* have been configured via the [ACM Author-Izer Service](http://www.acm.org/publications/authors/acm-author-izer-service) in advance, or the link will break.

#### Automatic Sub Project Sections

The scripts can also produce separate lists of sub projects, based on the contents of the BibTeX file. By default, these are appended to the main output HTML file after the complete list (facilitating having a single page that links to the ACM Author-Izer, but that has project-specific sections). In addition, sub project HTML files are produced.

To make use of this functionality, you need to provide a space-separated list of project identifiers in each entry of your BibTeX file with the field `Projects`. Each paper may belong to multiple projects. The project identifiers are used to set anchors in the HTML to the appropriate section and name the sub project files. They need not be human readable (and cannot contain spaces).

### Running the Converter Scripts

To run the scripts, after making the modifications above, simply run:

`./mainPubsGen.sh <source BibTeX file> <BST file> <URL that will hold the BibTeX page, in quotes, with all forward slashes escaped> (pairs of project identifiers and human-readable project identifiers in quotes)`

If everything works correctly, you should get the following files:

1. `mainPubs.html` - the main publications list with all publications and then sub lists of all sub projects (with headers that include anchors). Normally, the contents of this file can be copied and pasted into a website (note that this file only contains the HTML contents, which makes it suitable for dropping into WordPress or similar sites).
1. `mainPubs_bib.html` - a page that contains the BibTeX for each entry on the page above, with headers that include anchors based on cite key. This page should be copied to the URL specified as the separate BibTeX page.
1. A series of sub-project HTML pages, each named `<project identifier>SubPubs.html`. These are contatenated at the end of `mainPubs.html`, but could be used on their own. Note that Author-Izer links will only work if the page was previously authorized!
