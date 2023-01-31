# ADEPT: Autoencoder with Differentially Expressed Genes and Imputation for a Robust Spatial Transcriptomics Clustering

## Introduction 

Recent advancements in spatial transcriptomics (ST) have enabled an in-depth understanding of complex tissue by allowing the measurement of gene expression at spots of tissue along with their spatial information. Several notable clustering methods have been introduced to utilize both spatial and transcriptional information in analysis of ST datasets. However, data quality across different ST sequencing techniques and types of datasets appears as a crucial factor that influences the performance of different methods and influences benchmarks. To harness both spatial context and transcriptional profile in ST data, we develop a novel graph-based multi-stage framework for robust clustering, called ADEPT. To control and stabilize data quality, ADEPT relies on selection of differentially expressed genes (DEGs) and imputation of the multiple DEG-based matrices for the initial and final clustering of a graph autoencoder backbone that minimizes the variance of clustering results. We benchmarked ADEPT against five other popular methods on ST data generated by different ST platforms. ADEPT demonstrated its robustness and superiority in different analyses such as spatial domain identification, visualization, spatial trajectory inference, and data denoising.

The general workflow of ADEPT works as follows:

![image](https:****.png)

## Installation
### Dependencies
scanpy==1.9.1

pytorch==1.8.0

pyG==2.0.1

pandas

numpy==1.20.3

scipy

matplolib

### Install from github
1. git clone --recursive https://github.com/maiziezhoulab/AquilaDeepFilter.git
2. conda create -n [EnvName] python=3.7
3. source activate [EnvName]
4. install pytorch (https://pytorch.org/get-started/locally/) + pytorch-geometric (https://pytorch-geometric.readthedocs.io/en/latest/install/installation.html) before everything
5. pip install -r requirements.txt

## Usage

### Data input

we put the STARmap dataset in ***dataset/STARmap***. It is an h5ad file which could be directly used.

For 10x Spatial Transcripts (ST) datasets, files should be put in the same structure with that provided by 10x website. Taking slide 151673 for instance:

> dataset/151673/ 
  >> spatial/  # The folder where files for spatial information can be found 
  
  >> GT.txt # mainly for annotation
  
  >> 151673_filtered_feature_bc_matrix.h5 # gene expression data

### ADEPT main function

Run ADEPT 

The main function of ADEPT is implemented in ***ADEPT_main.py***. When using ADEPT, we do not need to specify any data types. 

The meaning of each argument in ***run_CCST.py*** is listed below.

    parser.add_argument('--impute_cluster_num', type=str, default="7", help="diff cluster numbers for imputation")
    parser.add_argument('--cluster_num', type=int, default=7, help="input data cluster number")
    parser.add_argument('--radius', type=int, default=150, help="input data radius")
    parser.add_argument("--de_candidates", type=str, default="200", help="candidate de list for imputation, separated by comma")
    parser.add_argument('--no_de', type=int, default=0, help='switch on/off DEG selection module')
    parser.add_argument("--use_mean", type=int, default=0, help="use mean value in de list or not")
    parser.add_argument("--impute_runs", type=int, default=2, help="time of runs for imputation")
    parser.add_argument("--runs", type=int, default=20, help="total runs for the data")
    parser.add_argument('--gt', type=int, default=1, help="ground truth for the input data")
    parser.add_argument('--use_hvgs', type=int, default=3000, help="select highly variable genes before training")
    parser.add_argument('--use_preprocessing', type=int, default=1, help='use preprocessed input or raw input')
    parser.add_argument('--save_fig', type=int, default=1, help='saving output visualization')
    parser.add_argument('--filter_nzr', type=float, default=0.15, help='suggested nzr threshold for filtering')
    parser.add_argument('--filter_num', type=int, default=None, help='suggested gene threshold for filtering')
    parser.add_argument('--de_nzr_min', type=float, default=0.299, help='suggested min nzr threshold after de selection')
    parser.add_argument('--de_nzr_max', type=float, default=0.399, help='suggested max nzr threshold after de selection')
    parser.add_argument('--use_gpu_id', type=str, default='0', help='use which GPU, only applies when you have multiple gpu')

**--data_dir**: "./". root dir for input data and results saving

**--gt_dir**: "./". root dir for data ground truth

**--input_data**: the name/section_id of dataset

**--impute_cluster_num**: a hyperparameter for imputation module.  

**--cluster_num**: a hyperparameter in initial/final clustering steps.  

**--radius**：radius to build up kNN graph

**--de_candidates**: candidate de list for imputation, separated by comma

**--no_de**: switch on/off DEG selection module

**--use_mean**: use mean value of de list or not

**--impute_runs**: time of runs for imputation

**--runs**: total runs for the data

**--gt**: ground truth for the input data

**--use_hvgs**: select highly variable genes before training

**--use_preprocessing**: use preprocessed input or raw input

**--save_fig**: saving output visualization

**--filter_nzr**: suggested nzr threshold for filtering

**--filter_num**: suggested gene threshold for filtering

**--de_nzr_min**: suggested min nzr threshold after de selection

**--de_nzr_max**: suggested max nzr threshold after de selection

**--use_gpu_id**: use which GPU, only applies when you have multiple gpu



### sample code for running ADEPT

 
For using ADEPT on DLPFC data, run

 `python ADEPT_main.py --input_data=151673 --cluster_num=7 --radius=150 --use_hvgs=0 --runs=3 --de_candidates=None --impute_runs=5` 
 
 and on STARmap data, run
 
 `python ADEPT_main.py --input_data=starmap --cluster_num=7 --radius=400 --use_hvgs=0 --runs=3 --de_candidates=None --filter_nzr=0 --impute_runs=10 --save_fig=1`
 

### notice

(1) DLPFC data are provided in this zenodo link: .

(2) STARmap data is in this repository.

(3) For data without GT.txt, set  `--gt==0`.

(4) The input root dir for data and grount truth could be separated, so we have to specify paths for both.

<!-- All results are saved in the results folder. We provide our results in the folder ***results*** for taking further analysis. 

(1) The cell clustering labels are saved in ***types.txt***, where the first column refers to cell index, and the last column refers to cell cluster label. 

(3) The spatial distribution of cells within each batch are illustrated in ***.png*** files.  -->


Citation
--------
Yunfei Hu, et al. "ADEPT: Autoencoder with Differentially Expressed Genes and Imputation for a Robust Spatial Transcriptomics Clustering."

paper currently under review for recombseq-2023

