+++
# About widget.
widget = "about"  # See https://sourcethemes.com/academic/docs/page-builder/
headless = false  # This file represents a page section.
active = false  # Activate this widget? true/false
weight = 20  # Order that this section will appear in.

title = "About the Istmobiome Project"
editable = false
[header]
image = "about_map.png"
#caption = "Image credit: [**Academic**](https://github.com/gcushen/hugo-academic/)"

# Choose the user profile to display
# This should be the username of a profile in your `content/authors/` folder.
# See https://sourcethemes.com/academic/docs/get-started/#introduce-yourself
author = "admin"
+++

The Istmobiome Project, more formally known as the ***Divergence of Marine Symbiosis After the Rise of the Isthmus of Panama***, is a collaborative project funded by the Gordon and Betty Moore Foundation between the University of California---Davis and the Smithsonian Tropical Research Institute. 

{{% callout note %}}
The grant DOI is: http://doi.org/10.37807/GBMF5603
{{% /callout %}}


## Site Overview

This site is a gallery meant to capture and highlight all of the other work we conducted during this grant.  Most of the site is pretty straightforward however the [PROJECTS]({{< ref "../#projects" >}}) section is worth a moment of explanation. Each individual PROJECT page contains a brief project overview of the project plus quick links to the bioinformatic workflows, raw data and data products, GitHub repo, code, etc. In most cases, the complete and reproducible bioinformatic workflows are actually hosted on separate GitHub Pages websites. This has to do with the way each project is generated. Since we often use R code, many figures, tables, analyses, etc. are processed when the project site is built and rendered. Once a project is finished we can archive the final code and simply link to it from istmobiome.rbind.io. This allows us to continually update istmobiome.rbind.io without needing to re-render each project with every site build. It also makes istmobiome.rbind.io more lightweight and faster since it does not have to load every project.

## Grant Overview

The geological formation of the Isthmus of Panama had profound environmental and biological consequences (reviewed in O’Dea et al. 2016[^1]). It divided an ancient seaway into Atlantic and Pacific Oceans, driving major biological changes in the seas and land (op. cit.). It entrained a perfect, natural Darwinian evolutionary experiment in the sea, by creating two oceans with strikingly different geophysical characteristics. The Caribbean (Western Atlantic, WA) became warmer, saltier and nutrient poor, which are ideal conditions for the growth of coral reefs. Conversely, the Bay of Panama in the Tropical Eastern Pacific (TEP) experiences an up-welling of deep water when the seasonal trade-winds blow. Temperature fluctuations are high, both seasonally and due to recurrent ENSO events; the water is more acidic and nutrient rich, with lower salinity from higher rain inputs; and biological productivity is primarily pelagic (Robertson et al. 2009[^2]).

The final separation of these two oceans dates to approximately 3 million years before present, based on an overwhelming body of evidence from geology, marine paleontology, biogeography, geochemistry, and molecular evolution (see O’Dea et al. 2016[^1]), despite recent claims for an older separation (Bacon et al. 2015[^3]; Montes et al. 2015[^4]). Extensive studies have used this natural experiment to examine evolutionary processes relating to molecular divergence and speciation of shallow-water marine macro-organisms in the two oceans, using pairs of sister species (**geminates**), one in the Western Atlantic (WA) and one TEP (reviewed in Lessios 2008[^5]). These studies have leveraged a 60-year history of marine research at the Smithsonian Tropical Research Institute (STRI) that have resulted in nearly 2000 scientific publications, including extensive environmental data from marine stations in the WA and TEP, the former dating to monitoring studies associated with oil spills.

In contrast to the macrofauna and flora, nothing is known about how the isthmian divergence has shaped the evolution of the microbiomes of geminate hosts. Indeed, studies of fish microbiomes in general are relatively few; a meta-analysis of fish gut microbiomes provides data only for 25 fish gut communities representing 16 fish genera, ten of which are marine (Sullam et al. 2012[^6]). In general, how these communities diverge with respect to host divergence is an open question.

