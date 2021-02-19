# [A Meta-Analysis of Alzheimer's Disease Brain Transcriptomic Data](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6484273/)

##  Introduction
The cause of Alzheimer's disease remains unclear. To find the candidate genes that may play an role in Alzheimer's disease processing. The author collected the transcriptome data of AD/ non-AD patients from different brain regions, and tried to identify the gene expression change specific to AD. 

## Figure to reproduce
I prepare to reproduce the Figure 2 in the article.
![](https://www.ncbi.nlm.nih.gov/corecgi/tileshop/tileshop.fcgi?p=PMC3&id=544369&s=87&r=1&c=1)
This is a heatmap based on microarray data. They tested the gene expression level in temporal lobe, frontal lobe, pariental lobe and cerebellum. Based on the expression level change, the genes were divided into 3 sets. The first set stands for genes that are up or down regulated in the same direction throughout the brain regions. Set 2 stands for those regulates in the same direction in the temporal lob, frontal lobe, and parietal lobe, but reverse regulated in cerebellum. Set 3 are those differentially expressed in all four regions compare to the healthy but show different regulation directions in the four regions. They also labeled the genes that specifically differentially expressed only in AD but not other diseases in red.

## Material and Method
The data were from the Accelerating Medicines Partnership-Alzheimer's Disease AMP-AD shared microarray data and ArrayExpress. Differentially expression fold changes are recorded as xslx files in the supplimentary materials of the article. p-value is calculated by Adaptively Weighted with One-sided Correction in R package "MetaDE"(version 1.0.5). 
There are additional normalization and data cleanup procedures before or after the raw data were transformed into xlsx. I will consider to replicate the works or directly start from the xslx basing on the time I have. The heatmap was drawn by R. The author didn't show which package he used to generate the heatmap, but ggplot2 should be able to do that.

## Conclusion
They presented the most extensive human AD brain microarray transcriptomic meta-analysis study to date, incorporating, brain regions both affected and partially spared by AD pathology, and utilize related non-AD disorders to infer AD-specific brain change. They identified seven genes specifically perturbed across all AD brain regions and are considered disease-specific, nineteen genes specifically perturbed in AD brains which could play a role in AD neuropathology.
