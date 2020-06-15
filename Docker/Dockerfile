FROM openanalytics/r-base

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

RUN R -e "install.packages('devtools')"
RUN R CMD javareconf
RUN R -e "devtools::install_github('ohdsi/DatabaseConnector')"
RUN R -e "devtools::install_github('ohdsi/FeatureExtraction')"
RUN R -e "devtools::install_github('ohdsi/PatientLevelPrediction')"
RUN R -e "install.packages(c('dplyr','epitools','ggfortify','ggplot2','lcmm','lme4','reshape2','survival'))"
RUN R -e "install.packages(c('shinydashboard','shinyWidgets','shinydashboardPlus','tidyverse','lmerTest'))"
RUN R -e "install.packages('openssl')"
RUN R -e "install.packages('jose')"
RUN R -e "devtools::install_github('ABMI/RTROD_WINGS',ref='master')"

RUN mkdir -p /root/icarusviewer/key
COPY key /root/icarusviewer/key

EXPOSE 3838

RUN echo "local({ options(shiny.port = 3838, shiny.host = '0.0.0.0') })" >> /usr/lib/R/etc/Rprofile.site

CMD ["R", "-e", "shiny::runApp(appDir = file.path(.libPaths()[1],'ICARUSviewer','app.R'))"]
