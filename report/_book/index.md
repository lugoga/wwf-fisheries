--- 
title: "RUMAKI Seascape"
subtitle: "Five Year Small Scale Fisheries Statistics (2014-2018)"
author: "Baraka Kuguru"
header-includes:
  - \usepackage{titling}
  - \pretitle{\begin{center}
    \includegraphics[width=6in,height=6in]{front.pdf}\LARGE\\}
  - \usepackage{float}
  - \floatplacement{figure}{H}  ##make every figure with caption = h, this was the fix
  - \renewcommand{\topfraction}{.85}
  - \renewcommand{\bottomfraction}{.7}
  - \renewcommand{\textfraction}{.15}
  - \renewcommand{\floatpagefraction}{.66}
  - \setcounter{topnumber}{3}
  - \setcounter{bottomnumber}{3}
  - \setcounter{totalnumber}{4}
  - \usepackage{fontspec}
  - \setmainfont{Adobe Caslon Pro} ## set font to Times New Roman
  # - \setlength\abovecaptionskip{-20pt}
  # - \usepackage{fancyhdr}
  # - \pagestyle{fancy}
  # - \fancyhead[LO,RE]{Kuguru B.}
  - \usepackage[utf8]{inputenc}
  - \hypersetup{unicode=true,pdfusetitle,bookmarks=true,bookmarksnumbered=true,bookmarksopen=true,bookmarksopenlevel=2,breaklinks=false,backref=false,colorlinks=true,linkcolor=blue}
# date: paste0("Last updated: 12 June, 2020")
date: "June 2020"
site: bookdown::bookdown_site
documentclass: book
classoption: oneside
bibliography: [biblio.bib, packages.bib]
biblio-style: apalike # elsevier.csl
link-citations: yes
linkcolor: NavyBlue
links-as-notes: true
papersize: a4
fontsize: 12pt
linestretch: 1.2
geometry: "left=2.5cm, right=2.5cm, top=2.5cm, bottom=2.5cm"
# favicon: "introduction/favicon.ico"
# cover-image: "introduction/data_science_live_book_cover.png"
description: ""
always_allow_html: true
lof: true
lot: true
toc: true
toc_depth: 3
---

# Executive Summary {-}
It is widely recognized that knowledge of the status and trends of capture fisheries, including socio--economic aspects, is a key to sound policy--development, better decision--making and responsible fisheries management. For the fisheries to contribute to food security it is important that information related to a particular stock or entire fisheries is available on an instant basis. Countries in coastal East Africa have taken up a scheme to decentralized management of the fisheries. These countries have introduced fisheries co--management systems---approach to manage coastal and marine resources. It was anticipated that participation of BMUs in the data collection would improve coverage and timely collection of the fisheries data.\index{fisheries!statistics}

In 2016, TAFIRI in collaboration with WWF conducted a pilot study to explore the use of the mobile application as a tool in fisheries data collection (e--CAS). The initiative started with  five (5) selected BMUs. Members were trained on the use of mobile application in fisheries data collection. It was through this initiative species of tuna and tuna-like were recorded for the first time [@kuguru]. As of today, the initiative was expanded to to all coastal districts landing sites in Mainland Tanzania and endorsed by the Government. The current assignment aimed to analyze data collected from the e-CAS and regular CAS survey collected from 2014 to 2018 to assess stock healthiness and quality of small-scale fisheries landings in Rufiji \index{RUMAKI!Rufiji}, Mafia \index{RUMAKI!Mafia}, and Kilwa \index{RUMAKI!Kilwa} districts (RUMAKI). \index{RUMAKI} 

The status of fish stock healthiness in the RUMAKI and Non--RUMAKI districts was assessed based on the data from CAS spanning from 2014 to 2018 and additional data from the e--CAS \index{Tools!e--CAS} spanning from 2016 to 2019 respectively. Fisheries catch\index{fisheries!catch} variation spatial maps, total catch and catch trends  of  the six priority fishery categories were used to assess the status of fish stock healthiness in the RUMAKI districts. The report focus on major five priority fisheries groups due the reasons that there was inconsistent in the data collection in terms of specific species or particular family groups. The groups were drawn from the ongoing SWIOFISH project. These include *Octopus*\index{Family!octopus}, *Small pelagics*\index{Family!pelagics}, *tuna and tuna* like species\index{Family!tuna}, *Reef* \index{Family!reef} and *Elasmobranch*\index{Family!elasmobrach} species.\index{Non--RUMAKI}


