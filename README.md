# [A Meta-Analysis of Alzheimer's Disease Brain Transcriptomic Data](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6484273/)

##  Introduction
The cause of Alzheimer's disease remains unclear. In the earliest stages of the disease, plaques and tangles form in areas of the brain primarily involved in learning and memory, specifically the hippocampus and entorhinal cortex, both situated in the temporal lobe (TL) region. Next, the frontal lobe (FL), a region involved in voluntary movement, is affected, followed by the parietal lobe (PL), a region involved in processing reading and writing. In the later stage of the disease, the occipital lobe, a region involved in processing information from the eyes, can become affected, followed by the cerebellum (CB), a region which receives information from the sensory systems and the spinal cord to regulates motor movement. Nerve cell death, tissue loss, and atrophy occur throughout the brain as AD progresses, leading to the manifestation of clinical symptoms associated with loss of normal brain function. Interestingly, in most cases, although cerebellum contains ～50% of the neurons in brain, plaques are only occasionally seen and tangles are generally not reported in that region.  
To find the candidate genes that may play an role in Alzheimer's disease processing, the author collected the transcriptome data of AD/ non-AD mental disorder patients from different brain regions, and tried to identify the gene expression change specific to AD. By studying the candidate genes, we may be able to reveal the mechanism of Alzheimer's disease processing. 

## Figure to reproduce
I prepare to reproduce the Figure 2 in the article.  
![](https://www.ncbi.nlm.nih.gov/corecgi/tileshop/tileshop.fcgi?p=PMC3&id=544369&s=87&r=1&c=1)  
This is a heatmap based on microarray data. They tested the gene expression level in temporal lobe, frontal lobe, pariental lobe and cerebellum. Based on the expression level change, the genes were divided into 3 sets. The first set stands for genes that are up or down regulated in the same direction throughout the brain regions. Set 2 stands for those regulates in the same direction in the temporal lob, frontal lobe, and parietal lobe, but reverse regulated in cerebellum. Set 3 are those differentially expressed in all four regions compare to the healthy but show different regulation directions in the four regions. They also labeled the genes that specifically differentially expressed only in AD but not other diseases in red.

## Material and Method
The data were from the Accelerating Medicines Partnership-Alzheimer's Disease AMP-AD shared microarray data and ArrayExpress. Differentially expression fold changes are recorded as xslx files in the supplimentary materials of the article. p-value is calculated by Adaptively Weighted with One-sided Correction in R package "MetaDE"(version 1.0.5). 
There are additional normalization and data cleanup procedures before or after the raw data were transformed into xlsx. I will consider to replicate the works or directly start from the xslx basing on the time I have.  
jad_2019_68-4_jad-68-4-jad181085_jad-68-jad181085-s001.xlsx is the table for Meta-analysis of the 22 AD datasets;  
column A-E are Cerebellum data; G-K are Frontal Lobe; M-Q are Parietal Lobe; S-W are Temporal Lobe.  
In order: Gene ID; Gene name; Description; Meta expression change value; p-value.  

jad_2019_68-4_jad-68-4-jad181085_jad-68-jad181085-s002.xlsx is the table for Meta-analysis of the 21 non-AD datasets;  
column A-D are Cerebellum; F-I are Frontal Lobe; K-N are Parietal Lobe; P-S are Temporal Lobe.  
In order: Gene ID; Gene name; p-value; regulation direction.  

The heatmap was drawn by R. The author didn't show which package he used to generate the heatmap, so here're the ways I found. In this way, I will skip the p-value based selection of candidate gene and calculation of meta expression value. As I am quite new to this, it's better not to be so aggressive. And, these steps can actually be done easily on Galaxy website instead of by coding.
1. Transform the excel1 file back to csv file, re-organize the data. Take 1 column for gene ID and the 4 columns for expression change value out. Sort out the genes that differently expressed in all four regions.  
2. Copy another table of these data, transform the expression change value into +/-, and compare it with excel2. Get a list of AD-specific genes.  
3. Re-sort the row order as all change in same direction, reverse regulated in cerebellum, and different regulated in the four regions.  
4. Draw the heatmap by pheatmap package on R, or matplotlib function from python.  
5. Label the AD-specific genes in step 2 as red.  

## Result  
This is a heatmap of the first part only: with only the genes upregulated in all four regions:  
![](https://raw.githubusercontent.com/Intro-Sci-Comp-UIowa/biol-4386-course-project-Shulin-Liu/main/Output/Rplot.png)  

## Conclusion
They presented the most extensive human AD brain microarray transcriptomic meta-analysis study to date, incorporating, brain regions both affected and partially spared by AD pathology, and utilize related non-AD disorders to infer AD-specific brain change. They identified seven genes specifically perturbed across all AD brain regions and are considered disease-specific, nineteen genes specifically perturbed in AD brains which could play a role in AD neuropathology.
