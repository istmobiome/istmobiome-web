---
title: Interactive fish phylogeny 
subtitle: The Betancur-R bony fish phylogeny, visualized in anvi'o, with metadata scraped from FishBase using rvest. 
authors: [Jarrod J Scott]
summary: Betancur-R fish phylogeny, anvi'o, and rvest webscraping
tags: ["fish", "phylogenetics", "anvio"]
categories: []
date: "2020-11-09T00:00:00+01:00"
external_link:
bibliography: [cite.bib]
header:
  caption: ""
  image: "betancur_banner.png"
image:
  caption: ""
  focal_point:
  preview_only: true
links:
- name: Click here for the Interactive tree
#  url: 
#- icon: database
#  icon_pack: fas
#  name: Data
#  url: https://projectdigest.github.io/data_availability.html
#- icon: code
#  icon_pack: fas
#  name: Code
#  url: https://projectdigest.github.io/raw_code.txt
- icon: github
  icon_pack: fab
  name: GitHub
  url: https://github.com/projectdigest/betancur_r-fish-tree/
publication: []
slides:
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

weight: 5
---

{{% toc %}}

<hr>

<p><small>

**All banner images from** <em>The Fishes of Porto Rico (1902) </em> by Everman & Marsh, Bulletin of the United States Fish Commission, v.20, pt.1, for 1900, Washington, DC: Government Printing Office. Retrieved from <a href="https://commons.wikimedia.org/wiki/Category:The_Fishes_of_Porto_Rico_(1902)">Wikimedia Commons</a> and licenced under <a href="https://creativecommons.org/share-your-work/public-domain/cc0">CC-0</a>. Yes, that is how they spelled Puerto Rico in 1902.
</small></p>

<hr>

My goal in this project is to extend the functionality of the phylogeny of bony fishes by [Betancur-R et. al., (2017)](https://link.springer.com/article/10.1186/s12862-017-0958-3). I use [anvi'o](http://merenlab.org/software/anvio/) to visualize the tree and metadata scraped from [FishBase](https://www.fishbase.se/search.php) to create an interactive phylogeny. 

{{% callout alert %}}
If you are only interested in the data, no need to read further. You can grab all the files from the GitHub repo linked above or by [clicking here](https://github.com/projectdigest/betancur_r-fish-tree/). The repo contains instructions and files needed to visualize the Betancur-R and the FishBase metadata in anvi'o. 
{{% /callout %}}


## Background

As part of an analysis for [another project]({{< ref "/project/projectdigest/index.md" >}}), I needed to create a phylogenetic tree of five fish species. To construct the tree, I also needed an appropriate out group, but given that I knew little about fish, I wasn't sure which species would be most appropriate. My search for an out-group lead to [DeepFin](https://sites.google.com/site/guilleorti/) and the amazing paper by [Betancur-R et. al.](https://link.springer.com/article/10.1186/s12862-017-0958-3)(2017) about the phylogenetic classification of bony fishes. 

Anyway, I used the tree from the paper to choose Gerreidae as the out group for the analysis. I then decided to make my own interactive representation of the Betancur-R tree using [anvi'o](http://merenlab.org/software/anvio/). Anvi'o *is an open-source, community-driven analysis and visualization platform for ‘omics data.* One of the best aspects of anvi'o is its interactive interface, which is amazing for data exploration---including phylogenetic trees. Another strength of the interface is that you can overlay all types of metadata. I wanted to capitalize on this functionality and gathered as much metadata as I could for each species. For this part I used the R package [`rvest`](https://rvest.tidyverse.org/) to scrape species pages on [FishBase](https://www.fishbase.se/search.php) for metadata.

How I did all of this is the subject of this post. 

<br/>

{{% callout synopsis %}}

Workflow overview
<hr>

1) Get the Betancur-R tree data.  <br/> 
2) Modify the data to be compatible with anvi'o. <br/> 
3) Scrape FishBase for metadata using rvest in R.  <br/>
4) Curate metadata. Some manual manipulation required.  <br/>
5) Build anvi'o profile data base.  <br/>