Two ecotypes of freshwater guppies in Trinidad have well-known phenotypic differences related to living in low- versus high-predation streams. Their gut microbiomes differed even when the two ecotypes were reared under identical conditions, suggesting that genetic divergence between the two host ecotypes helps shape the gut microbiome community (Sullam et al. 2015[^7]). In wild populations from four streams, however, these communities varied temporally, as well as among
streams and ecotypes in consistent manners, providing evidence against parallel evolution of the gut microbiome with evolution of a novel eco-phenotype in a low-predation environment (op. cit.).

In midas cichlid fish in Nicaraguan lakes there has been repeated evolution of ecologically specialized limnetic and benthic species from a generalist benthic ancestor. The microbiomes of these different forms differed in an older crater lake (maximum age, 24,000 years), but not in a younger one (maximum age, 6,100 years) (Franchini et al. 2014[^8]). The functional significance of these differences remains to be explored.

This study aims to take advantage of the isthmian experiment, leveraging extensive studies on the marine biology of hosts and their environments, to address key questions relating to the evolutionary divergence of marine microbiomes in changing environments and their functional significance. It brings together two complementary teams of researchers, including authorities on geminate species, tropical fish, near-shore marine ecology, and microbial evolution.

Based on a considerable wealth of knowledge gathered by STRI scientists and colleagues over many years, we have identified pairs of geminate species, which meet specific criteria for inclusion in this study, including: relative ease of collecting samples, phylogenetic evidence establishing geminate status, information on evolutionary divergence dates, and differences in trophic strategies (Table 1). The fish and echinoids are all locally available in Panama, while some crustaceans will require collecting further afield (Galapagos and Santa Marta, Colombia).

Using these species we will begin to address the following key questions:

- What were the evolutionary consequences for the microbiomes, in terms of community composition and function, following the evolutionary diversification of host taxa in new environments? 

- Was there a reduction in microbial diversity associated with bottlenecks in host populations? Did the microbiome compensate in any way for reduced genetic diversity of hosts? 

- To what extent did the microbiome co-speciate or co-evolve with hosts? Are there any parallel changes that have occurred in the microbiomes (from a taxonomic composition or functional point of view) across the different taxa on either side of the Isthmus?

- What are the key functional differences in geminate pairs that might be associated with divergent microbiomes? 

<hr>

<p><small>

**All banner images retrieved from** Wikimedia Commons and licenced under <a href="https://creativecommons.org/share-your-work/public-domain/cc0" target = "_blank">CC-0</a>. 

Top row, from left to right: <a href="https://commons.wikimedia.org/wiki/File:Admiralty_Chart_No_1793_Bahia_Almirante_and_Laguna_Chiriqui,_Published_1964.jpg" target = "_blank"> Nautical chart of Bahia Almirante and Laguna Chiriqui, Panama, at a scale of 1:103,280. Suerveyed by Commander E. Barnett 1839</a>; <a href="https://commons.wikimedia.org/wiki/File:Admiralty_Chart_No_1799_Anchorages_on_the_North_Coast_of_Panama,_Published_1927.jpg" target = "_blank"> Nautical chart of Anchorages on the North Coast of Panama</a>; <a href="https://commons.wikimedia.org/wiki/File:Admiralty_Chart_No_657_Isthmus_of_Panama_Showing_The_Proposed_Panama_Canal_and_the_Railway_._._._,_Published_1885.jpg" target = "_blank"> 1885 Admiralty Chart - Isthmus of Panama Showing The Proposed Panama Canal and the Railway</a>; <a href="https://commons.wikimedia.org/wiki/File:Admiralty_Chart_No_1299_Panama_Canal,_Published_1957.jpg" target = "_blank"> Nautical chart of the Panama Canal at a scale of 1/50,000</a>; <a href="https://commons.wikimedia.org/wiki/File:Darien_Nautical_Chart_1737.jpg" target = "_blank"> Darien Nautical Chart 1737. </a>

