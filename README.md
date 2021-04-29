# [A Meta-Analysis of Alzheimer's Disease Brain Transcriptomic Data](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6484273/)

##  Introduction
The cause of Alzheimer's disease remains unclear. In the earliest stages of the disease, plaques and tangles form in areas of the brain primarily involved in learning and memory, specifically the hippocampus and entorhinal cortex, both situated in the temporal lobe (TL) region. Next, the frontal lobe (FL), a region involved in voluntary movement, is affected, followed by the parietal lobe (PL), a region involved in processing reading and writing. In the later stage of the disease, the occipital lobe, a region involved in processing information from the eyes, can become affected, followed by the cerebellum (CB), a region which receives information from the sensory systems and the spinal cord to regulates motor movement. Nerve cell death, tissue loss, and atrophy occur throughout the brain as AD progresses, leading to the manifestation of clinical symptoms associated with loss of normal brain function. Interestingly, in most cases, although cerebellum contains ~50% of the neurons in brain, plaques are only occasionally seen and tangles are generally not reported in that region.  
To find the candidate genes that may play an role in Alzheimer's disease processing, the author collected the transcriptome data of AD/ non-AD mental disorder patients from different brain regions, and tried to identify the gene expression change specific to AD. By studying the candidate genes, we may be able to reveal the mechanism of Alzheimer's disease processing. 

## Figure to reproduce
I prepare to reproduce the Figure 2 in the article.  
![](https://www.ncbi.nlm.nih.gov/corecgi/tileshop/tileshop.fcgi?p=PMC3&id=544369&s=87&r=1&c=1)  
This is a heatmap based on microarray data. They tested the gene expression level in temporal lobe, frontal lobe, pariental lobe and cerebellum. Based on the expression level change, the genes were divided into 3 sets. The first set stands for genes that are up or down regulated in the same direction throughout the brain regions. Set 2 stands for those regulates in the same direction in the temporal lob, frontal lobe, and parietal lobe, but reverse regulated in cerebellum. Set 3 are those differentially expressed in all four regions compare to the healthy but show different regulation directions in the four regions. They also labeled the genes that specifically differentially expressed only in AD but not other diseases in red.

## Material and Method
The data were from the Accelerating Medicines Partnership-Alzheimer's Disease AMP-AD shared microarray data and ArrayExpress. Differentially expression fold changes are recorded as xlsx files in the supplimentary materials of the article. p-value is calculated by Adaptively Weighted with One-sided Correction in R package "MetaDE"(version 1.0.5). Only the genes with p-values larger than 0.05 were included in the xlsx tables. 
There are additional normalization and data cleanup procedures before or after the raw data were transformed into xlsx. I directly started from the xlsx. So I didn't do the data analysis by myself.  
jad_2019_68-4_jad-68-4-jad181085_jad-68-jad181085-s001.xlsx is the table for Meta-analysis of the 22 AD datasets;  
column A-E are Cerebellum data; G-K are Frontal Lobe; M-Q are Parietal Lobe; S-W are Temporal Lobe.  
In order: Gene ID; Gene name; Description; Meta expression change value; p-value.  

jad_2019_68-4_jad-68-4-jad181085_jad-68-jad181085-s002.xlsx is the table for Meta-analysis of the 21 non-AD datasets;  
column A-D are Cerebellum; F-I are Frontal Lobe; K-N are Parietal Lobe; P-S are Temporal Lobe.  
In order: Gene ID; Gene name; p-value; regulation direction.  

I firstly downloaded the two xlsx files into my notebook directory using terminal. And then clean-up/sort the data properly on Rstudio. Finally I tried to produce the heatmap from the data by heatmap.2  
For data cleanup:  
1. Pick out the expression level fold change in different brain regions separately into different tables.
2. Separate the up-regulated genes and down-regulated genes in different tables. Delete all the empty lines.
3. Use `Reduce(intersect,list()` to pick out the genes that meet specific requirements (up-regulated/ down-regulated in all four regions, up-regulated/ down-regulated in three regions except brain). Also find all the genes that are differentially regulated in all 4 regions. Use `setdiff()` to delete the ones that meet other requirements. The rest of the genes are the third group, differentially expressed in all four regions compare to the healthy but show different regulation directions in the four regions.
4. Change the data type to numeric by `as.data.frame(lapply(x,as.numeric))`. Actually I didn't expect to meet this problem that the data in the table are characters that can't directly be used to create heatmap. So in this step the tables were transformed to numeric matrix. The format of the original tables were rebuilt by `matrix(x, nrow = m, ncol = n)`. 
5. Find the AD-specific genes. As I have mentioned, the original heatmap I'm trying to reproduce showed all the AD-specific genes in red. So here I found the AD-specific genes by comparing the AD data set and non-AD data set. The genes differed in all four regions of non-AD patients were picked out by simply `nonADgene <- c(nonCere$Gene_Symbol, nonFrontal$Gene_Symbol, nonParietal$Gene_Symbol, nonTemporal$Gene_Symbol)`. The raw names were the gene names. Then I did a logic calculation whether the elements in the differently expressed gene set are also in this nonADgene data set by `TFAD <- rowADmap %in% nonADgene`. The "TRUE"s were replaced by "black" and the "FALSE"s were replaced by "red". This would be the value I used to set the row label color. So that I can label all the "FALSE"s (the genes didn't show differentially expression in non-AD but showed in AD patients) as red.
6. Draw the heatmap using `heatmap.2`. The original heatmap used a tricky skill: they used a red-black-green system to represent the up/down regulation, but they didn't used the color near "black". To mimic their map, I didn't use the `colorRampPalette(c("red", "black", "green"))` as my color set directly. Instead, I divided the colorRampPalette into 20 parts and only used the first and last 7 parts. Seems that they divided the color into more parts, but the plot I created is similar enough.