{{% /callout %}}

## Examples of viualization in anvi'o

Here are a few examples of the Betancur-R tree, accompanied by various pieces of FishBase metadata, all visualized in anvi'o. 

<br/>

{{< gallery album="gallery">}}

## The Betancur-R Fish Phylogeny Data

The Betancur-R bony fish phylogeny from the paper is the 4th version (the original dates back to 2013) and contains 1992 species encompassing 72 orders and 410 families. Wow. The tree has representatives from roughly 80% of recognized bony fish families. The study used molecular and genomic data to base classification on inferred phylogenies. From the abstract the author states:

*The first explicit phylogenetic classification of bony fishes was published in 2013, based on a comprehensive molecular phylogeny (www.deepfin.org). We here update the first version of that classification by incorporating the most recent phylogenetic results.*

The authors provide many files with the paper but for the purposes of this visualization I used two files from the [Supplementary material](https://link.springer.com/article/10.1186/s12862-017-0958-3#SupplementaryMaterial), specifically **Additional file 2** ([complete tree in newick format](https://static-content.springer.com/esm/art%3A10.1186%2Fs12862-017-0958-3/MediaObjects/12862_2017_958_MOESM2_ESM.tre)) and **Additional file 4** (Table S1. spreadsheet with full classification). 

I first modified the tree file slightly---in the original file, the Family name was used as a prefix for each species. I removed the Family names from all leaves. To do this you could of course use the command line, but I prefer using the [BBEdit](http://www.barebones.com/products/bbedit/index.html) text editor because it allows grep pattern matching and I can see what is being replaced before I do anything. Plus, for some reason I still absolutely terrible at using the command line for find and replace functions. So, there's that too. The free version of BBEdit has this functionality but if you like the software I recommend purchasing a one-time license. Anyway, this simple regular expression will replace all the Family names in the tree. 

`[A-Z]\w+ae_`

Basically, the command says look for a single capital letter (`[A-Z]`), followed by any word character (`\w`) any number of times (`+`), then the letters `ae`, and finally an underscore (`_`). The `ae` is important because all of the Family names end in `ae`. 

I also removed any subspecies names and any other superfluous information. 

## Scraping FishBase

The next step was to collect metadata about each fish to include in the visualization. [FishBase](https://www.fishbase.se/search.php) is a huge online data base and as of this writing had information on over 34,000 fish species. Here is an example page about [*Herichthys minckleyi* ](https://www.fishbase.se/summary/Herichthys-minckleyi.html) or Minkley's cichlid, a freshwater fish endemic to [Cuatro Cienegas]( https://en.wikipedia.org/wiki/Cuatro_Ci%C3%A9negas), Mexico. There is a lot of metadata on these pages, but I was specifically interested in the following pieces:

1) the scientific and common names.
2) the Environment metadata.
3) the Size / Weight / Age metadata.
4) the IUCN Red List Status.
5) the Threat to humans.
6) the Human uses.
7) the Distribution metadata. *Note* this is retrieved separately. 

