---
title: Betancur-R interactive fish phylogeny in anvi'o
subtitle: Coming Soon
authors: [Jarrod J Scott]
summary: Coming Soon
tags: ["fish", "16s rRNA", "microbiome", "intestinal", "Bocas del Toro", "coral reefs"]
categories: []
date: "2020-04-05T00:00:00+01:00"
external_link:
bibliography: [cite.bib]
header:
  caption: ""
  image: "featured.jpg"
image:
  caption: ""
  focal_point:
  preview_only: true
links:
- name: Go to Project Site
  url: https://bocasbiome.github.io/
- icon: database
  icon_pack: fas
  name: Data
#  url: https://projectdigest.github.io/data_availability.html
- icon: code
  icon_pack: fas
  name: Code
#  url: https://projectdigest.github.io/raw_code.txt
- icon: github
  icon_pack: fab
  name: GitHub
  url: https://github.com/bocasbiome/web/
- icon: newspaper
  icon_pack: fas
  name: Publication
  url: https://doi.org/10.1101/2020.09.21.306712
- name: Authors
#  url: https://projectdigest.github.io/people.html
publication: [clever-etal-2020]
slides:
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

weight: 20
---

As part of an analysis for [another project](project/projectdigest/), I needed a phylogenetic tree of five fish species. To construct the tree I also needed an appropriate out group but given that I know little about fish, I wasn't sure which species would be most appropriate. My search for an out group lead to [DeepFin](https://sites.google.com/site/guilleorti/) and the amazing paper by [Betancur-R et. al., (2017)](https://link.springer.com/article/10.1186/s12862-017-0958-3) about the phylogenetic classification of bony fishes. 

Anyway, I used the tree from the paper to choose Gerreidae as the out group for the analysis. Then, just for the fun of it, I decided to make my own interactive representation of the Betancur-R et. al., tree. How I did this and what the tree looks like is the subject of this post. 

<br/>

{{% alert synopsis %}}

Workflow overview
<hr>
1) Retrieve Betancur-R tree data. Modify the data to be compatible with anvi'o. <br/> 
2) Scrape FishBase for metadata.  <br/>
3) Curate metadata.  <br/>

{{% /alert %}}

<hr/>

# Modifying the Betancur-R Data

The Betancur-R bony fish phylogeny from the paper is the 4th version (the original dates back to 2013) and contains 1992 species encompassing 72 orders and 410 families. Wow. At the time of publication this tree encompassed roughly 80% of recognized bony fish families. The study uses molecular and genomic data to base classification on inferred phylogenies. From the abstract the author states:

*The first explicit phylogenetic classification of bony fishes was published in 2013, based on a comprehensive molecular phylogeny (www.deepfin.org). We here update the first version of that classification by incorporating the most recent phylogenetic results.*

The authors provide many files with the paper but for the purposes of this visualization I used two files from the [Supplementary material](https://link.springer.com/article/10.1186/s12862-017-0958-3#SupplementaryMaterial), specifically **Additional file 2** ([complete tree in newick format](https://static-content.springer.com/esm/art%3A10.1186%2Fs12862-017-0958-3/MediaObjects/12862_2017_958_MOESM2_ESM.tre)) and **Additional file 4** (Table S1. spreadsheet with full classification). 

I first modified the tree file slightly---in the original file, the Family name was used as a prefix for each species. I removed the Family names from all leaves. To do this you could of course use the command line but I prefer using the [BBEdit](http://www.barebones.com/products/bbedit/index.html) text editor because it allows grep pattern matching and I can see what is being replaced before I do anything. The free version of BBEdit has this functionality but if you like the software I recommend purchacing a lisence. Anyway, this simple regular expression will relace all the Family names in the tree. 

`[A-Z]\w+ae_`

Basically, the command says look for a capital letter (`[A-Z]`), followed by any word character (`\w`) any number of times (`+`), then the letters `ae`, and finally an underscore (`_`). The `ae` is important because all of the Family names end in `ae`. 

ADD DETAILS ABOUT Additional file 4 

# Scraping FishBase

The next step was to collect metadata about each fish to include in the visualization. [FishBase](https://www.fishbase.se/search.php) is a huge online data base and as of this writing had information on over 34,000 fish species. Here is an example page about [*Herichthys minckleyi* ](https://www.fishbase.se/summary/Herichthys-minckleyi.html) or Minkley's cichlid, a freshwater fish endemic to Cuatro Cienegas, Mexico. There is a lot of  metadata on these pages but I was specifically interested in the following pieces:

1) the scientific and common names.
2) the Environment metadata.
3) the Size / Weight / Age metadata.
4) the IUCN Red List Status.
5) the Threat to humans.
6) the Human uses.
7) the Distribution metadata. *Note* this is retreived separately. 

