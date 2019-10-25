# Demonstration of 'fattening' container to speed github actions CI
# This will be pushed to dockerhub 
# starting in the 'bioc_bbs2.0/Dockerfiles'' folder
# docker build -t vjcitn/vjcbbs_support:v1 ssrch_ghactions
# docker push vjcitn/vjcbbs_support:v1

# then this container can be used in a github actions workflow
 
FROM bioconductor/bioconductor_full:devel

RUN R -e "BiocManager::install(c('ssrch', 'ggplot2'))"
