# Copyright (c) 2021 NVIDIA Corporation.  All rights reserved. 

# To build the docker container, run: $ sudo docker build -t nways-labs:latest .
# To run: $ sudo docker run --rm -it --runtime nvidia -p 8888:8888 nways-labs:latest
# Finally, open http://localhost:8888/

#FROM nvcr.io/nvidia/nvhpc:20.11-devel-cuda_multi-ubuntu20.04
FROM nvcr.io/nvidia/nvhpc:21.3-devel-cuda_multi-ubuntu20.04

RUN apt-get -y update && \
        DEBIAN_FRONTEND=noninteractive apt-get -yq install --no-install-recommends python3-pip python3-setuptools nginx zip make build-essential libtbb-dev && \
        rm -rf /var/lib/apt/lists/* && \
        pip3 install --upgrade pip &&\
        pip3 install gdown
        
RUN apt-get update -y        
RUN apt-get install -y git nvidia-modprobe
RUN pip3 install jupyterlab
# Install required python packages
RUN pip3 install ipywidgets

############################################
# NVIDIA nsight-systems-2020.5.1 ,nsight-compute-2
#RUN apt-get update -y && \
#        DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
#       apt-transport-https ca-certificates gnupg wget && \
#        apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys F60F4B3D7FA2AF80 && \
#        echo "deb https://developer.download.nvidia.com/devtools/repos/ubuntu2004/amd64/ /" >> /etc/apt/sources.list.d/nsight.list &&\
RUN apt-get update -y

# RUN DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends nsight-systems-2020.5.1 nsight-compute-2020.2.1 

# TO COPY the data
COPY nways_labs/ /labs/

RUN python3 /labs/nways_MD/English/C/source_code/dataset.py
RUN python3 /labs/nways_MD/English/Fortran/source_code/dataset.py

#################################################
ENV LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/lib:/opt/nvidia/hpc_sdk/Linux_x86_64/21.3/cuda/11.3/lib64/"
#ENV PATH="/opt/nvidia/nsight-systems/2020.5.1/bin:/opt/nvidia/nsight-compute/2020.2.1:/opt/nvidia/hpc_sdk/Linux_x86_64/21.3/cuda/11.2/include:/usr/local/bin:/opt/anaconda3/bin:/usr/bin:$PATH"
ENV PATH="/opt/nvidia/hpc_sdk/Linux_x86_64/21.3/cuda/11.2/include:/usr/local/bin:/opt/anaconda3/bin:/usr/bin:$PATH"
#################################################

ADD nways_labs/ /labs
WORKDIR /labs
CMD jupyter-lab --no-browser --allow-root --ip=0.0.0.0 --port=8888 --NotebookApp.token="" --notebook-dir=/labs
