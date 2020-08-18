---
title: "Relationship between ribosomal protein position and tau association in Alzheimer's disease"
author: "Shon A. Koren"
date: "August 10th, 2020"
output:
  github_document:
    pandoc_args: --webtex
indent: no
---



## Introduction
This code demonstrates the relationship between human ribosomal protein (RP) position within the ribosome and association with the microtubule associated protein (MAP) tau. Recent studies have illustrated the role of tau in (dys)regulating RNA metabolism and protein synthesis (Koren et al., 2020), but little is known about why and how tau associates with RPs. To evaluate one potential guiding principle for tau associating with RPs, we can evaluate the structural position (internal or external) of any specific RP by the Interface Index ($II$). The $II$ measures the number of amino acids (AA) an RP interfaces water and rRNA, suggesting an internal and external position, respectively (Natchiar et al., 2017, Shigeoka et al., 2019).

<p align="center">
$$InterfaceIndex = \displaystyle \frac{ {\sum AA}_{rRNA} } {\sum(AA_{rRNA}+AA_{water} ) }$$
</p>
We can couple the $II$ of any specific RP to its association with tau based on immunoprecipitation-coupled mass spectrometry (IP-MS) studies in the human brain to evaluate any potential bias of association.

## Most RPs are external, but a defined core of internal RPs exist.
Internal RP proteins have more AA interfacing with rRNA than water resulting in a higher Interface Index. A common cutoff for RP structural position is 0.60 (Shigeoka et al., 2019). Plotting for the relative frequency of RP interface indices (**Fig. 1**) reveals most RPs are external rather than internal, but roughly 20% of RPs reside internally.
<p align="center">
<img src="/figs/Tau-RP-Structure/unnamed-chunk-1-1.png" title="testing" alt="testing" style="display: block; margin: auto;" />
</p>
## Tau interactome data from human brain shows high RP association

The most robust investigation of the tau interactome was recently published (Hsieh et al., 2019). This study used label free quantification mass spectrometry (LFQ MS) to investigate the tau interactome when immunoprecipitated from human postmortem brain tissue. Similar to other studies in models of tauopathy, Hsieh et al noted that tau associates with many RP and translational proteins and AD disease status correlates with an enhanced tau-RP association (**Fig. 2**). Notably, however, not all RPs were found due to limitations in MS depth, precluding analysis on roughly half of all RPs.
<p align="center">
<img src="/figs/Tau-RP-Structure/unnamed-chunk-2-1.png" title="testing" alt="testing" style="display: block; margin: auto;" />
</p>
## RP association with tau is unrelated to position within the ribosome

Ribosomes function as molecular motors, coordinating over 100 different proteins to translate RNA at roughly 3 AA/ms. Ribosomes canonically form in the nucleolus, where rRNA and RPs associate together to form their overall quaternary structure. Ribosomes localize ubiquitously in neurons, ranging from distal dendrites to soma to axons. Over continued use throughout the neuronal lifespan, damaged or misfolded RPs require replacement by younger, undamaged variants of that RP to prevent aberrant ribosomal function. This on-site ribosome remodeling program has exciting and broad physiological implications, but remains poorly understood. Considering the depth of tau-RP association, four main hypotheses form:
1. Tau associates with nucleolar RPs during ribosomal biogenesis, involving RPs prior to and following their overall position within the ribosome
2. Tau associates with locally-synthesized RPs during the on-site remodeling program and RPs already positioned within the ribosome
3. Tau associates with extra-ribosomal RPs functioning outside of ribosomes
4. Tau associated with RPs regardless of their localization, function, age, etc.

*Note: Much of ribosomal regulation and proteostasis remains unknown, so these hypotheses are purely speculative and probably oversimplistic. Additionally, tau is known to associate with rRNA and tRNA, complicating this working model.*

If **(1)** is true, then tau would associate with mostly nuclear RPs and not favor any internal or external RP. If **(2)** is true, tau would likely favor nascent RPs, inhibiting translation of RPs would diminish at least part of tau-RP associations and not be bias to structural position in the ribosome. If **(3)** is true, tau-RP association would not require RNA based on known examples of extra-ribosomal RP function and similarly not be bias to RP position. If **(4)** is true, RP position would not correlate with tau association.

More simply, RPs most often reside within the ribosome. Internal RPs have less AA available for surface association of tau, suggesting that tau might favor association with external RPs. To assess whether there is a tendency for tau to bind to RPs based on their structural position within the ribosome, the two sample Kolmogorov-Smirnov (two-sample K-S) test can be used to determine whether two distributions are independent of one another. Since a common cutoff for internal RPs is an Interface Index of 0.60, we can separate RPs based on that statistic. The two-sample K-S test can then be used to test whether the tau association of RPs is independent of their structural position in the ribosome:

{% highlight text %}
## 
## 	Two-sample Kolmogorov-Smirnov test
## 
## data:  rp.filt.in$CTmean and rp.filt.ex$CTmean
## D = 0.38235, p-value = 0.2952
## alternative hypothesis: two-sided
{% endhighlight %}

Since the *p* value of the two-sample K-S test > 0.05, tau-RP association grouped based on their ribosomal structural position (external, $II < 0.60$ and internal, $II > 0.60$) are not independent. This suggests that tau associates with RPs regardless of their position in ribosomes, at least in this basal state of control, non-demented patient brain samples (**Fig. 3A**). RP Interface Index and tau association can be further plotted for AD brain samples (**Fig. 3B**) and the change in tau-RP association from control (CT) to AD brain (**Fig. 3C**).

First, the two-sample K-S test for tau-RP in AD:

{% highlight text %}
## 
## 	Two-sample Kolmogorov-Smirnov test
## 
## data:  rp.filt.in$ADmean and rp.filt.ex$ADmean
## D = 0.32353, p-value = 0.5778
## alternative hypothesis: two-sided
{% endhighlight %}

Next, the two-sample K-S test for tau-RP for the fold change in association between AD and CT samples:

{% highlight text %}
## 
## 	Two-sample Kolmogorov-Smirnov test
## 
## data:  rp.filt.in$AD.CT.FC and rp.filt.ex$AD.CT.FC
## D = 0.38235, p-value = 0.2952
## alternative hypothesis: two-sided
{% endhighlight %}
Both two-sample K-S tests reveal no significant difference between the 0.60 $II$ cutoff between internal and external RPs. This minimal association is more easily interpreted when plotted:
<p align="center">
<img src="/figs/Tau-RP-Structure/unnamed-chunk-6-1.png" title="testing" alt="testing" style="display: block; margin: auto;" />
</p>
However, the two-sample K-S test is only as robust as the distinction between groups. In this biological case, the 0.60 $II$ cutoff might not be a strong divider. Instead, the distribution of RP $II$ to tau-RP association can be linearly regressed and tested by non-parametric Spearman's test for a significant linear correlation. Overall, CT, AD, and AD-CT fold change of tau-RP association do not significantly associate with RP structural position within the ribosome. 

Spearman's non-parametric test of correlation is used over the more robust Pearson's parametric test since the variables (Interface Index and Tau IP intensity) are not normally distributed and some outliers exist. Importantly, in control brain, tau-RP association using Pearson's r reveals $II$ and Tau-IP intensity are significantly associated: *p* = 0.018 with a medium effect size of r = -0.37. Though, this is likely due to two RPs (RPS7 and RPL29) on the extrema.

## Conclusions

Ribosome metabolism provides surprising levels of post-transcriptional regulation of gene expression and protein synthesis. In neurons, ribosomal metabolism spans neuronal cellular compartments from nucleolar ribosome biogenesis and rRNA transcription, to cytoplasmic RNA granule dynamics and transport, to dendritic on-site synthesis of RP and ribosome remodeling programs. Tau associates with ribosomal proteins and rRNA/tRNA in a disease-dependent manner, but the functional implications of this remains elusive. Here, I investigated whether the structural position of a ribosomal protein correlates with association to tau in healthy and diseased brain. Though RP position does not correlate with tau association, this negative data provides important biophysical context on tau-RP binding that can be used to refute hypotheses of tau-mediated translational dysregulation.

## References:

1. Hsieh, Y.-C. et al. Tau-Mediated Disruption of the Spliceosome Triggers Cryptic RNA Splicing and Neurodegeneration in Alzheimer’s Disease. Cell Rep 29, 301-316.e10 (2019).\
2. Koren, S. A., Galvis-Escobar, S. & Abisambra, J. F. Tau-mediated dysregulation of RNA: Evidence for a common molecular mechanism of toxicity in frontotemporal dementia and other tauopathies. Neurobiol. Dis. 141, 104939 (2020).\
3. Natchiar, S. K., Myasnikov, A. G., Kratzat, H., Hazemann, I. & Klaholz, B. P. Visualization of chemical modifications in the human 80S ribosome structure. Nature 551, 472–477 (2017).\
4. Shigeoka, T. et al. On-Site Ribosome Remodeling by Locally Synthesized Ribosomal Proteins in Axons. Cell Rep 29, 3605-3619.e10 (2019).


