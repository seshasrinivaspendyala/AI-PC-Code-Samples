# Video Description Generation and Query Retrieval
## Introduction
This sample demonstrates how to generate video descriptions using the [**Qwen 2.5 Vision-Language model**](https://github.com/QwenLM/Qwen2.5-VL) and store their embeddings in [**ChromaDB**](https://www.trychroma.com/) for efficient semantic search on **Intel® Core™ Ultra Processors**. The Qwen 2.5 Vision-Language model is loaded using the [**PyTorch XPU backend**](https://docs.pytorch.org/docs/stable/notes/get_start_xpu.html) to leverage Intel hardware acceleration.\
For each video, a description is generated and stored as an embedding in ChromaDB. When a user submits a query, cosine similarity search is performed in ChromaDB to retrieve the most relevant video description. The matching video is then displayed inline.\
This sample supports sports-related queries and uses a subset of videos from the [**Sports Videos in the Wild (SVW)**](https://cvlab.cse.msu.edu/project-svw.html) dataset. For more information on the dataset and citation requirements, please refer to the [**Sports Videos in the Wild (SVW) dataset paper**](https://cvlab.cse.msu.edu/project-svw.html#:~:text=SVW%20Download,Bibtex%20%7C%20PDF).

## Table of Contents
1. Video Description Generation and Query Retrieval Workflow
2. Sample Structure
3. Prerequisites
4. Installing Prerequisites and Setting Up the Environment
   - For Windows
   - For Linux
5. Running the Sample
6. Sample Execution Output

## Video Description Generation and Query Retrieval Workflow
- During the initial data load, a subset of videos from the [Sports Videos in the Wild (SVW)](https://cvlab.cse.msu.edu/project-svw.html) dataset is fed into the [Qwen 2.5 Vision-Language model](https://github.com/QwenLM/Qwen2.5-VL).
- Here, the [Qwen2.5-VL-3B-Instruct](https://huggingface.co/Qwen/Qwen2.5-VL-3B-Instruct) model variant is used to process these videos and generate descriptions. The Qwen 2.5 Vision-Language model is loaded using the [PyTorch XPU backend](https://docs.pytorch.org/docs/stable/notes/get_start_xpu.html) to leverage Intel hardware acceleration.
- Next, the generated video descriptions are converted into embeddings using [Sentence Transformers](https://sbert.net/), with the [all-MiniLM-L6-v2 model](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2).
- These embeddings, along with the descriptions and video metadata, are stored in a persistent local [ChromaDB](https://www.trychroma.com/) collection. This is a one-time operation; since ChromaDB is local and persistent, it does not need to be repeated unless new videos are added.
- When a user submits a query, the text is similarly encoded into an embedding, which is then used to perform a semantic search (via cosine similarity) over the ChromaDB collection.
- The final result will be the most relevant video description and its associated video file name, and the video is displayed directly in the notebook.

![How it works](./assets/Video_description_generation_and_query_retrieval_workflow.jpg)

## Sample Structure

    .
    ├── Video-Description-Generation-Query-Retrieval/                          # Sample folder
    │   ├── assets/
    │   │   └── Video_description_generation_and_query_retrieval_workflow.jpg  # Workflow image
    │   ├── Readme.md                                                          # Readme file which contains all the details and instructions about the sample      
    │   ├── SVW_subset_video_dataset.zip                                       # ZIP file which contains subset videos of the Sport videos in the wild 
    │   ├── Video_Description_Generation_Query_Retrieval.ipynb                 # Notebook file to excute the sample
    │   ├── pyproject.toml                                                     # Requirements for the sample
    │   └── uv.lock                                                            # File which captures the packages installed for the sample

## Prerequisites

| Component      | Recommended                                             |
| -------------- | ------------------------------------------------------- |
| Operating System(OS)             | Windows 11 or later/ Ubuntu 20.04 or later                    |
| Random-access memory(RAM)          | 32 GB                                                   |
| Hardware | Intel® Core™ Ultra Processors, Intel Arc™ Graphics, Intel Graphics, Intel® Xeon® Processor, Intel® Data Center GPU Max Series |


## Installing Prerequisites and Setting Up the Environment
### Windows:
Install the following software.
1. **GPU Drivers installation**\
   Download and install the Intel® Graphics Driver for Intel® Arc™ B-Series, A-Series, Intel® Iris® Xe Graphics, and Intel® Core™ Ultra Processors with Intel® Arc™ Graphics from [here](https://www.intel.com/content/www/us/en/download/785597/intel-arc-iris-xe-graphics-windows.html)\
   **IMPORTANT:** Reboot the system after the installation.

2. **Git for Windows**\
   Download and install Git from [here](https://git-scm.com/downloads/win)

3. **uv for Windows**\

### Linux:
1. **GPU Drivers installation**\
   Download and install the GPU drivers from [here](https://dgpu-docs.intel.com/driver/client/overview.html)

2. **Dependencies on Linux**\
   Install Git using the following commands:
   - For Debian/Ubuntu-based systems:
   ```
   sudo apt update && sudo apt -y install git
   ```
   - For RHEL/CentOS-based systems:
   ```
   sudo dnf update && sudo dnf -y install git
   ```

## Run the `Video Description Generation and Query Retrieval` Sample:

### Using `uv`:
> 
> </br> **Linux:** </br>
> </br> Use curl to download the script and execute it with sh:
> ```bash
> curl -LsSf https://astral.sh/uv/install.sh | sh
> ```
>
> </br> If your system doesn't have curl, you can use wget:
> ```bash
> wget -qO- https://astral.sh/uv/install.sh | sh
> ```
   
1. In a terminal, navigate to `Video-Description-Generation-Query-Retrieval` folder:
   ```
   cd <path/to/Video-Description-Generation-Query-Retrieval/folder>
   ```
   
2. Launch Jupyter Notebook

   ```
   uv run jupyter lab
   ```
   **NOTE:** Run the below command if you face any dependency issues:
   ```
   uv clean
   ```
