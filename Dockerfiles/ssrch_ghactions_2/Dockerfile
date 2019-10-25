# Demonstration of 'fattening' container to speed github actions CI
# This will be pushed to dockerhub 
# starting in the 'bioc_bbs2.0/Dockerfiles'' folder
# docker build -t vjcitn/vjcbbs_support:v2 ssrch_ghactions_2
# docker push vjcitn/vjcbbs_support:v2

# then this container can be used in a github actions workflow
 
FROM vjcitn/vjcbbs_support:v1

RUN R -e "BiocManager::install(c('remotes', 'BiocPkgTools', 'pkgbuild'))"
