from ubuntu:20.04

RUN apt-get update -y --fix-missing
RUN DEBIAN_FRONTEND="noninteractive" apt-get install -y software-properties-common git python python3 python3-pip emacs curl wget zip unzip tar zlib1g-dev liblzo2-dev libcurl4-openssl-dev libssl-dev libncurses5 tzdata

COPY ./code/ /code/

RUN curl -O https://repo.anaconda.com/miniconda/Miniconda3-py37_4.12.0-Linux-x86_64.sh
RUN bash Miniconda3-py37_4.12.0-Linux-x86_64.sh -b -p /code/miniconda3
RUN rm Miniconda3-py37_4.12.0-Linux-x86_64.sh
ENV PATH /code/miniconda3/bin:$PATH
RUN conda update conda
RUN conda update --all
RUN conda install -y -c anaconda pandas=0.24.0
RUN conda install -y -c conda-forge python-lzo=1.12 python-xxhash=1.4.3 biopython=1.73
RUN conda install -y -c bioconda pysam=0.15.2 star=2.6.1b samtools=1.9.0 mapsplice=2.2.1
RUN conda install -y -c bioconda star-fusion=1.5.0 bowtie=1.2.3
RUN conda install -y -c bioconda bowtie2=2.3.4.3
RUN conda install -y -c bioconda bx-python=0.8.2 gffutils=0.10.1
RUN conda install -y -c bioconda fastqc=0.11.9
RUN conda install -y -c bioconda perl-parallel-forkmanager=2.02
RUN conda clean -ay
RUN conda create --name py27 python=2.7
RUN conda install --name py27 -y -c conda-forge biopython=1.73

# These commands should install R-4.2.1
# Maybe try to specify version to make installation more stable

RUN wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc
RUN add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
RUN apt-get install -y r-base

ENV PERL5LIB "$PERL5LIB:/code/SOAPfuse/1.27/source/bin/perl_module/"

# Install R packages

RUN R --slave -e 'install.packages("remotes")'
RUN R --slave -e 'remotes::install_version("optparse", "1.3.2")'
RUN R --slave -e 'remotes::install_version("tidyverse", "1.2.1")'
RUN R --slave -e 'remotes::install_version("XML", "3.99-0.9")'
RUN R --slave -e 'remotes::install_version("randomForest", "4.6-14")'


# After release 1.3.6 these lines can be used to automate docker image creation process further
# RUN rm -rf /code/easyfuse/ && \
# git clone -b  release_1.3.6 https://github.com/TRON-Bioinformatics/EasyFuse.git && \
# mv EasyFuse /code/easyfuse && \
# chmod -R a+rx /code && \
# mv /code/easyfuse/config.ini.docker /code/easyfuse/config.ini && \
# mv /code/easyfuse/build_env.sh.sample /code/easyfuse/build_env.sh && \
# mv /code/easyfuse/blacklist.txt.sample /code/easyfuse/blacklist.txt && \
