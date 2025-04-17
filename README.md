# Evaluating Methods for Efficient Entity Count Estimation
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.15023917.svg)](https://doi.org/10.5281/zenodo.15023917)

This repository contains the code and data for the paper *"Evaluating Methods for Efficient Entity Count Estimation"*. 

### Abstract
The problem of estimating the size of a query result has a long history in data management. When the query performs entity resolution (aka record linkage or deduplication), the problem is that of estimating the number of distinct entities, referred to as the *entity count*. This problem has received much attention from the statistics community but it has been largely overlooked in the data management literature. In this work, we formally define the entity count problem from a data management perspective and decompose it into a framework of fundamental steps. We explore approaches from both statistics and data management, systematically identifying a design space for different pipelines that address this problem. Finally, we provide extensive experiments to highlight the strengths and weaknesses of these approaches on real-world benchmarks.


### Prerequisites
First, please download the code and data from [Zenodo](https://doi.org/10.5281/zenodo.15023917). Please note the `.zip` file is 33GB in size. The unzipped folder is 123GB in size. 

The experiments were conducted using Python 3.11.5 and R 3.6.1. The experiments make use of the [Networkit](https://networkit.github.io/) library, which can be installed following the instructions on the [Networkit installation page](https://networkit.github.io/get_started.html), based on your operating system. The other Python dependencies are listed in the `requirements.txt`, which can be found in this repository (not the `.zip` file). To install them, we recommend using a virtual environment. Then, run the following command:

```bash 
pip install -r requirements.txt
```

To install [Blink](https://projecteuclid.org/journals/bayesian-analysis/volume-10/issue-4/Entity-Resolution-with-Empirically-Motivated-Priors/10.1214/15-BA965SI.full), and the R dependencies, run the following command:

```bash
cd src/R/Blink
R -e "install.packages('stringdist', repos='https://cloud.r-project.org/')"
R -e "install.packages('plyr', repos='https://cloud.r-project.org/')"
R -e "install.packages('reticulate', repos='https://cloud.r-project.org/')"
R -e "install.packages('knitr', repos='https://cloud.r-project.org/')"
R -e "install.packages('glue', repos='https://cloud.r-project.org/')"
R -e "install.packages('stringr', repos='https://cloud.r-project.org/')"
R CMD INSTALL blink_1.1.0.tar.gz
```

The experiments also make use of the `text-embedding-3-large` model from OpenAI, which requires an API key. To set up the API key, edit the `.env` file in the root directory and replace the `<API_KEY>` placeholder with your API key.


### Getting Started
Code, data and experimental results can be found in the `vldb-data-and-code` folder. To unzip the file, run the following commands:

```bash
unzip vldb-data-and-code.zip
cd vldb-data-and-code
```

The code is located in the `src` directory, the data is located in the `data` directory, and the experimental results are located in the `experiments-final` directory within the `vldb-data-and-code` folder.

#### Generating the plots
The code for generating the plots is located in the `experiments-final/parse_results` directory. To generate the plots, run the following command:

```bash
cd experiments-final/parse_results
python generate_plots.py
```

The plots will be saved in the `parse_results` directory. The script will generate the following plots, named after the figure numbers in the paper:
- `Figure_1.pdf`: The results for the base ML ER pipeline on a selection of small and large datasets in our experiments in terms of F1-score, approximation error and runtime.
- `Figure_3.pdf`: The results for the base ML ER, Simulation, Statistical, and LLM Embedding pipelines on the small datasets in our experiments.
- `Figure_4.pdf`: The results for the sampling variant of the ML ER, Simulation, Statistical, and LLM Embedding pipelines on a selection of small datasets in our experiments.
- `Figure_5.pdf`: The results for the sampling variant of the ML ER, Simulation, Statistical, and LLM Embedding pipelines on the large datasets in our experiments.


#### Experimental results
Results for the experiments can be found in the `experiments-final` folder. For each dataset, the results are stored in a separate folder. The datasets are as follows: `Music-Brainz-20k`, `Music-Brainz-200k`, `Music-Brainz-2M`, `North-Carolina-Voters-5M`, `Cars`, `Alaska-monitor`, `DBLP-Scholar`, `WDC-xlarge-computers`, `Cars-1M` and `WDC-xlarge-computers-1M`. Each dataset folder contains the following files:
- `ER_baseline`: The results for the base ML ER pipeline with RoBERTa and DistilBERT. The folder contains two subfolders, one for each model.
- `ER_sampling`: The results for the sampling variant of the ML ER pipeline with RoBERTa and DistilBERT. The folder contains two subfolders, one for each model.
- `Simulation_baseline`: The results for the base Simulation pipeline with a calibration process based on RoBERTa and DistilBERT. The folder contains two subfolders, one for each model.
- `Simulation_sampling`: The results for the sampling variant of the Simulation pipeline with a calibration process based on RoBERTa and DistilBERT. The folder contains two subfolders, one for each model.
- `Statistical_baseline`: The results for the base Statistical pipeline with [Blink](https://projecteuclid.org/journals/bayesian-analysis/volume-10/issue-4/Entity-Resolution-with-Empirically-Motivated-Priors/10.1214/15-BA965SI.full). The folder contains two subfolders, one for each model (edit distance and mpnet-based sentence transformer model).
- `Statistical_sampling`: The results for the sampling variant of the Statistical pipeline with [Blink](https://projecteuclid.org/journals/bayesian-analysis/volume-10/issue-4/Entity-Resolution-with-Empirically-Motivated-Priors/10.1214/15-BA965SI.full). The folder contains two subfolders, one for each model (edit distance and mpnet-based sentence transformer model).
- `LLM_embedding_baseline`: The results for the base LLM Embedding pipeline with using the [text-embedding-3-large](https://platform.openai.com/docs/guides/embeddings) model from OpenAI and edit distance. The folder contains two subfolders, one for each model.
- `LLM_embedding_sampling`: The results for the sampling variant of the LLM Embedding pipeline with using the [text-embedding-3-large](https://platform.openai.com/docs/guides/embeddings) model from OpenAI and edit distance. The folder contains two subfolders, one for each model.

For each pipeline the results are stored in a `.json` file with the prefix `results_`.




#### Running the pipelines
The code for to run the pipelines is located in the `experiments-final` directory, and are listed below:
- `run_er_baseline.py`: This script runs the base ML ER pipeline on all datasets for both RoBERTa and DistilBERT.
- `run_er_baseline_control_queries.py`: This script runs the base ML ER pipeline on all datasets for both RoBERTa and DistilBERT with control queries.
- `run_er_sampling.py`: This script runs the sampling variant of the ML ER pipeline on all datasets for both RoBERTa and DistilBERT.
- `run_er_sampling_srs.py`: This script runs the sampling variant of the ML ER pipeline on all datasets for both RoBERTa and DistilBERT with simple random sampling instead of Bernoulli sampling.
- `run_simulation_baselines.py`: This script runs the base Simulation pipeline on all datasets for both RoBERTa and DistilBERT based calibration.
- `run_simulation_sampling.py`: This script runs the sampling variant of the Simulation pipeline on all datasets for both RoBERTa and DistilBERT based calibration.
- `run_simulation_sampling_srs.py`: This script runs the sampling variant of the Simulation pipeline on all datasets for both RoBERTa and DistilBERT based calibration with simple random sampling instead of Bernoulli sampling.
- `run_statistical_baselines.py`: This script runs the base Statistical pipeline on all datasets for both edit distance and mpnet-based sentence transformer model.
- `run_statistical_sampling.py`: This script runs the sampling variant of the Statistical pipeline on all datasets for both edit distance and mpnet-based sentence transformer model.
- `run_clustering_baselines.py`: This script runs the base LLM Embedding pipeline on all datasets for both the [text-embedding-3-large](https://platform.openai.com/docs/guides/embeddings) model from OpenAI and edit distance.
- `run_clustering_sampling.py`: This script runs the sampling variant of the LLM Embedding pipeline on all datasets for both the [text-embedding-3-large](https://platform.openai.com/docs/guides/embeddings) model from OpenAI and edit distance.
- `run_clustering_sampling_srs.py`: This script runs the sampling variant of the LLM Embedding pipeline on all datasets for both the [text-embedding-3-large](https://platform.openai.com/docs/guides/embeddings) model from OpenAI and edit distance with simple random sampling instead of Bernoulli sampling.


To run a pipeline, use the following command:

````bash 
python <pipeline_script> 
````

Where `<pipeline_script>` is the name of the pipeline script you want to run. The results will be stored in the `experiments-final` directory in a folder with the name of the dataset and the pipeline.
