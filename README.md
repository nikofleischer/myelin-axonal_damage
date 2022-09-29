# myelin-axonal_damage
Repository for scRNAseq analyses for the paper on myelin and axonal damage by Schaeffner, Bosch-Queralt et al. 

## Methods
### Mouse single-cell partitioning and library generation
The scRNA-sequencing on MACS-purified immune cells was performed using the 10X Genomics 3ʹ Single-Cell v.3.1 kit. The cell count in the single cell suspension was quantified by manual hemocytometer cell counts and the suspension was subsequently diluted to a cell stock concentration of 1,000 cells/µl. The appropriate volume of cell stock for a targeted cell recovery of 10,000 cells was mixed with nuclease-free H2O and the Master Mix according to the Chromium Single-Cell 3’ Reagent Kits v3.1 (dual index) user guide.
The cell suspensions were loaded onto Chromium Next GEM Chip G  (v.3) and run using a Chromium controller. Barcoded single-cell libraries were generated from droplets following the manufacturer’s specifications. The quality of the libraries was assessed using the Agilent 5200 Fragment Analyzer running the High Sensitivity DNA quantification kit.
Next generation sequencing
Single-cell libraries were prepared for sequencing using Illumina NextSeq550 and Novaseq 6000 according to the manufacturer's guidelines and sequenced with 150 bp paired-end reads. Sequencing was performed by the Core Unit DNA-Technologien (Leipzig University/Medical Faculty) and the Insitute for Human Genetics (University Hospital Leipzig).
Single-cell data preprocessing 
Raw Illumina bcl files were demultiplexed using the mkfastq command in bcl2fastq v.2.20. Gene expression matrices were generated with the count command in Cell Ranger v.3.1.0 using the default mm10 mouse genome bundled with Cell Ranger. 

## Single-cell data analysis 
Single-cell RNAseq data were analyzed using R v.4.1.2, primarily with the help of the package Seurat v. 4.1.1. Additional plotting was performed using ggplot2 v.3.3.6, scCustomize v.0.7.0, and PCAtools v2.6.0.

## Quality control
In brief, cells were filtered to contain more than 1000 UMIs, 500 genes, and less than 5 % mitochondrial gene reads. Data were log-normalized and 2000 most highly variable genes selected. Data were scaled and PCA was performed. Based on the ElbowPlot, the first 30 dimensions were selected for downstream analysis. Clustering was performed using the shared nearest neighbor modularity optimization approach implemented in Seurat with resolution parameter 0.8. Dimensionality reduction was performed using UMAP with hyperparameters n.neighbors = 30, min.dist = 0.3. At this point, iterative quality control was performed by calculating marker genes for every cluster using the Wilcoxon Rank Sum test implemented in Seurat. Contaminating cell populations consisting of Oligodendrocytes (Ptgds+), and Endothelial cells (Cldn5+) were excluded. Doublets were removed manually on the basis of dual cell-type marker gene expression and automatically using scDblFinder v.1.8.0 and scran v.1.22.1. Cell cycle scores were calculated using a method established by Tirosh et al. as described in the teaching materials at the Harvard Chan Bioinformatics Core. The remaining cells were reprocessed as before, but regressing out the cell cycle phase during data scaling.  
Cluster annotation
Clusters were annotated based on plotting of canonical marker genes expression and literature search of marker genes that were calculated using the Wilcoxon Rank Sum test as implemented in Seurat. Clusters were joined until each cell type was represented by one cluster. 

## Visualization
The expression of canonical genes for homeostasis and activation of Microglia and Macrophages was plotted for their respective cell cluster using ggplot2. The expression of cytokines was plotted for all cells jointly. 
A pairsplot of the first 10 dimensions of PCA was plotted using PCAtools v.2.6.0. 

 
## Analysis of published data 
The processed 10X Multiome dataset published in Meijer et al. 2022. Neuron, was downloaded from GEO (GSE193238). The dataset was subset to contain only cells annotated as oligodendrocytes by the authors. The expression of the Slc16a1 gene was plotted using ggplot2. 
