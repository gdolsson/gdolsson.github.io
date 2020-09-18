---
layout: post
title: Radial Insights
date: '2020-09-18 11:00'
---

Using the amber cpptraj (previously the ptraj) modules I have adapted some "lingo" referring to the calculated numerical density at distance x as "the integrated radial distribution function (RDF, g(x))". It turns out that while this value still presentes the numerical density within the given cutoff radius it is in fact not the integrated RDF (n(r)/G(x)). Running an RDF analysis for x and y produces the same RDF (g(r)) as y and x. This means that the functions are identical.

Doing a first maxima (regardless wether it is the true or a local one) identification produces the same maximum value at the exact same distance comparing the two RDFs, as would be expected. However, the calculated numerical density at radius x should be different when comparing x-y and y-x as these are not present in equal amounts. This is true as these values vary greatly. Based on this, referring to the numerical density as the "integrated RDF value" is not correct. An integration is included in the calculation though it also includes the numer of x and y included in the analyses to get the numerical density.

All in all, everything is more or less as one would expect though the terminology previously used should not be in used in the future rather stick with numerical density.
