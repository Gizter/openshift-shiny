FROM rocker/shiny-verse:latest

RUN apt-get update -qq && apt-get -y --no-install-recommends install \
    libxml2-dev \
    libcairo2-dev \
    libsqlite3-dev \
    libpq-dev \
    libssh2-1-dev \
    unixodbc-dev \
    r-cran-v8 \
    libv8-dev \
    net-tools \
    libprotobuf-dev \
    protobuf-compiler \
    libjq-dev \
    libudunits2-0 \
    libudunits2-dev \
    libgdal-dev \
    libssl-dev
    
## update system libraries
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get clean
##Permissions to download packages
RUN sudo usermod -a -G staff shiny

#Custom install packages
RUN echo "options(repos = c(CRAN = 'https://cran.rstudio.com/'), download.file.method = 'libcurl')" >> /usr/local/lib/R/etc/Rprofile.site
RUN R -e 'install.packages("remotes")'
RUN R -e 'remotes::install_github("r-lib/remotes", ref = "97bbf81")'
RUN Rscript -e 'remotes::install_version("magrittr",upgrade="never", version = "1.5")'
RUN Rscript -e 'remotes::install_version("glue",upgrade="never", version = "1.4.2")'
RUN Rscript -e 'remotes::install_version("processx",upgrade="never", version = "3.4.4")'
RUN Rscript -e 'remotes::install_version("testthat",upgrade="never", version = "3.0.0")'
RUN Rscript -e 'remotes::install_version("htmltools",upgrade="never", version = "0.5.0")'
RUN Rscript -e 'remotes::install_version("rmarkdown",upgrade="never", version = "2.5")'
RUN Rscript -e 'remotes::install_version("knitr",upgrade="never", version = "1.30")'
RUN Rscript -e 'remotes::install_version("attempt",upgrade="never", version = "0.3.1")'
RUN Rscript -e 'remotes::install_version("shiny",upgrade="never", version = "1.5.0")'
RUN Rscript -e 'remotes::install_version("config",upgrade="never", version = "0.3.1")'
RUN Rscript -e 'remotes::install_version("tidyverse",upgrade="never", version = "1.3.0")'
RUN Rscript -e 'remotes::install_version("skimr",upgrade="never", version = "2.1.2")'
RUN Rscript -e 'remotes::install_version("visdat",upgrade="never", version = "0.5.3")'
RUN Rscript -e 'remotes::install_version("cowplot",upgrade="never", version = "1.1.1")'
RUN Rscript -e 'remotes::install_version("vroom",upgrade="never", version = "1.3.2")'
RUN Rscript -e 'remotes::install_version("tm",upgrade="never", version = "0.7-8")'
RUN Rscript -e 'remotes::install_version("waiter",upgrade="never", version = "0.2.0")'
RUN Rscript -e 'remotes::install_version("hexbin",upgrade="never", version = "1.28.2")'
RUN Rscript -e 'remotes::install_version("viridis",upgrade="never", version = "0.5.1")'
RUN Rscript -e 'remotes::install_version("sf",upgrade="never", version = "0.9-6")'
RUN Rscript -e 'remotes::install_version("ggrepel",upgrade="never", version = "0.8.2")'
RUN Rscript -e 'remotes::install_version("hrbrthemes",upgrade="never", version = "0.8.0")'
RUN Rscript -e 'remotes::install_version("shinythemes",upgrade="never", version = "1.2.0")'
RUN Rscript -e 'remotes::install_version("DT",upgrade="never", version = "0.16")'
RUN Rscript -e 'remotes::install_version("golem",upgrade="never", version = "0.2.1")'
RUN Rscript -e 'remotes::install_version("wordcloud",upgrade="never", version = "2.6")'
RUN Rscript -e 'remotes::install_version("ggthemes",upgrade="never", version = "4.2.4")'
RUN Rscript -e "install.packages('plotly')
RUN R -e 'remotes::install_local(upgrade="never")'

#Main install
COPY shiny-server.conf /etc/shiny-server/shiny-server.conf
RUN mkdir /srv/code/
RUN chown -R shiny /var/lib/shiny-server/ && chown -R shiny /srv/code/

# OpenShift gives a random uid for the user and some programs try to find a username from the /etc/passwd.
# Let user to fix it, but obviously this shouldn't be run outside OpenShift
RUN chmod ug+rw /etc/passwd
COPY fix-username.sh /usr/bin/fix-username.sh
COPY shiny-server.sh /usr/bin/shiny-server.sh
RUN chmod a+rx /usr/bin/shiny-server.sh

# Make sure the directory for individual app logs exists and is usable
RUN chmod -R a+rwX /var/log/shiny-server
RUN chmod -R a+rwX /var/lib/shiny-server

CMD /usr/bin/shiny-server.sh
