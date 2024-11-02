# Evaluating Methods for Efficient Entity Count Estimation
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.14017389.svg)](https://doi.org/10.5281/zenodo.14017388)


This repository contains the code and data for the paper *"Evaluating Methods for Efficient Entity Count Estimation"*. 

### Abstract
The problem of estimating the size of a query result has a long history in data management. When the query performs entity resolution (aka record linkage or deduplication), the problem is that of estimating the number of distinct entities, referred to as the *entity count*. This problem has received much attention from the statistics community but it has been largely overlooked in the data management literature. In this work, we formally define the entity count problem from a data management perspective and decompose it into a framework of fundamental steps. We explore approaches from both statistics and data management, systematically identifying a design space for different pipelines that address this problem. Finally, we provide extensive experiments to highlight the strengths and weaknesses of these approaches on real-world benchmarks.


### Prerequisites
First, please download the code and data from [Zenodo](https://doi.org/10.5281/zenodo.14017388). 

The experiments were conducted using Python 3.11.5 and R 3.6.1. The experiments make use of the [Networkit](https://networkit.github.io/) library, which can be installed following the instructions on the [Networkit installation page](https://networkit.github.io/get_started.html), based on your operating system. The other Python dependencies are listed in the `requirements.txt` file in the root directory. To install them, we recommend using a virtual environment. Then, run the following command:

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
#### Experimental results
Results for the experiments can be found in the `experiments` folder. For each dataset, the results are stored in a separate folder. The datasets are as follows: `Music-Brainz-20k`, `Music-Brainz-200k`, `Music-Brainz-2M`, `North-Carolina-Voters-5M`, `Cars`, `Alaska-monitor`, `DBLP-Scholar`, `WDC-xlarge-computers`, `Cars-1M` and `WDC-xlarge-computers-1M`. Each dataset folder contains the following files:
- `ER_baseline_with_noisy_<N>_oracle`: The results for the base ML ER pipeline with a noisy oracle, where `<N>` (range 1-4) is the name of the noisy oracle.
- `ER_sampling_with_noisy_<N>_oracle`: The results for the sampling variant of the ML ER pipeline with a noisy oracle, where `<N>` (range 1-4) is the name of the noisy oracle.
- `Simulation_baseline_with_noisy_<N>_oracle`: The results for the base Simulation pipeline with a noisy oracle, where `<N>` (range 1-4) is the name of the noisy oracle.
- `Simulation_sampling_with_noisy_<N>_oracle`: The results for the sampling variant of the Simulation pipeline with a noisy oracle, where `<N>` (range 1-4) is the name of the noisy oracle.
- `Statistical_baseline`: The results for the base Statistical pipeline with [Blink](https://projecteuclid.org/journals/bayesian-analysis/volume-10/issue-4/Entity-Resolution-with-Empirically-Motivated-Priors/10.1214/15-BA965SI.full)
- `Statistical_sampling`: The results for the sampling variant of the Statistical pipeline with [Blink](https://projecteuclid.org/journals/bayesian-analysis/volume-10/issue-4/Entity-Resolution-with-Empirically-Motivated-Priors/10.1214/15-BA965SI.full)
- `Clustering_baseline`: The results for the base LLM Embedding pipeline with using the [text-embedding-3-large](https://platform.openai.com/docs/guides/embeddings) model from OpenAI.
- `Clustering_sampling`: The results for the sampling variant of the LLM Embedding pipeline with using the [text-embedding-3-large](https://platform.openai.com/docs/guides/embeddings) model from OpenAI.

For each pipeline the results are stored in a `.json` file with the prefix `results_`. 

#### Generating the plots
The code for generating the plots is located in the `experiments/result` directory. To generate the plots, run the following command:

```bash
cd experiments/results
python parse_results.py
```

The plots will be saved in the `experiments/results/figures` directory. The script will generate the following plots:
- `baseline_pipeline_small_datasets.pdf`: The results for the base ML ER, Simulation, Statistical, and LLM Embedding pipelines on the small datasets in our experiments.
- `sampling_pipeline_small_datasets.pdf`: The results for the sampling variant of the ML ER, Simulation, Statistical, and LLM Embedding pipelines on a selection of small datasets in our experiments.
- `sampling_pipeline_large_datasets.pdf`: The results for the sampling variant of the ML ER, Simulation, Statistical, and LLM Embedding pipelines on the large datasets in our experiments.


#### Running the pipelines
The code for to run the pipelines is located in the `experiments` directory, and are listed below:
- `ece_er_baseline_with_noisy_oracle.py`: This script implements the base ML ER pipeline with a noisy oracle.
- `ece_er_sampling_with_noisy_oracle.py`: This script implements the sampling variant of the ML ER pipeline with a noisy oracle.
- `ece_simulation_baseline_with_noisy_oracle.py`: This script implements the base Simulation pipeline with a noisy oracle.
- `ece_simulation_sampling_with_noisy_oracle.py`: This script implements the sampling variant of the Simulation pipeline with a noisy oracle.
- `ece_statistical_baseline.py`: This script implements the base Statistical pipeline with [Blink](https://projecteuclid.org/journals/bayesian-analysis/volume-10/issue-4/Entity-Resolution-with-Empirically-Motivated-Priors/10.1214/15-BA965SI.full)
- `ece_statistical_sampling.py`: This script implements the sampling variant of the Statistical pipeline with [Blink](https://projecteuclid.org/journals/bayesian-analysis/volume-10/issue-4/Entity-Resolution-with-Empirically-Motivated-Priors/10.1214/15-BA965SI.full)
- `ece_clustering_baseline.py`: This script implements the base LLM Embedding pipeline with using the [text-embedding-3-large](https://platform.openai.com/docs/guides/embeddings) model from OpenAI.
- `ece_clustering_sampling.py`: This script implements the sampling variant of the LLM Embedding pipeline with using the [text-embedding-3-large](https://platform.openai.com/docs/guides/embeddings) model from OpenAI.

The settings for the pipelines for each dataset are located in the `experiments/dataset_config.json` file. 

To run a pipeline on a dataset, run the following command:

```bash
python <pipeline_script> --dataset <dataset_name> 
```

Where `<pipeline_script>` is the name of the pipeline script you want to run, and `<dataset_name>` is the name of the dataset you want to run the pipeline on. The list of datasets is the same as the ones in the `dataset_config.json` file and are as follows: `Music-Brainz-20k`, `Music-Brainz-200k`, `Music-Brainz-2M`, `North-Carolina-Voters-5M`, `Cars`, `Alaska-monitor`, `DBLP-Scholar`, `WDC-xlarge-computers`, `Cars-1M` and `WDC-xlarge-computers-1M`.