Given the size of the tree, doing all of this by hand was simply out of the question. So I had to learn a little about [web scraping](https://en.wikipedia.org/wiki/Web_scraping). I will not go into the details of webscraping here but basically I used HTML parsing, where specific sequences of HTML tags are used to parse information. The great thing about FishBase is that species pages follow the same general format, making it easier to grab the info you want. Ultimitely, how you code a webscrape is highly dependent on structure of the page and the info you want. If you are interested in webscraping there are a ton of great resouces and courses. Here are a few I relied on heavily.

- [Web crawling and scraping](https://tm4ss.github.io/docs/Tutorial_1_Web_scraping.html) from a tutorial on *Text mining in R for the social sciences and digital humanities* by Andreas Niekler, Gregor Wiedemann. 
- [Web Scraping Reference: Cheat Sheet for Web Scraping using R](https://github.com/yusuzech/r-web-scraping-cheat-sheet) by [yifyan](https://github.com/yusuzech). 
- [R Web Scraping Cheat Sheet](https://awesomeopensource.com/project/yusuzech/r-web-scraping-cheat-sheet) from Awesome Open Source. 
- [Functions with R and rvest: A Laymen’s Guide](https://towardsdatascience.com/functions-with-r-and-rvest-a-laymens-guide-acda42325a77) by [@peterjgensler](https://medium.com/@peterjgensler). This artcile was key in helping me design the functions described below. 

## Workflow

This workflow uses [`rvest`](https://www.rdocumentation.org/packages/rvest/versions/0.3.6), an R package for scraping web pages. I will keep the explaination here to a minimum since there are many authoratative sources online.  

The first thing to do is load the required packages. 

```{r, eval=FALSE}
library(tidyverse)
library(webdriver)
library(magrittr) 
library(dplyr) 
library(rvest) 
library(xml2) 
library(selectr) 
library(tibble)
library(purrr) 
library(httr)
```

### Scrape Species Pages

Moving on. I found out after I ran everything that dealing with the Distribution metadata my scripts parsed from the web pages was huge pain. The amount of manual manipulation I was doing was unacceptable. So the first thing I did was to grab all of the other metadata before returning to the Distribution data. This does make the code longer but it also makes data cleanup at the end a lot easier. 

The first thing I need are URLs to scrape. FishBase being the awesome resourse that it is has a very simple URL structure, for example:

`https://www.fishbase.se/summary/Herichthys-minckleyi.html`

Every species follows the same format. The only file I needed to start was a list of all species in the tree so I grabbed the names from my modifed Betancur-R tree file. The only change I had to make was swapping the underscore (`_`) in the tree names for a dash (`-`). 

From that list of species names I made a data frame containing each species name and the corresponding FishBase URL. This code reads in the file and writes a complete URL for each species.

```{r}
rm(list = ls())
base_url <- "https://www.fishbase.se/summary/"
fish <- scan('fish_list.txt', what = "character", sep = "\n")
rm(list = ls(pattern = "sample_data"))
sample_data <- data.frame()
for (name in fish) {
     tmp_link <- paste(base_url, name, ".html", sep = "")
     tmp_entry <- cbind(name, tmp_link)
     sample_data <- rbind(sample_data, tmp_entry)
     rm(list = ls(pattern = "tmp_"))

}
sample_data <- sample_data %>% dplyr::rename("link" = "tmp_link")
```

Now I have a two-column dataframe with the species name and FishBase URL.

```{r}
all_names <- sample_data$name
all_links <- sample_data$link
```

To understand what this next step is about, please read the [Web crawling and scraping](https://tm4ss.github.io/docs/Tutorial_1_Web_scraping.html) tutorial. 

```{r}
pjs_instance <- run_phantomjs()
pjs_session <- Session$new(port = pjs_instance$port)
```

Now it was time to write my scrape function, called simply `scrape_fish_base`. Depending on the type of information you are trying to scrape, the code can be pretty simple or a little more complicated. The key is to use specific enough [XPATHs](https://en.wikipedia.org/wiki/XPath) to select the information you are interested in and *only* that information. I am learning that scrapping is a bit of an art form and it takes a little practice to understand how to code things effectively. I practiced a lot on a small subset of the fish species until I got just the right `XPATHs`. I strongly suggest you work through a tutorial like [Scrape This Site](https://scrapethissite.com/) before attempting a more complicated projects. 

I won't try to explain what each `XPATH` code means but I will show you how I found the right ones. Let's use our old friend [*Herichthys minckleyi*](https://www.fishbase.se/summary/Herichthys-minckleyi.html) as an example. When you open up this page you need to find the Developer Tools of your browser, the location of which is slightly different depending on your browser. In Chrome for example, you go to `View > Developer > Inspect Elements`. When you do this, a new window  pops up showing the Developer Tools. Use your cursor to find the element of interest, right click, and hit `Copy XPath`. You need to be in the `Elements` tab when you do this. I have found in some cases using the `Selector` instead works better. I am not sure why that is but keep it in mind.




Let's say I am interested in the Environments where *Herichthys minckleyi* is typically found. I run through these steps and end up with this `XPATH`:

> `//*[@id="ss-main"]/div[2]/span` 

I  use this code to select the information I need and then repeat the process for the rest of the metadata. At the end I have a collection of `XPATHS` and `Selectors` that I can add to my function. I found that `XPATHS` worked best for scientific name, common name, environment, human uses, and size / weight / age metadata while `Selector` worked better for IUCN Red List status and threats to humans. In addition, the human uses metadata was a little tricky because on some pages it was one `XPATH` while on other pages it was a different `XPATH`. So I had to code the threats to humans twice. Once I have the data I simply combine those columns downstream. In a nutshell, here is what this funtion does. Keep in mind, all of this happens behind the scenes. 

1) Start an instance of PhantomJS & create a new browser session.  
2) Reads a URL from the list & navigates to the site.
3) Goes through each of the tags, retreives and stores the data in a dataframe called `article`.
4) Moves on to the next URL and repeats.

```{r}
scrape_fish_base <- function(url) {
  
  pjs_session$go(url)
  rendered_source <- pjs_session$getSource()
  html_document <- read_html(rendered_source)

  sci_xpath <- '//*[@id="ss-sciname"]/h1'
  sci_text <- html_document %>%
    html_node(xpath = sci_xpath) %>%
    html_text(trim = T)
  
  comm_xpath <- '//*[@id="ss-sciname"]/span'
  comm_text <- html_document %>%
    html_node(xpath = comm_xpath) %>%
    html_text(trim = T)
  
  env_xpath <- '//*[@id="ss-main"]/div[2]/span'
  evn_text <- html_document %>%
    html_node(xpath = env_xpath) %>%
    html_text(trim = T)
  
  threat_path <- '#ss-main > div.rlalign.sonehalf > div > span'
  threat_text <- html_document %>%
    html_node(threat_path) %>%
    html_text(trim = T)

  uses_xpath1 <- '//*[@id="ss-main"]/div[13]'
  uses_text1 <- html_document %>%
    html_node(xpath = uses_xpath1) %>%
    html_text(trim = T)

  uses_xpath2 <- '//*[@id="ss-main"]/div[12]'
  uses_text2 <- html_document %>%
    html_node(xpath = uses_xpath2) %>%
    html_text(trim = T)

  iucn_xpath <- '#ss-main > div.sleft.sonehalf > div > span'
  iucn_text <- html_document %>%
    html_node(iucn_xpath) %>%
    html_text(trim = T)

  phys_xpath <- '//*[@id="ss-main"]/div[4]/span'
  phys_text <- html_document %>%
    html_node(xpath = phys_xpath) %>%
    html_text(trim = T)

  article <- data.frame(
    url = url,
    sci_name = sci_text,
    common_name = comm_text,
    habitat = evn_text, 
    threat_to_humans = threat_text,
    human_uses1 = uses_text1,
    human_uses2 = uses_text2,
    IUCN_red_list_status = iucn_text,
    physical_characters = phys_text
  )
  
  return(article)
}
```

And here is where I actually call the function. 

```{r}
all_fish_data <- data.frame()
for (i in 1:length(all_links)) {
  cat("Downloading", i, "of", length(all_links), "URL:", all_links[i], "\n")
  article <- scrape_fish_base(all_links[i])
  all_fish_data <- rbind(all_fish_data, article)
}
```

```
Downloading 1 of 1992 URL: https://www.fishbase.se/summary/Leucoraja-erinacea.html 
Downloading 2 of 1992 URL: https://www.fishbase.se/summary/Callorhinchus-milii.html 
Downloading 3 of 1992 URL: https://www.fishbase.se/summary/Latimeria-chalumnae.html 
Downloading 4 of 1992 URL: https://www.fishbase.se/summary/Neoceratodus-forsteri.html 
Downloading 5 of 1992 URL: https://www.fishbase.se/summary/Protopterus-aethiopicus.html 
```

Now I have a dataframe called `all_fish_data` where each row is a fish species and each column is one of the metadata categories. This took a little over an hour to run through all 1992 species but I have the slowest internet connection in the world. I expect the process would be much faster with a better connection.  

Next, it is time to clean up the data. I there is a better way to code the scrape (e.g., more specific `XPATHs`) so less cleanup is required. Anyway, I will go through the cleanup steps for each piece of metadata. 
 

## Human Uses

I start with the Human Uses data. First I will make a copy of the raw dataframe just in case. Here is an example of what these data look like. Remember, I had to use two different `XPATHs` for this metadata because of the structure of the web pages. This gets a little messy because the code picks up none-target metadata. Ok, in this example, we see that the Human uses for #4 and 5 were picked up by the first `XPATH`, #1, 2, and 6 by the second `XPATH`, while #3 returned no results. 

|   | human_uses1                                          | human_uses2                               |
|---|------------------------------------------------------|-------------------------------------------|
| 1 | FAO(Publication : search)  FishSource  Sea Around Us | Fisheries: commercial	                   |
| 2 | FAO(fisheries: production; publication : search)     | Gamefish: yes; aquarium: public aquariums |
| 3 | FAO(Publication : search)  FishSource	               | Threat to humans   Harmless            	 |
| 4 | Fisheries: subsistence fisheries	                   | Threat to humans   Harmless            	 |
| 5 | Fisheries: minor commercial	                         | Threat to humans   Harmless            	 |
| 6 | FAO(Publication : search)  FishSource                | Fisheries: subsistence fisheries          | 

So what I needed to do was replace the non-target results with `NA` and then merge the two columns. 

```{r}
tmp_fish_data <- all_fish_data

tmp_fish_data$human_uses2  <- gsub(x = tmp_fish_data$human_uses2, 
                                   pattern = "Threat to humans.*", 
                                   replacement = NA)  
tmp_fish_data$human_uses1  <- gsub(x = tmp_fish_data$human_uses1, 
                                   pattern = "FAO.*", 
                                   replacement = NA)  
tmp_fish_data <- tmp_fish_data %>% tidyr::unite(human_uses, c(human_uses1, human_uses2), 
                                   remove = TRUE, na.rm = TRUE)
```

Next, I deal with the IUCN Red List status data. This data has some information I am not interested in (e.g., Date assessed). Here is what some of the raw data looks like. 

```
Least Concern (LC) ; Date assessed: 27 October 2011	
Vulnerable (VU) (A1c, B1+2ac); Date assessed: 01 August 1996	
Vulnerable (VU) (A1a, B1+2ac); Date assessed: 01 August 1996	
Data deficient (DD) ; Date assessed: 25 October 2011	
Vulnerable (VU) (B1+2abc, D2); Date assessed: 01 August 1996
```

What I want to do is remove the unwanted data and then make two columns, one for the status and another for the status abbreviation. 

```{r}
tmp_fish_data$IUCN_red_list_status <- stringr::str_replace(tmp_fish_data$IUCN_red_list_status, 
                                                           "; Date assessed.*", "")
tmp_fish_data <- tmp_fish_data %>% tidyr::separate(IUCN_red_list_status, 
                                                   into = c("IUCN", "IUCN_abr"), 
                                                   sep = "[(]",  remove = TRUE)
tmp_fish_data$IUCN_abr <- stringr::str_replace(tmp_fish_data$IUCN_abr, "[)].*", "")
```

Moving on. Some of the Threats to Humans data has references appended (e.g., `(Ref. 4537)`). I am not intertered in these references for my visualization. 

```{r}
## threat_to_humans
tmp_fish_data <- tmp_fish_data %>% tidyr::separate(threat_to_humans, 
                                                   into = c("threat_to_humans"), 
                                                   sep = "[(]",  remove = TRUE)

```




https://www.fishbase.se/search.php
To construct the tree above we first downloaded the newick file from the paper’s supplementary material ([download link](https://static-content.springer.com/esm/art%3A10.1186%2Fs12862-017-0958-3/MediaObjects/12862_2017_958_MOESM2_ESM.tre)), parsed out the Family data from the tree file (included in the name of each leaf), and finally visualized the tree in anvi’o. Within the anvi’o interface we visualized the tree as a circular phylogram and collapsed any Family represented by two or more species. The size of each triangle is proportional to the number of species in that family while the colors are somewhat arbitrary. Originally we thought it would be cool to color leaves by the taxa but that was too complicated. So the colors are really just for looks. The families we studied in this project are in the lower left, marked by asterisks. In the future we would like to identify which fish families have been the subject of microbial investigations and use the phylogenetic framework to identify gaps in knowledge and patterns of associations.





Downloading 1 of 1992 URL: https://www.fishbase.se/summary/Leucoraja-erinacea.html 
Downloading 2 of 1992 URL: https://www.fishbase.se/summary/Callorhinchus-milii.html 
Downloading 3 of 1992 URL: https://www.fishbase.se/summary/Latimeria-chalumnae.html 
Downloading 4 of 1992 URL: https://www.fishbase.se/summary/Neoceratodus-forsteri.html 
Downloading 5 of 1992 URL: https://www.fishbase.se/summary/Protopterus-aethiopicus.html 



Downloading 1 of 1992 URL: https://www.fishbase.se/Country/FaoAreaList.php?ID=2557&GenusName=Leucoraja&SpeciesName=erinacea&fc=19&StockCode=2753&Scientific=Leucoraja+erinacea 
Downloading 2 of 1992 URL: https://www.fishbase.se/Country/FaoAreaList.php?ID=4722&GenusName=Callorhinchus&SpeciesName=milii&fc=24&StockCode=4944&Scientific=Callorhinchus+milii 
Downloading 3 of 1992 URL: https://www.fishbase.se/Country/FaoAreaList.php?ID=2063&GenusName=Latimeria&SpeciesName=chalumnae&fc=30&StockCode=2258&Scientific=Latimeria+chalumnae 
Downloading 4 of 1992 URL: https://www.fishbase.se/Country/FaoAreaList.php?ID=4512&GenusName=Neoceratodus&SpeciesName=forsteri&fc=27&StockCode=4705&Scientific=Neoceratodus+forsteri 
Downloading 5 of 1992 URL: https://www.fishbase.se/Country/FaoAreaList.php?ID=8734&GenusName=Protopterus&SpeciesName=aethiopicus&fc=552&StockCode=9056&Scientific=Protopterus+aethiopicus 
