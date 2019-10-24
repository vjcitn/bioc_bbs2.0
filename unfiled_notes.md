# UNFILED

Drop information of interest, including random links, 
links to documents, etc. here. Once there is a better home for them, 
they can be reorganized.


## Infrastructure resources

### Infrastructure-as-code

- https://techbeacon.com/enterprise-it/infrastructure-code-engine-heart-devops
- https://medium.com/faun/infrastructure-as-code-a-devops-way-to-manage-it-infrastructure-e4f63bfc98fb
- https://docs.ansible.com/ansible/latest/index.html
- https://www.terraform.io/


## Notes

### 10/24 

- github actions should be usable in conjunction with bioconductor_full to automate build tasks
- continuous delivery concept is relevant for developers -- time-scale of response to changes can ultimately be defined by the developer
- relationships between "build system" and "development infrastructure" including bug tracking etc. could be managed with technologies like gitlab
- understand queueing infrastructure
- regard a 'run' of the current BBS as a checkpoint for nonregression of newer implementation of the Build System

### From Slack #build-sys-evolve, Wed Oct 16, 2019

- Vince Carey 3:19 PM: The current build system includes optimization and parallelization in different phases, @hpages can comment further.  How throughput can be increased cost-effectively with greater value to developers (Aaron has raised the most direct concern with difficulty of debugging on windows) is our basic question.  I am also concerned that the burden of build system  maintenance and operation, currently resting on Herve and Lori, should be shared and reduced. (edited) 
- Hervé Pagès 3:20 PM: rather than essentially sequential builds on one node Just to clarify:  The builds are parallelized. They use 15 cores on malbec1 (out of 20), 25 on tokay1 (out of 40), and 18 cores on merida1 (out of 24). The total cumulated time to build/check all software packages is about 5 days so there would be no way we could run nightly builds without parallelization.
- Kasper Hansen 3:20 PM: But it is parallelization across a single R instance? I mean across-cores within a single node. I don't know much about how it is coded right now
- Hervé Pagès 3:23 PM: Yes across cores within a single node. I understand that Martin was suggesting doing parallel builds across nodes but thought I would clarify the "rather than essentially sequential builds on one node" part of his comment just so there is no misunderstanding.
- Kasper Hansen 3:24 PM: how is it done. I know install.packages() has the Ncpus argument, but I realize I don't know about CHECK
- Hervé Pagès 3:27 PM: BBS is written in Python. We use the subprocess module to process the queue of packages i.e. when a slot becomes available we assign it to the next package in the queue and fire a new R CMD check on it (post-office model).
- Sean Davis 3:37 PM: That queue model is probably distributable to more than one node as needed. I’ve been using a publisher/subscriber approach with autoscaled workers on Kubernetes for processing GEO data. Coding is also in python.
- Kasper Hansen 3:39 PM: There is the (probably solvable) issue of installing the package that passes check, to make it available downstream. That's easier with a shared library directory
- Sean Davis 3:39 PM: The interesting detail of costs in such an autoscaling model is that the wall time to complete the processing is not related to cost. Running 20 workers for 1/20th the time costs the same as running 1 instance to completion. The first case completes the same work in 1/20 the time for the same cost. (edited) 
- BJ Stubbs 3:40 PM:  I believe the first step in the BBS is determining all package dependencies and installing them to the host system, and CHECK is later in the workflow after each package is installed, built, etc (edited) 
- Hervé Pagès 3:44 PM: You are correct @BJ Note that this currently uses the same Python-based queue mechanism as the other steps (BUILD, CHECK, and BUILD BIN), and not install.packages(Ncpus) for various reasons.
- Vince Carey 3:46 PM: One hope I have is that the bioconductor_full docker image can be bound to kubernetes cluster and each node gets a piece of the overall dependency graph.  We can then decide how fast this needs to get done and choose the size of the cluster to do this.  Maintenance of that docker image is an ongoing commitment and should be taken advantage of.
- Hervé Pagès 3:47 PM: I think your main point was that all deps are installed first (before any R CMD build or R CMD check), which is also correct. More on this: not all deps get reinstalled at each run, only those that have changed. Also everything gets installed to the same library folder so, on a given build machine, the same R can be used for all the builds (software, data, workflows, and long-tests) and also by the SPB. They all benefit from having everything already installed. 