Given the size of the tree, doing all of this by hand was simply out of the question. So, I had to learn a little about [web scraping](https://en.wikipedia.org/wiki/Web_scraping). I will not go into the details of web scraping here but basically I used HTML parsing, where specific sequences of HTML tags are used to parse information. The great thing about FishBase is that species pages follow the same general format, making it easier to grab the info you want. Ultimately, how you code a web scrape is highly dependent on structure of the page and the info you want. If you are interested in web scraping there are a ton of great resources and courses. Here are a few I relied on heavily.

- [Web crawling and scraping](https://tm4ss.github.io/docs/Tutorial_1_Web_scraping.html) from a tutorial on *Text mining in R for the social sciences and digital humanities* by Andreas Niekler, Gregor Wiedemann. 
- [Web Scraping Reference: Cheat Sheet for Web Scraping using R](https://github.com/yusuzech/r-web-scraping-cheat-sheet) by [yifyan](https://github.com/yusuzech). 
- [R Web Scraping Cheat Sheet](https://awesomeopensource.com/project/yusuzech/r-web-scraping-cheat-sheet) from Awesome Open Source. 
- [Functions with R and rvest: A Laymen’s Guide](https://towardsdatascience.com/functions-with-r-and-rvest-a-laymens-guide-acda42325a77) by [@peterjgensler](https://medium.com/@peterjgensler). This article was key in helping me design the functions described below. 

This workflow uses [`rvest`](https://www.rdocumentation.org/packages/rvest/versions/0.3.6), an R package for scraping web pages. I will keep the explanation here to a minimum since there are many authoritative sources online.  

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

Moving on. I soon found out that dealing with the *Distribution* metadata my scripts were parsing was huge pain. The amount of manual manipulation I had to do was unacceptable. The first thing I did was to grab all of the other metadata before returning to the *Distribution* data. This does make the code longer, but it also makes data cleanup at the end a lot easier. 

To scrape you need a list of URLs. FishBase being the awesome resource that it is has a very simple URL structure, for example:

`https://www.fishbase.se/summary/Herichthys-minckleyi.html`

Every species follows the same format. The only thing I needed was a list of all species in the tree, so I grabbed the names from my modified Betancur-R tree file. I had to swap the underscore (`_`) in the tree names for a dash (`-`) to make it fit the FishBase URL naming scheme.  

From this list of species names, I made a data frame containing each species name and the corresponding FishBase URL. The code reads in the file and writes a complete URL for each species.

```r
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

Now I have a two-column data frame with the species name and FishBase URL.

```r
all_names <- sample_data$name
all_links <- sample_data$link
```

To understand what this next step is about, please read the [Web crawling and scraping](https://tm4ss.github.io/docs/Tutorial_1_Web_scraping.html) tutorial. 

```r
pjs_instance <- run_phantomjs()
pjs_session <- Session$new(port = pjs_instance$port)
```

Now it was time to write my scrape function, called simply `scrape_fish_base`. Depending on the type of information you are trying to scrape, the code can be pretty simple or a little more complicated. The key is to use specific enough [XPATHs](https://en.wikipedia.org/wiki/XPath) to select the information you are interested in and *only* that information. I am learning that scrapping is a bit of an art form and it takes a little practice to understand how to code things effectively. I practiced a lot on a small subset of the fish species until I got just the right `XPATHs`. I strongly suggest you work through a tutorial like [Scrape This Site](https://scrapethissite.com/) before attempting a more complicated project. 

I won't try to explain what each `XPATH` code means but I will show you how I found the right ones. Let's use our old friend [*Herichthys minckleyi*](https://www.fishbase.se/summary/Herichthys-minckleyi.html) as an example. When you open up this page you need to find the Developer Tools of your browser, the location of which is slightly different depending on your browser. In Chrome for example, you go to `View > Developer > Inspect Elements`. When you do this, a new window  pops up showing the Developer Tools. Use your cursor to find the element of interest, right click, and hit `Copy XPath`. You need to be in the `Elements` tab when you do this. I have found in some cases using the `Selector` instead works better. I am not sure why that is but keep it in mind.

{{< figure src = "images/scrape_1.png">}}

Let's say I am interested in the Environments where *Herichthys minckleyi* is typically found. I run through these steps and end up with this `XPATH`:

> `//*[@id="ss-main"]/div[2]/span` 

I  use this code to select the information I need and then repeat the process for the rest of the metadata. At the end I have a collection of `XPATHS` and `Selectors` that I can add to my function. I found that `XPATHS` worked best for scientific name, common name, environment, human uses, and size / weight / age metadata while `Selector` worked better for IUCN Red List status and threats to humans. In addition, the human uses metadata was a little tricky because on some pages it was one `XPATH` while on other pages it was a different `XPATH`. So, I had to code the threats to humans twice. Once I have the data I simply combine those columns downstream. In a nutshell, here is what this function does. Keep in mind, all of this happens behind the scenes. 

1) Start an instance of PhantomJS & create a new browser session.  
2) Reads a URL from the list & navigates to the site.
3) Goes through each of the tags, retrieves and stores the data in a data frame called `article`.
4) Moves on to the next URL and repeats.

For this function I use the command `html_text`, which tells `rvest` that I am after text. 

```r
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

```r
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

```r
saveRDS(all_fish_data, "rdata/1_scrape_fish_base.rds")
save.image("rdata/1_scrape_fish_base.rdata")
```

Now I have a data frame called `all_fish_data` where each row is a fish species and each column is one of the metadata categories. This took a little over an hour to run through all 1992 species, but I have the slowest internet connection in the world. I expect the process would be much faster with a better connection.  

Next, it is time to clean up the data. I there is a better way to code the scrape (e.g., more specific `XPATHs`) so less cleanup is required. Anyway, I will go through the cleanup steps for each piece of metadata. 

I start with the Human Uses data. First I will make a copy of the raw data frame just in case. Here is an example of what these data look like. Remember, I had to use two different `XPATHs` for this metadata because of the structure of the web pages. This gets a little messy because the code picks up none-target metadata. Ok, in this example, we see that the Human uses for #4 and 5 were picked up by the first `XPATH`, #1, 2, and 6 by the second `XPATH`, while #3 returned no results. 

|   | human_uses1                                          | human_uses2                               |
|---|------------------------------------------------------|-------------------------------------------|
| 1 | FAO(Publication : search)  FishSource  Sea Around Us | Fisheries: commercial                     |
| 2 | FAO(fisheries: production; publication : search)     | Gamefish: yes; aquarium: public aquariums |
| 3 | FAO(Publication : search)  FishSource                | Threat to humans   Harmless               |
| 4 | Fisheries: subsistence fisheries                     | Threat to humans   Harmless               |
| 5 | Fisheries: minor commercial                          | Threat to humans   Harmless               |
| 6 | FAO(Publication : search)  FishSource                | Fisheries: subsistence fisheries          | 

So what I needed to do was replace the non-target results with `NA` and then merge the two columns. 

```r
tmp_fish_data <- all_fish_data

tmp_fish_data$human_uses2  <- gsub(x = tmp_fish_data$human_uses2, 
                                   pattern = "Threat to humans.*", 
                                   replacement = NA)  
tmp_fish_data$human_uses1  <- gsub(x = tmp_fish_data$human_uses1, 
                                   pattern = "FAO.*", 
                                   replacement = NA)  
tmp_fish_data <- tmp_fish_data %>% tidyr::unite(human_uses, 
                                                c(human_uses1, human_uses2), 
                                                remove = TRUE, na.rm = TRUE)
tmp_fish_data$human_uses[tmp_fish_data$human_uses==""] <- NA
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

```r
tmp_fish_data$IUCN_red_list_status <- stringr::str_replace(tmp_fish_data$IUCN_red_list_status, 
                                                           "; Date assessed.*", "")
tmp_fish_data <- tmp_fish_data %>% tidyr::separate(IUCN_red_list_status, 
                                                   into = c("IUCN", "IUCN_abr"), 
                                                   sep = "[(]",  remove = TRUE)
tmp_fish_data$IUCN_abr <- stringr::str_replace(tmp_fish_data$IUCN_abr, "[)].*", "")
```

Moving on. Some of the Threats to Humans data has references appended (e.g., `(Ref. 4537)`). I am not interested in using these references for my visualization. 

```r
## threat_to_humans
tmp_fish_data <- tmp_fish_data %>% tidyr::separate(threat_to_humans, 
                                                   into = c("threat_to_humans"), 
                                                   sep = "[(]",  remove = TRUE)
```

At this point I am sad to say that I had to do the rest of the cleanup by hand, despite my best efforts to code the cleanup. The data is still pretty complicated for me to devise a suitable automated method. 

```r
write.table(tmp_fish_data, "tmp_fish_data_fixed_to_manual.txt", sep = "\t", quote = FALSE) 
saveRDS(tmp_fish_data, "rdata/2_scrape_fish_base_fixed_to_manual.rds")
```

### Scrape IDs & Stock Codes

Ok, I mentioned earlier that the *Distribution* metadata from the main page was too difficult to parse so I decided to scrape *FAO areas* subpage. These pages contain a simple table that looks like this. 

| FAO Area              | Status  | Note                         |
|-----------------------|---------|------------------------------|
| Indian Ocean, Western | native  | 30° E - 80° E; 45° S - 30° N | 
| Indian Ocean, Eastern | native  | 77°E - 150°E; 55°S - 24°N    | 
| Pacific, Northwest    | native  |                              | 

Now, the issue is that the URL for the *FAO areas* subpage was a bit harder to code because the links have species specific `ID codes`  and `StockCodes`. Turns out that I actually needed to first scrape the main page to get the *FAO areas* URL then use that URL to scrape the tables. 

{{< figure src = "images/scrape_2.png">}}

Basically, I want to scrape the URL that is sitting under the highlighted link. As before, I first turn my list of species to FishBase URLs.

```r
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

```r
all_links <- sample_data$link
all_names <- sample_data$name
```

```r
pjs_instance <- run_phantomjs()
pjs_session <- Session$new(port = pjs_instance$port)
```

Then I create a new function called `scrape_fish_code_urls` which uses the XPATH `//*[@id="ss-main"]/h1[3]/span/span[2]/a` to grab that subpage link. Here I use `html_attr` in my code to indicate that the scrape is specifically looking for a URL and not plain text. 

```r
scrape_fish_code_urls <- function(url) {
  
  pjs_session$go(url)
  rendered_source <- pjs_session$getSource()
  html_document <- read_html(rendered_source)

  codes_xpath <- '//*[@id="ss-main"]/h1[3]/span/span[2]/a'
  codes_text <- html_document %>%
    html_node(xpath = codes_xpath) %>%
    html_attr(name = "href", default = "https://www.fishbase.se/Country/FaoAreaList.php?ID=6320&GenusName=Herichthys&SpeciesName=minckleyi&fc=349&StockCode=6633&Scientific=Herichthys+minckleyi")

  fish_codes <- data.frame(
    url = url,
    codes = codes_text
  )
  
  return(fish_codes)
}
```

You should notice that for `html_attr` I used the `default` option and set it to a specific URL. This is an actual *FAO areas* page but for a species not in the list. The reason I do this is to avoid downstream errors with species that do not have a FishBase page, an *FAO areas* page, and/or an empty table. That way, if the code encounters a page that doesn't exist, it will use this one instead. Poor man's workaround to what I can only assume is a simple fix using an `ifelse` statement. 

Again, I run the function. This time, instead of a bunch of metadata I should end up with just a new list of URLs.  

```r
all_fish_codes <- data.frame()
for (i in 1:length(all_links)) {
  cat("Downloading", i, "of", length(all_links), "URL:", all_links[i], "\n")
  fish_codes <- scrape_fish_code_urls(all_links[i])
  all_fish_codes <- rbind(all_fish_codes, fish_codes)
}
```

```
Downloading 1 of 1992 URL: https://www.fishbase.se/summary/Leucoraja-erinacea.html 
Downloading 2 of 1992 URL: https://www.fishbase.se/summary/Callorhinchus-milii.html 
Downloading 3 of 1992 URL: https://www.fishbase.se/summary/Latimeria-chalumnae.html 
Downloading 4 of 1992 URL: https://www.fishbase.se/summary/Neoceratodus-forsteri.html 
Downloading 5 of 1992 URL: https://www.fishbase.se/summary/Protopterus-aethiopicus.html
```

```r
saveRDS(all_fish_codes, "rdata/3_scrape_fish_code_urls.rds")
save.image("rdata/3_scrape_fish_code_urls.rdata")     
```

A little clean-up is required before proceeding. I expected a full URL to the FAO pages, but instead I just got extensions. No big deal, I will just add the base URL name and make a few other changes for downstream analysis.

```r
all_fish_codes$codes <- stringr::str_replace(all_fish_codes$codes, 
                        "\\.\\./Country/FaoAreaList.php", 
                        "https://www.fishbase.se/Country/FaoAreaList.php")

all_fish_codes$url <- stringr::str_replace(all_fish_codes$url, "https://www.fishbase.se/summary/", "")
all_fish_codes$url <- stringr::str_replace(all_fish_codes$url, ".html", "") 
all_fish_codes <- all_fish_codes %>% dplyr::rename("name" = "url")
all_fish_codes <- all_fish_codes %>% dplyr::rename("link" = "codes")
saveRDS(all_fish_codes, "rdata/4_scrape_fish_code_urls_fixed.rds")
```

```r
gdata::keep(all_fish_codes, sure = TRUE)
```

### Scrape Distribution Tables

So now I have a new list of URLs for each species that I can use to download the FAO table data. The tables look like this.


{{< figure src = "images/scrape_3.png">}}


```r
all_links <- all_fish_codes$link
all_names <- all_fish_codes$name
```

```r
pjs_instance <- run_phantomjs()
pjs_session <- Session$new(port = pjs_instance$port)
```

Pretty much the same function as above except this time  I use the command `html_table` to tell `rvest` that I am after a table. One tricky thing is that if the table is empty the script will fail. The only way around this I could find was setting `header = FALSE` which treats table headers as actual data. Not a huge deal since one line of code can remove these later on. 

{{% callout warning %}}
If `header = TRUE` and the table is empty, the job will fail. By setting `header = FALSE` the function treats table headers as data. This means that the final data frame will have headers that must be removed.
{{% /callout %}}

```r
scrape_fish_dist_tabs <- function(url) {
  
  pjs_session$go(url)
  rendered_source <- pjs_session$getSource()
  html_document <- read_html(rendered_source)
  dist_xpath <- '//*[@id="dataTable"]'
  dist_text <- html_document %>%
    html_node(xpath = dist_xpath) %>%
    html_table(header = FALSE, fill = TRUE, trim = TRUE)
  fish_dist <- data.frame(
    url = url,
    dist = dist_text
  )
  
  return(fish_dist)
}
```

```r
all_fish_dist <- data.frame()
for (i in 1:length(all_links)) {
  cat("Downloading", i, "of", length(all_links), "URL:", all_links[i], "\n")
  fish_dist <- scrape_fish_dist_tabs(all_links[i])
  # Append current article data.frame to the data.frame of all articles
  all_fish_dist <- rbind(all_fish_dist, fish_dist)
}
saveRDS(all_fish_dist, "rdata/5_scrape_fish_dist_tabs.rds")
save.image("rdata/5_scrape_fish_dist_tabs.rdata")
```

```
Downloading 1 of 1992 URL: https://www.fishbase.se/Country/FaoAreaList.php?ID=2557&GenusName=Leucoraja&SpeciesName=erinacea&fc=19&StockCode=2753&Scientific=Leucoraja+erinacea 
Downloading 2 of 1992 URL: https://www.fishbase.se/Country/FaoAreaList.php?ID=4722&GenusName=Callorhinchus&SpeciesName=milii&fc=24&StockCode=4944&Scientific=Callorhinchus+milii 
Downloading 3 of 1992 URL: https://www.fishbase.se/Country/FaoAreaList.php?ID=2063&GenusName=Latimeria&SpeciesName=chalumnae&fc=30&StockCode=2258&Scientific=Latimeria+chalumnae 
Downloading 4 of 1992 URL: https://www.fishbase.se/Country/FaoAreaList.php?ID=4512&GenusName=Neoceratodus&SpeciesName=forsteri&fc=27&StockCode=4705&Scientific=Neoceratodus+forsteri 
Downloading 5 of 1992 URL: https://www.fishbase.se/Country/FaoAreaList.php?ID=8734&GenusName=Protopterus&SpeciesName=aethiopicus&fc=552&StockCode=9056&Scientific=Protopterus+aethiopicus 
```

What we end up with is a data frame where each line is the species URL plus a single line from the table. This means that if a table has more than one entry (like the example above), a species will have multiple entries in the data frame. I want a single entry for each species, so I need to concatenate instances of table entries. 

First a little cleanup. I do not need the *Note* data from the tables, and I also want to change the names of the columns. I can also remove any `Herichthys minckleyi` since this was used as dummy data. Come to think of it, I should have removed it from the URL list *before* running `scrape_fish_dist_tabs`. Oh well, live and learn.

```r
all_fish_dist <- readRDS("rdata/scrape_fish_dist_tabs.rds")
all_fish_dist$dist.X3 <- NULL
all_fish_dist <- all_fish_dist[all_fish_dist$dist.X1 != "FAO Area", ]
all_fish_dist <- dplyr::distinct(all_fish_dist)

all_fish_dist <- all_fish_dist %>% dplyr::rename("dist.FAO.Area" = "dist.X1")
all_fish_dist <- all_fish_dist %>% dplyr::rename("dist.Status" = "dist.X2")
```

Now I merge all multiple entries, so I end up with a single line per species containing the FAO area and Status. 

```r
all_fish_dist_agg <- all_fish_dist %>%
                     group_by(url) %>%
                 #mutate(row = row_number()) %>%
                     tidyr::pivot_wider(names_from = dist.FAO.Area, 
                                    values_from = c("dist.FAO.Area", 
                                                    "dist.Status"))
                 #select(-row) %>%
                 #ungroup()
fao_area <- all_fish_dist_agg %>% 
                  ungroup() %>%
                  dplyr::select(matches("^dist.FAO.Area_.*")) %>% 
                  tidyr::unite("fao_area", na.rm = TRUE, 
                               remove = TRUE, sep = "; ")

fao_status <- all_fish_dist_agg %>% 
                  ungroup() %>%
                  dplyr::select(matches("^dist.Status_.*")) %>%
                  tidyr::unite("fao_status", na.rm = TRUE, remove = TRUE, sep = "; ") %>%
                  tidyr::separate(fao_status, "fao_status")

all_fish_dist_final <- cbind(all_fish_dist_agg$url, fao_area, fao_status)
```

```r
saveRDS(all_fish_dist_final, "rdata/6_scrape_fish_dist_tabs_final.rds")
```

OK! Now it is time to put everything together. Here is what I have so far. 

1) The original species list `fish_list.txt`.
2) The manually curated metadata scrape from the species pages `scrape_fish_base_final.txt`. 
3) The fixed Distance metadata from the FAO pages `scrape_fish_dist_tabs_final.rds`. 