Bottom row, from left to right: <a href="https://commons.wikimedia.org/wiki/File:Admiralty_Chart_No_1799_Anchorages_on_the_North_Coast_of_Panama,_Published_1927.jpg" target = "_blank"> Nautical chart of Anchorages on the North Coast of Panama</a>; <a href="https://commons.wikimedia.org/wiki/File:Admiralty_Chart_No_2261_Panama_Bay,_Published_1935.jpg" target = "_blank"> Admiralty Chart No 2261 Panama Bay, Published 1935</a>; <a href="https://commons.wikimedia.org/wiki/File:Panama_Nautical_Chart_1775.jpg" target = "_blank"> Panama Nautical Chart 1775</a>; <a href="https://commons.wikimedia.org/wiki/File:Admiralty_Chart_No_2145_Cabo_Mala_to_Bahia_Elena,_Published_1889.jpg" target = "_blank"> Nautical chart of Punta Mala to Santa Elena Bay</a>; <a href="https://commons.wikimedia.org/wiki/File:Admiralty_Chart_No_1799_Anchorages_on_the_North_Coast_of_Panama,_Published_1927.jpg" target = "_blank"> Nautical chart of Anchorages on the North Coast of Panama. </a>

</small></p>


[^1]: O’Dea A and 34 authors (2016). *Formation of the Isthmus of Panama*. [**Science Advances**, 2: e1600883](https://doi.org/10.1126/sciadv.1600883).

[^2]: Robertson, D. R., J. H. Christy, R. Collin, R. G. Cooke, L. D’Croz, K. W. Kaufmann, S. Heckadon Moreno, J. L. Mate, A. O’Dea, and M. Torchin (2009). *The Smithsonian Tropical Research Institute: Marine research, education, and conservation in Panama*. [**Smithsonian Contributions to Marine Sciences**. 38: 73-93](https://doi.org/10.5479/si.01960768.38.73).

[^3]: Bacon, C. D., D. Silvestro, C. Jaramillo, B. T. Smith, P. Chakrabarty, and A. Antonelli (2015). *Biological evidence supports an early and complex emergence of the Isthmus of Panama*. [**Proceedings of the National Academy of Sciences of the United States of America** 112: 6110-6115](https://doi.org/10.1073/pnas.1423853112).

[^4]: Montes, C., A. Cardona, C. Jaramillo, A. Pardo, J. C. Silva, V. Valencia, C. Ayala, L. C. Pérez-Angel, L. A. Rodriguez-Parra, V. Ramirez, and H. Niño. (2015). *Middle Miocene closure of the Central American Seaway*. [**Science** 348:226-229](https://doi.org/10.1126/science.aaa2815).

[^5]: Lessios, HA. (2008). *The great American schism: Divergence of marine organisms after the rise of the Central American Isthmus*. [**Annual Review of Ecology, Evolution, and Systematics** 39, 63--91](https://doi.org/10.1146/annurev.ecolsys.38.091206.095815).

[^6]: Sullam KE, Essinger SD, Lozupone CA, O’Connor MP, Rosen GL, et al. (2012) *Environmental and ecological factors that shape the gut bacterial communities of fish: a meta-analysis*. [**Molecular Ecology 21: 3363--3378**](https://doi.org/10.1111/j.1365-294x.2012.05552.x).

[^7]: Sullam KE, Rubin BER, Dalton CM, Kilham SS, Flecker AS and Russell JA. (2015). *Divergence across diet, time and populations rules out parallel evolution in the gut microbiomes of Trinidadian guppies*. [**The ISME Journal** 9: 1508--1522](https://dx.doi.org/10.1038%2Fismej.2014.231).

[^8]: Franchini P, Fruciano C, Frickey T, Jones JC, Meyer A (2014) The gut microbial community of Midas cichlid fish in repeatedly evolved limnetic-benthic species pairs. [**PLoS ONE** 9: e95027](doi:10.1371/journal.pone.0095027).





