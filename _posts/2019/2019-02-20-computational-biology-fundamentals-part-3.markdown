---
layout: "post"
title: "CompBio Fundamentals part3"
date: "2019-02-20 09:25"
---
Today I am attending the third instalment (realising I'm missing parts 1 and 2) of the Computational Biology Fundamentals which is a seminar series that I am attending. It is extremely confusing trying to keep up with the biology part, it has been a long while since I did any work with biological systems.

Nevertheless, it is interesting to learn new things and at least the technological part is the same, the implementation.

So far I have been exposed to one new and exciting tool [QIIME2](https://qiime2.org/) (and the older alternative [QIIME](https://qiime.org)?) which I will bee looking closer at when I have time.

So in order to use these tools to determine what you have in environmental samples (since seemingly people attending just get a big batch of sample and have to figure out what's in the sample using sequences) you need to have databases to compare against. These are the ones that have been discussed so far:

* [silva](https://www.arb-silva.de/) High quality ribosomal RNA database
* [RDP](http://rdp.cme.msu.edu/) Ribosomal Database Project
* [GreenGenes](http://greengenes.secondgenome.com/) The 16S rRNA Gene database and Tools

There was a paper recommendation regarding [RDP](https://dx.doi.org/10.1128/AEM.00062-07).

Choosing database depends on both update frequency and taxonomy model. Apparently there are issues with the [NCBI](https://www.ncbi.nlm.nih.gov/taxonomy) taxonomy and hence moving towards using the [GTDB](http://gtdb.ecogenomic.org/) (genome taxonomy database) which has a new model for dealing with divergency that better treats divergent species. This is apparently dealt with in another [paper recommendation](https://dx.doi.org/10.1038/nbt.4229).

Apparently, the classification is a problem because dividing family trees for species with different developmental frequency producing different results. Hence [taxonomic rank](https://en.wikipedia.org/wiki/Taxonomic_rank) is causing problems. This has been addressed in [another paper recommended](https://dx.doi.org/10.1371/journal.pcbi.1003531)

It seems that pooling samples and weighting proportions is a big problem. One piece of takeaway is that you have to compare your alternatives to not pollute/affect data while also not increasing the demand on resources without getting any payoff in return. Another paper discussing rarefication with regards to alpha-diversity was [suggested and discussed](https://dx.doi.org/10.1101/231878) where differences stemming from different sequencing depths is suggested to be obscured when looking at environment after statistical treatment. What is suggested is that looking at both alpha-diversity and rarefying can obscure differences stemming from sequencing depth, hence a new method is suggested. We will apparently be exposed to this method today. There is supposed to be a a package available for the "Willis" method that is called "breakaway", I suppose I'll find it.

Even though I find the entire concept confusing it is obvious that computational biology and computational chemistry suffers from similar issues. Here it is methods for treating and weighting samples, for me it is weighting and comparing parameters, forcefields and softwares.

It turns out that using the "Willies" package "breakaway" is not completely trivial, as demonstrated after the lunch break. I did however pick up some new things I had not yet figured out or remembered regarding R.

It turns out that "%>%" is a "R-pipe" as compared to ("|") and when using ggplot then "+" is the pipe symbol.

I need to read more however using a non-metric [multidimensional scaling](https://en.wikipedia.org/wiki/Multidimensional_scaling) (nMDS) analysis is apparently one of the first things one do?

I might update this post later if something additionally interesting comes up.
