# PecanPy: A parallelized, efficient, and accelerated _node2vec_ in Python

A fast, parallelized, memory efficient, and cache optimized Python implementation of [node2vec](https://github.com/aditya-grover/node2vec). The use of CSR representation of sparse graph is inspired by [this blog post](https://www.singlelunch.com/2019/08/01/700x-faster-node2vec-models-fastest-random-walks-on-a-graph/). Full implementation and optimization description, along with benchmarks, can be found in this [preprint](https://doi.org/10.1101/2020.07.23.218487)

## Requirements

* python==3.7.4
* gensim==3.8.0
* numpy==1.17.4
* numba==0.46.0  
**(YOU MIGHT GET SEGMENTATION FAULTS IF YOURE NUMBA IS OUTDATED)**

## Basic Usage

### Example

To run *node2vec* on Zachary's karate club network using `SparseOTF` mode, execute the following command from the project home directory:

```bash
python -m n2v --input demo/karate.edg --output demo/karate.emb --mode SparseOTF
```

### Demo

Execute the following command for full demonstration:

```bash
sh demo/run_n2v
```

### Mode

Three different modes are available, each of which is better optimized for different network sizes/densities

* `PreComp` (default) - precompute second order transition probabilities, using CSR graph
* `SparseOTF` - transition probabilites computed on-the-fly, using CSR graph
* `DenseOTF` - transition probabilities computed on-the-fly, using dense matrix

### Options

Check out the full list of options available using:
```bash
python -m n2v --help
```

### Input

The supported input format is edgelist `.edg` file (node id could be int or string):

```
node1_id node2_id <weight_float, optional>
```

Another supported input format (only for `DenseOTF`) is numpy array `.npz` file, run the following command to prepare the `.npz` file from `.edg`

```bash
python -m n2v --input $input_edgelist --output $output_npz --task todense
```

### Output

The output files has *n+1* lines for graph with *n* vertices, with a header line of the following format:

```
num_of_nodes dim_of_representation
```

The following  next *n* lines are the representations of dimension *d* following the corresponding node ID:

```
node_id dim_1 dim_2 ... dim_d
```
