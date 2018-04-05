
Bootstrap: docker
From: nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

%environment

  PATH=/opt/conda/bin:$PATH
  export PATH
  
%post
  
    # Update list of packages
    apt-get update
    # Install dependencies
    apt-get install -y --no-install-recommends build-essential cmake git curl vim ca-certificates libjpeg-dev libpng-dev
    # Clean up
    rm -rf /var/lib/apt/lists/*
    
    # Install miniconda
    cd /opt
    curl -o /opt/miniconda.sh -O  https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
    chmod +x /opt/miniconda.sh
    /opt/miniconda.sh -b -p /opt/conda 
    rm /opt/miniconda.sh
    /opt/conda/bin/conda install numpy pyyaml scipy ipython mkl mkl-include
    /opt/conda/bin/conda install -c soumith magma-cuda90
    /opt/conda/bin/conda clean -ya
    
    # Clone pytorch
    cd /opt/
    git clone https://github.com/pytorch/pytorch.git
    
    # Install pytorch and vision
    cd /opt/pytorch
    git submodule update --init
    export PATH=/opt/conda/bin:$PATH
    TORCH_CUDA_ARCH_LIST="3.5 5.2 6.0 6.1 7.0+PTX" TORCH_NVCC_FLAGS="-Xfatbin -compress-all" \
    CMAKE_PREFIX_PATH="/opt/conda/" /opt/conda/bin/pip install -v .
    git clone https://github.com/pytorch/vision.git
    cd vision
    /opt/conda/bin/pip install -v .

   echo 'export PATH=/opt/conda/bin:$PATH' >>$SINGULARITY_ENVIRONMENT
