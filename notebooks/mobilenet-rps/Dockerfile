# stable Debian (bookworm) with Python 3.10 (works with TF 2.14 wheels)
# Uncomment this for Raspberry Pi
# FROM python:3.10-slim-bookworm

# Uncomment this for X86_64 with GPU support
FROM tensorflow/tensorflow:2.14.0

# FROM python:3.10

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    libgl1 \
    libglib2.0-0 \
    ffmpeg libsm6 libxext6 \
    nano wget tree make \
    openjdk-17-jdk \
    cuda \
 && rm -rf /var/lib/apt/lists/*

# Make sure pip installs to a PATH dir used by your 'docker exec' shell
RUN python -m pip install --no-cache-dir -U pip \
 && python -m pip install --no-cache-dir \
    "tensorflow[gpu]==2.14.0" \
    model-compression-toolkit~=2.2.0 \
    imx500-converter[tf] \ 
    numpy==1.26.4 \
    tensorflow_datasets \
    jupyterlab notebook \
    opencv-python-headless

# Create a non-root user and switch
ARG NB_USER=notebook
ARG NB_UID=1000
RUN useradd -m -s /bin/bash -u ${NB_UID} ${NB_USER}

# Add .local/bin to PATH for user-installed packages
ENV PATH="/home/${NB_USER}/.local/bin:${PATH}"

# Make sure your workdir is owned by the user
WORKDIR /tmp/tutorial
COPY *.ipynb Makefile ./
RUN chown -R ${NB_USER}:${NB_USER} /tmp/tutorial

USER ${NB_USER}

# If you want the image to launch Jupyter directly:
# CMD ["jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--NotebookApp.token=raspi"]
# Otherwise keep your make target:
CMD ["make", "test-github"]
