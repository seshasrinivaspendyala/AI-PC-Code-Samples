# llama-cpp-python with Intel GPU (SYCL) Docker

This repository provides a Dockerfile to build a containerized environment for running [llama-cpp-python](https://github.com/abetlen/llama-cpp-python) with Intel GPU acceleration using SYCL (oneAPI). The image is based on Ubuntu 25.10 and includes all necessary Intel GPU drivers, oneAPI Base Toolkit, and Python dependencies for efficient LLM inference.

## Table of Contents

- [Features](#features)
- [Build the Docker Image](#build-the-docker-image)
- [Run the Container](#run-the-container)
  - [Custom Model or Arguments](#custom-model-or-arguments)
  - [Mount a Local Directory and Run a Model from It](#mount-a-local-directory-and-run-a-model-from-it)
- [Notes](#notes)
- [License](#license)

--

## Features
- **Intel GPU support**: Installs [Intel GPU drivers](https://dgpu-docs.intel.com/driver/client/overview.html) and [oneAPI](https://www.intel.com/content/www/us/en/developer/tools/oneapi/overview.html) for SYCL acceleration.
- **llama-cpp-python**: Builds and installs with Intel SYCL support for fast inference.
- **Python environment**: Uses [uv](https://github.com/astral-sh/uv) for fast Python dependency management.
- **Ready-to-use server**: Runs the llama-cpp-python server by default.

## Build the Docker Image

Clone this repository and build the Docker image:

```sh
# Build the Docker image (replace <tag> with your preferred tag)
docker build -t llamacpp-intel-sycl:latest .
```

## Run the Container

To run the server with a default HuggingFace model (Qwen/Qwen2-0.5B-Instruct-GGUF):

```sh
docker run -it --rm \
    --device /dev/dri \
    -p 8000:8000 \
    llamacpp-intel-sycl:latest
```

- `--device /dev/dri` exposes the Intel GPU to the container.
- `-p 8000:8000` maps the server port.

## Custom Model or Arguments

The default model and arguments can be overridden as needed:

```sh
docker run -it --rm \
    --device /dev/dri \
    -p 8000:8000 \
    llamacpp-intel-sycl:latest \
    --n_gpu_layers -1 \
    --hf_model_repo_id <hf-repo> --model <model-file>
```

## Mount a Local Directory and Run a Model from It

To use a model file stored on the host machine, mount the directory containing the model into the container and specify the path to the model file. For example, if the model is located in `/path/to/models` on the host:

```sh
docker run -it --rm \
    --device /dev/dri \
    -p 8000:8000 \
    -v /path/to/models:/models \
    llamacpp-intel-sycl:latest \
    --n_gpu_layers -1 \
    --model /models/<model-file>
```

- `-v /path/to/models:/models` mounts the local directory into the container at `/models`.
- Replace `<model-file>` with the actual filename of the model inside `/path/to/models`.

This approach can be combined with other arguments as needed.

## Notes
- Make sure your host system has an Intel GPU and the necessary drivers installed.
- For more information about supported models, server options, and how to call inference endpoints, see the [llama-cpp-python OpenAI Server documentation](https://llama-cpp-python.readthedocs.io/en/latest/server/).
- If you're behind some proxies, please update the *config.json(~/.docker/config.json)* file with the correct proxy settings before running the container.

## License:
View the LICENSE file for the repository [here](./LICENSE).
