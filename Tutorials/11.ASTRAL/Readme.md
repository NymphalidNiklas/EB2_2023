# Gene tree summary methods

It is quite often observed that phylogenetic relationships inferred from single genes differs from the tree that is inferred using all the available data. Sometimes this is simply due to the single genes not having enough phylogenetic signal, and thus cannot resolve the relationships in question. But there are biological reasons why this would happen, of which two are well studied. The first is Incomplete Lineage Sorting (ILS), which basically means that gene polymorphisms in ancestral populations are carried over to the descendant species. This in turn means that some haplotypes in one species will be more related to haplotypes in another species than to other haplotypes in the same species. The second biological reason that can lead to so-called gene tree/species tree conflicts is hybridization (also called introgression or horizontal transfer). 

Various methods have been developed to investigate these patterns. We will use [ASTRAL-III](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-018-2129-y). You can download the program from its [GitHub repository](https://github.com/smirarab/ASTRAL). [ASTRAL](https://doi.org/10.1093/bioinformatics/btu462) attempts to find the species tree that agrees with the largest number of quartet trees induced by the set of gene trees.

ASTRAL is a commandline driven program, but works very simply. The input file is a file with all the gene trees that one is interested in, so to do an ASTRAL analysis, we first need to generate gene trees. This can be done with IQ-TREE or MrBayes, or any other program that can generate a phylogenetic tree from DNA data. In principle one could do this one by one for each gene, but that would be tedious if one has a large number of genes (e.g. 100s or 1000s of genes). Fortunately, IQ-TREE has a command that will do this for you automatically, either based on a folder with the individual genes in separate files (Fasta or Phylip format), or based on a concatenated dataset and a gene partition file. 

As the four genes we have been working with are too few to do a proper ASTRAL analysis, we are providing you with alignments of the other 9 protein coding genes and two ribosomal genes. You can find all 15 genes [here](../../Data/Day4) as separate Phylip formatted files. In the folder where you have downloaded IQ-TREE, create a folder called `FelidaeMtgenes` and copy the 15 Phylip files to that folder. Estimating the gene trees is very easy, you just run the command

```
./iqtree2 -S FelidaeMtgenes --prefix Felidaeloci -T AUTO
```
After a couple of minutes you should have a number of files with the name `Felidaeloci.*`. The file we are interested in is `Felidaeloci.treefile`, which should have 15 trees in it (one tree for each gene). You can open this file in **FigTree**, and flip through the different trees by clicking on the arrows on the top right (see red rectangle below) in the program. *Do they all look like they have the same topology?* If you look very carefully, you will notice some trees are missing one or two species. This is because that particular gene was missing from the mitochondrial genome data, perhaps because the quality of the sequence was not good.

<p align="center"><img src="./FigTree15trees.png" alt="15 trees" width="800"></p>

The next thing to do is to copy the `Felidaeloci.treefile` file into the same directory where you have your ASTRAL program. Then go to that directory in your terminal and put in the following commands:

```
java -jar astral.5.7.8.jar -i Felidaeloci.treefile -o FelidaeASTRAL.tre 2>FelidaeASTRAL.log
```

Now have a look at the `FelidaeASTRAL.tre` file in **FigTree**. *Does it differ from the tree you have previously gotten when analysing concatenated data?* Have a look also at the FelidaeASTRAL.log file, it gives information about how the run has progressed.

**Task for the afternoon**: you can make a concatenated dataset of all 15 genes, along with a gene partition file (as you did in [Tutorial 2](../2.Alignments), and then analyse this dataset in IQ-TREE (as you did in [Tutorial 3](../3.ModelSelection) and [Tutorial 5](../5.MaximumLikelihood)).