The average annual total catch for the octopus fishery was found to be 124.83 $\pm$ 16.33 tones per year in the non-RUMAKI and 173.90 $\pm$ 20.77 (*Mean $\pm$ SD) tones per year in the RUMAKI districts (Appendix \@ref(tab:tab071)); the district of Kilwa and Ilala had higher landings in the RUMAKI and no-RUMAKI respectively (Appendix \@ref(tab:tab072)). For tuna and tuna-like, the average total annual catch for the tuna and tuna-like species was found to be 74.85 $\pm$ 8.28 tones per year in the non-RUMAKI and 65.55 $\pm$ 10.51 tones per year in the RUMAKI districts (Appendix \@ref(tab:tab071). The Ilala district had higher landings in Non--RUMAKI and  in the RUMAKI and Mafia had the highest catch in RUMAKI (Appendix \@ref(tab:tab072)). The average annual total catch for the Small pelagic fishery was found to be 153.25 $\pm$ 17.32 tones per year in the non-RUMAKI and 121.21 $\pm$ 20.82 tones per year in the RUMAKI districts; the Mkuranga and Mtwara rural districts had higher landings in the RUMAKI and no-RUMAKI respectively (Appendix \@ref(tab:tab072)). In addition, for reef species the average annual landing was found to be 113.83 $\pm$ 17.93 tones per year in the non-RUMAKI and 102.87 $\pm$ 21.85 tones per year in the RUMAKI districts (Appendix \@ref(tab:tab071)); the Mafia and Pangani districts had higher landings in the RUMAKI and no-RUMAKI respectively (Appendix \@ref(tab:tab072)). 

<!-- Finally, for elasmobranch, annual average catch was (xx-xx tones per year) in the non-RUMAKI and (xx-xx tones per year) in the RUMAKI districts; the district of xx and xx had higher landings in the RUMAKI and no-RUMAKI respectively.  -->

In general, findings indicate there is a decrease in total catch from artisanal fishers in the RUMAKI districts for all major fisheries groups with the exception of small pelagic based on the data collected by BMUs from 2016-2019. Data collection is improving, in terms of timely submission of the data, but there is inconsistency in the data collection and little focus on the species of interest. 

Among others, the current report recommended the following: The Department of Fisheries (DoF) \index{Institutes!Department of Fisheries} in collaboration with TAFIRI \index{Institutes!TAFIRI} should consider identifying representative species for all priority (major) fisheries groups and consistency follow up the data collection on the identified species. The DoF should consider adapting and upscaling the eCAS as the main data collection tool. Promote timely preparation of fishery statistics through the application of the databases. This should go along with the timely sharing of the statistics with relevant stakeholders.  In addition, promote capacity development to all levels in the area of data collection, processing, analysis, interpretation and reporting. This should go hand in hand with periodic strategic planning/system review of marine fisheries as data collection should be in a position to answer some of the management questions of the time.

The collection and analysis of fishery data and information is a costly and timely exercise. To be relevant and cost-effective, fishery data and information collection systems must have a clear set of objectives and appropriate strategies to collect data, which should be based on priorities and requirements of data users. However, chronic problems of insufficient human and financial resources allocated for data collection often resulted in the poor quality of information that further led to no- or limited use of statistics for fishery management and policy development. Consequently only dwindling support was given to the systematic improvement of national fishery data and information collection systems. There is an urgent need to terminate this vicious cycle of problems. The previous budget allocated by the Government for data collection during CAS\index{Tools!CAS} should be more than enough if re-allocated to the used in the improved e--CAS\index{Tools!e--CAS} data collection system.

\newpage

## Acknowledgements {-}

This work was supported by many individuals from various institutions.  Thank you to the Department of Fisheries, Tanzania Fisheries Research Institute, Fisheries Officers along the coastal Districts, BMU et.... We also extend our gratitude to the WWF-Tanzania for financial support to conduct this research. 

Thank you to Mathias Igulu for his time to review and edit the book. We appreciated [Semba’s blog](https://semba-blog.netlify.app/) for the materials that helped to prepare the report. We also extend our gratitude to Masumbuko Semba for analyzing the data and typeset and plots for used for this document. 

The report is written in [RMarkdown](https://rmarkdown.rstudio.com) with [bookdown](https://bookdown.org). It is automatically rebuilt from [source](https://github.com/hadley/r4ds) by [travis](http://travis-ci.org/).

\newpage

## How this Report is organised {-}

The previous description of the tools of data science is organised roughly according to the order in which you use them in an analysis (although of course you'll iterate through them multiple times). In our experience, however, this is not the best way to learn them:

* Starting with data ingest and tidying is sub-optimal because 80% of the time 
  it's routine and boring, and the other 20% of the time it's weird and
  frustrating. That's a bad place to start learning a new subject! Instead, 
  we'll start with visualisation and transformation of data that's already been
  imported and tidied. That way, when you ingest and tidy your own data, your
  motivation will stay high because you know the pain is worth it.
  
* Some topics are best explained with other tools. For example, we believe that
  it's easier to understand how models work if you already know about 
  visualisation, tidy data, and programming.
  
* Programming tools are not necessarily interesting in their own right, 
  but do allow you to tackle considerably more challenging problems. We'll
  give you a selection of programming tools in the middle of the book, and 
  then you'll see how they can combine with the data science tools to tackle 
  interesting modelling problems.

Within each chapter, we try and stick to a similar pattern: start with some motivating examples so you can see the bigger picture, and then dive into the details. Each section of the book is paired with exercises to help you practice what you've learned. While it's tempting to skip the exercises, there's no better way to learn than practicing on real problems.



## Citation {-}

If you would like to cite this book, please use the below:

> Baraka Kuguru and Innocent Sailale (2020). *Analysis of fisheries data collected from small scale fishers with focus on RUMAKI seascape*. Dar es Salaam, Tanzania. 





