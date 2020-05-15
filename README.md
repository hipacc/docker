# Docker Images for Hipacc Development

### Provided images
- **[ubuntu-minimal](#image-ubuntu-minimal)***
- **[ubuntu-minimal-cuda](#image-ubuntu-minimal-cuda)***
- **[windows-minimal](#image-windows-minimal)**

*Note that before you can use Docker images from Github's package registry, you need to **[generate a personal access token](https://github.com/settings/tokens)** for authentication.

## Image: ubuntu-minimal
### Description
The image contains the minimal environment to compile Hipacc and run CPU tests on Linux. C++ and OpenCL for CPUs is supported.
Suitable for CI runners without GPU support.

### Build Hipacc and Run CPU Tests
~~~sh
mkdir build_hipacc
cd build_hipacc

# Get the sources
git clone --recursive https://github.com/hipacc/hipacc

# Login to Github package registry
docker login docker.pkg.github.com -u <github-user> -p <personal-access-token>

# Get and run the Docker image
docker run -it -d --name hipacc -v $(pwd):/workspace -w /workspace \
    docker.pkg.github.com/hipacc/docker/ubuntu-minimal:latest

# Compile Hipacc
docker exec hipacc hipacc/.github/run_build.sh

# Run tests (with GPU samples disabled)
docker exec hipacc hipacc/.github/run_tests.sh \
    -DHIPACC_SAMPLE_CUDA=OFF -DHIPACC_SAMPLE_OPENCL_GPU=OFF
~~~

## Image: ubuntu-minimal-cuda
### Description
The image contains the minimal environment to compile Hipacc and run CPU and GPU tests on Linux. C++, CUDA and OpenCL is supported.
**[Docker needs to be set up with native GPU support](https://github.com/NVIDIA/nvidia-docker#quickstart)** and only Nvidia GPUs are supported.

### Build Hipacc and Run All Tests
~~~sh
mkdir build_hipacc
cd build_hipacc

# Get the sources
git clone --recursive https://github.com/hipacc/hipacc

# Login to Github package registry
docker login docker.pkg.github.com -u <github-user> -p <personal-access-token>

# Get and run the Docker image
docker run --gpus all -it -d --name hipacc-cuda -v $(pwd):/workspace -w /workspace \
    docker.pkg.github.com/hipacc/docker/ubuntu-minimal-cuda:latest

# Compile Hipacc
docker exec hipacc-cuda hipacc/.github/run_build.sh

# Run all tests (including CUDA and OpenCL for GPUs)
docker exec hipacc-cuda hipacc/.github/run_tests.sh
~~~

### Troubleshooting
Ensure that the CUDA driver version of the image and the Docker host match:
~~~sh
# Query driver version of the Docker image
docker exec hipacc-cuda nvidia-smi
~~~

## Image: windows-minimal
### Description
The image contains the minimal environment to compile Hipacc and run CPU tests on Windows. C++ and OpenCL for CPUs is supported.
Suitable for CI runners without GPU support.

### Build Hipacc and Run CPU Tests
~~~powershell
mkdir build_hipacc
cd build_hipacc

# Get the sources
git clone --recursive https://github.com/hipacc/hipacc

# Get and run the Docker image
docker run -it -d --name hipacc -v ${pwd}:C:\workspace -w C:\workspace `
    oreiche/hipacc-windows-minimal:latest

# Compile Hipacc
docker exec hipacc powershell hipacc\.github\run_build.ps1

# Run tests (with GPU samples disabled)
docker exec hipacc powershell hipacc\.github\run_tests.ps1 `
    -DHIPACC_SAMPLE_CUDA=OFF -DHIPACC_SAMPLE_OPENCL_GPU=OFF
~~~