I read these data into R so that I can do some final touchup and merge all of the metadata. 

```r
rm(list = ls())
tree_names_dashes <- read.table("fish_list.txt", row.names = NULL)
scrape_fish_base <- read.table("tables/scrape_fish_base_final.txt", 
                               header = TRUE, sep = "\t", 
                               row.names = NULL, stringsAsFactors = FALSE)
scrape_dist <- readRDS("rdata/6_scrape_fish_dist_tabs_final.rds")
```

The trick here is that anvi'o needs a first column called `id` that matches the names in the tree. After that, I can list the metadata columns however I want, as long as the names are unique. 

I can start by renaming the column in the names file. 

```r
tree_names_dashes <- tree_names_dashes %>% dplyr::rename("id" = 1)
```

Next, I will fix the data scraped from the FAO pages. This code adds a new `id` column and renames the other columns. 

```r
scrape_dist <- scrape_dist %>% dplyr::rename("foa_url" = 1) %>%
                               dplyr::mutate(id = foa_url, .before = foa_url)

scrape_dist$id  <- gsub(x = scrape_dist$id, pattern = "https://www.fishbase.se/Country/FaoAreaList.php\\?ID=.*Scientific=", replacement = "")
scrape_dist$id  <- gsub(x = scrape_dist$id, pattern = "\\+", replacement = "-")  
```

