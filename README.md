# ARBOLpy
python implementation of ARBOL scRNAseq iterative tiered clustering

<img src="docs/ARBOLsmall.jpg?raw=true" align="right" width=500px>  

Iteratively cluster single cell datasets using a scanpy anndata object as input. Identify and use optimum 
cluster resolution parameters at each tier of clustering. Outputs QC and visualization plots for each clustering event. 

## Install

By github:
```
pip install git+https://github.com/jo-m-lab/ARBOLpy.git
```

from PyPI
```
pip install arbolpy
```

or clone the repository and source the functions directly from the script
```
git clone https://github.com/jo-m-lab/ARBOLpy.git

import "path/to/cloned/git/repo/ARBOLpy/ARBOL")
```

there is a docker image available with ARBOL and dependencies preinstalled
https://hub.docker.com/r/kkimler/arbolpy

## Recommended Usage

ARBOL was developed and used in the paper, "A treatment-naïve cellular atlas of pediatric Crohn’s disease predicts disease severity and therapeutic response"
Currently, a tutorial is only available for the R version, where the FGID atlas figure is reproduced: 
https://jo-m-lab.github.io/ARBOL/ARBOLtutorial.html

This package is meant as a starting point for the way that we approached clustering and and is meant to be edited/customized through community feedback through users such as yourself!

We have dedicated effort to choosing reasonable defaults, but there is no certainty that they are
the best defaults for your data.

The main function of ARBOLpy is ARBOL() - here is an example call. 
The helper function write_ARBOL_output writes the anytree object's endclusters to a csv file.

```
tree = ARBOL.ARBOL(adata)

ARBOL.write_ARBOL_output(tree)
```

**Note** This script can take a long time to run. Running on 20K cells could 
take an hour. Running on 100k+ cells could take 5 hours. This timing varies
based on the heterogeneity of your data.

**Note** The script requires approximately 1.2 GB RAM per 1k cells, meaning on a local machine with 16GB RAM, one could reasonably run 12k cells. The current RAM/time bottleneck is the silhouette analysis, which runs 

## ARBOL() Parameters

* *adata* scanpy anndata object
* *normalization_method* normalization method, defaults to "Pearson", scanpy's experimental implementation of SCTransform
* *tier* starting tier, defaults to 0
* *clustN* starting cluster, defaults to 0
* *min_cluster_size* minimum number of cells to allow further clustering
* *tree* anytree object to attach arbol to. Shouldn't be changed unless building onto a pre-existing tree.
* *parent* parent node of current clustering event, defaults to None. As with tree, shouldn't be changed unless building onto a pre-existing anytree object
* *max_tiers* maximum number of tiers to allow further clustering
* *min_silhouette_res* lower bound of silhouette analysis leiden clustering resolution parameter scan 
* *max_silhouette_res* upper bound
* *h5dir* where to save h5 objects for each tier and cluster, if None, does not save
* *figdir* where to save QC and viz figures for each tier and cluster, if None does not save

## Returns

* anytree object based on iterative tiered clustering
