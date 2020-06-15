FROM openanalytics/r-base:3.6.1

# system libraries of general use
RUN apt-get update && apt-get install -y \
    sudo \
    pandoc \
    pandoc-citeproc \
    libcurl4-gnutls-dev \
    libcairo2-dev \
    libxt-dev \
    libssl-dev \
    libssh2-1-dev \
    libssl1.0.0

# system library dependency for the euler app
RUN apt-get update && apt-get install -y \
    libmpfr-dev

# basic shiny functionality
RUN R -e "install.packages(c('shiny', 'rmarkdown'), repos='https://cloud.r-project.org/')"

# install dependencies of the euler app
RUN R -e "install.packages('Rmpfr', repos='https://cloud.r-project.org/')"

RUN apt-get update && \
    apt-get install -y openjdk-8-jdk
RUN mv /usr/lib/jvm/java-8-openjdk-amd64 /usr/lib/jvm/default-java
RUN R -e "install.packages('rJava', dependencies = TRUE, repos='https://cran.rstudio.com/')"

RUN apt-get update
RUN apt-get install -y \
        build-essential \
        libcurl4-gnutls-dev \
        libxml2-dev libssl-dev

RUN R CMD javareconf

## Install R packages
RUN mkdir -p /opt/software/setup/R
ADD install_packages.r /opt/software/setup/R/
RUN Rscript /opt/software/setup/R/install_packages.r

RUN mkdir -p /root/icarusviewer/key
COPY key /root/icarusviewer/key

EXPOSE 3838

RUN echo "local({ options(shiny.port = 3838, shiny.host = '0.0.0.0') })" >> /usr/lib/R/etc/Rprofile.site

CMD ["R", "-e", "shiny::runApp(appDir = file.path(.libPaths()[1],'ICARUSviewer','app.R'))"]