```r
scrape_fish_base <- scrape_fish_base %>% dplyr::rename("fb_url" = 1) %>%
                               dplyr::mutate(id = fb_url, .before = fb_url)
scrape_fish_base$id  <- gsub(x = scrape_fish_base$id, 
                        pattern = "https://www.fishbase.se/summary/|.html", 
                        replacement = "")  
```

At this point, if I try to merge all three data frames, something will look a bit fishy. You see, the tree has 1992 species in it, yes the merged data frame has 2000 lines. So, I need to see if anything is duplicated. Sure enough, when I look at the species list I see there are 4 species that are duplicates. 

```r
tree_names_dashes[duplicated(tree_names_dashes[,1:1]),]
```

```
[1] "Opsarius-koratensis"     "Distichodus-fasciolatus" "Lactarius-lactarius"     "Banjos-banjos"
```

Looks like I did not notice this when I made the species list from the original tree. That's ok. I can fix it in the final table. For now, I will remove the duplicate rows and merge all of the table. 

```r
scrape_fish_base <- dplyr::distinct(scrape_fish_base)

fish_base_merge <- dplyr::left_join(tree_names_dashes,  scrape_fish_base) %>%
                   dplyr::left_join(., scrape_dist, by = "id")
saveRDS(fish_base_merge, "rdata/8_fish_base_merge_raw.rds")
```                   