## Result  
[This is the Linux code for downloading the data from the author's supplimentary data.](https://raw.githubusercontent.com/Intro-Sci-Comp-UIowa/biol-4386-course-project-Shulin-Liu/main/Script/Downloading%20data.md)  
[This is the Rmarkdown for creating the heatmap from the xlsx data.](https://raw.githubusercontent.com/Intro-Sci-Comp-UIowa/biol-4386-course-project-Shulin-Liu/main/Script/Final%20project.Rmd)  
This is the final version of the heatmap.  
![](https://raw.githubusercontent.com/Intro-Sci-Comp-UIowa/biol-4386-course-project-Shulin-Liu/main/Output/AD_gene_expression_heatmap.png)  
Here as the original heatmap did, I found 42 genes that were differentially expressed in all 4 regions. 16 genes were regulated in same direction in all four regions. 10 of them were regulated in same direction in temporal lob, frontal lobe, and parietal lobe, but reverse regulated in cerebellum. The other 16 genes were regulated differently in all four regions but didn't show direction similarity in temporal lob, frontal lobe, and parietal lobe. There are 18 genes that only showed differentially expression in AD patients but not in non-AD patients, thus likely to be AD specific genes.
I didn't add the side bar label (gene set 1/2/3) in the original heatmap in my heatmap. This is because I didn't find the variables in heatmap.2 to add it directly, and I think it's easier to add it by other softwares after the heatmap is exported. It doesn't worth effort to add it by Rstudio.

## Conclusion
They presented the most extensive human AD brain microarray transcriptomic meta-analysis study to date, incorporating, brain regions both affected and partially spared by AD pathology, and utilize related non-AD disorders to infer AD-specific brain change. They identified seven genes specifically perturbed across all AD brain regions and are considered disease-specific, eighteen genes specifically perturbed in AD brains which could play a role in AD neuropathology. The 18 genes, especially those 7 genes could be the future direction of Alzheimer's disease pathway research target or medicine target.