Finally, I can merge all of these metadata with my modified version of **Additional file 4** (Table S1. spreadsheet with full classification) from  the Betancur-R paper. 

```r
additional_data <- read.table("tables/additional_file_4_modified.txt", 
                               header = TRUE, sep = "\t", 
                               row.names = NULL, stringsAsFactors = FALSE)
fish_base_merge <- fish_base_merge %>% dplyr::rename("FishBase_ID" = "id")

fish_metadata <- dplyr::left_join(additional_data, fish_base_merge, by = "FishBase_ID")

fish_metadata[duplicated(fish_metadata[,1:1]),]
fish_metadata <- dplyr::distinct(fish_metadata)
fish_metadata[duplicated(fish_metadata[,1:1]),]
fish_metadata[fish_metadata==""] <- NA
saveRDS(fish_metadata, "rdata/9_fish_metadata.rds")
```

## Building the anvi'o Profile database

{{% callout warning %}}
This section is a work in progress. Please check back soon for updates. 
{{% /callout %}}


At this point I have the Betancur-R tree and a complete metadata file with matching ids. Even though there is a lot of useful information in **Additional file 4**, I will remove some columns that are not really necessary for the visualization. 


1) The original species list from the Betancur-R tree (`fish_list.txt`). 
2) Various metadata scraped from FishBase for each species (`all_fish_meta_final`).
3) Distribution data scraped from the FAO page for each species (`all_fish_dist_final`).
4) Additional file 4 from Betancur-R paper. 

The key is that this table needs to have row names that match the names in the tree. 

Anvi'o is really easy to install using conda in the [Miniconda](https://docs.conda.io/en/latest/miniconda.html) environment. See the installation instructions [here](http://merenlab.org/2016/06/26/installation-v2/). 



