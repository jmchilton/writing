
---

class: title
# A Ubiquitous Approach to Reproducible Bioinformatics Across Computational Platforms

John Chilton, Marius van den Beek, Björn Grüning, Johannes Köster, and the Galaxy Team

John Chilton, Marius van den Beek, Björn Grüning, and the Galaxy Team

The Slides @ http://bit.ly/bosc-biocontainers

The Twitters `@jmchilton`, `#usegalaxy`, `#commonwl`

???

45 secs - Intro and problem with explicit annotation.
 - 15 seconds on example slide.
 - 30 seconds on problems with explicit annotation.
 - TODO: Find a transition.

1:15 min - Conda is better :(, SnakeMake
 - 60 seconds on the comparison slide.
 - 15 seconds on mention SnakeMake.

1 min - Biocontainers and Galaxy Support.
 - 30 seconds Biocontainers (TODO write what your going to say)
 - 30 seconds on jumping off point, reference my GCC talk.

1 min - CWL
  - 10 seconds on announce slide.
  - 20 seconds on install and use slide.
  - 30 seconds on application.

1 min - wrap it up:
 - 20 seconds - quick aside.
 - 20 seconds - repeat key points.
 - 20 seconds - thanks.

---

## Containers in Galaxy and CWL

Galaxy:

```xml
<requirements>
    <container type="docker">dukegcb/seqtk</container>
    <requirement type="package" version="1.2">seqtk</requirement>    
</requirements>
```

Common Workflow Language:

```yaml
hints:
  DockerRequirement:
    dockerPull: dukegcb/seqtk
  SoftwareRequirement:
    packages:
    - package: seqtk
      version:
      - r93
```

???

Both Galaxy and the Common Workflow Language allow you to annotate tools with Docker.

TODO: Verify DockerRequirement syntax!

---

class: left

## The Problem with Making Docker Explicit

.enlarge120[

An arbitrary container image is a blackbox and there is *no guarantee Galaxy/cwltool will execute the same binaries* outside the container.

]

???

For Galaxy in particular, we've had support for Docker since 2014. But we've not really 
bragged about it because we aren't happy about explicit annotation. We annotate tools
with packages that are installable in native systems for non-containerized execution.

So in our view it both hurts reproducibility to annotate a Docker image that is a blackbox
that may not provide the same binaries and libraries as the package annotations - but also
it is completely redundant.

---

## .image-25[![](images/conda_logo.png)] <br /> Conda Key Features for Platforms!

.enlarge120[

- HPC-ready (no root privileges needed) ~~Debian~~
- Cross-Platform (Windows, Mac OS X, Linux) ~~GUIX~~
- Easy to manage and maintain *multiple versions* of the same recipe ~~Homebrew~~

]

???

I love the dedication of Debina Med packagers - their dedicaton somehow makes the
entire open source informatics software ecosystem better. But Galaxy and cwltool
need to run CentOS.

GUIX is stunningly beautiful and the tooling is simply incredible - I think it
does some things in terms of reproducibility better than nix by far. But Galaxy
needs to run on Mac OS X.

I love Homebrew, I love and maintain package definitions, and I use it daily.
I fought hard to make Homebrew appropriate for Galaxy - but one needs to be able
to maintain older versions of packages - not just hack in the ability to install
them - and its just not there. It also lacks the tooling for environment creation
that GUIX and Conda have.

I was very reluctant to come to Conda as the answer, but now I'm willing to
give it an unconditional endorsement for use within computation platforms. The
other potential answers don't check the boxes.

Also Brad uses it!

---

## .image-25[![](images/conda_logo.png)] <br /> SnakeMake

http://bit.ly/snakemake-packages

```sh
snakemake --use-conda
```

```yaml
rule NAME:
    input:
        "table.txt"
    output:
        "plots/myplot.pdf"
    conda:
        "envs/ggplot.yaml"
    script:
        "scripts/plot-stuff.R"
```

```yaml
channels:
 - r
dependencies:
 - r=3.3.1
 - r-ggplot2=2.1.0
```

---

## BioContainers - Bioconda for Containers

.pull-right[.center[![](images/biocontainers.png)]]

*All* Bioconda packages are built into minimal containers.

This setup allows the *same binaries to be used within the container or on traditional (HPC) resources*.

Without any extra work by tool authors, **Galaxy can automatically find or build “the
correct” container** for a best-practice tool.

---

## A Launching Point for Galaxy

By shifting the burden of handling details of containers from the tool author to
the platform, we have increased reproducibility and made available containers for all
"best practice" tools.

We can do some cool new things with these containers:
- Singularity support for Galaxy.
- Native container scheduling Kubernetes and Mesos (via Chronos) support.
- Library support.

See [bit.ly/gcc-biocontainers](https://bit.ly/gcc-biocontainers) for more details.

---

## Bioconda and BioContainers for CWL

.border[.center[![](images/cwl_pr.png)]]

The same reproducibility stack using the same libraries has been integrated into cwltool and an open PR exists for Toil (Conda + BioContainers).

---

## CWL Tools Almost as Cool as Galaxy

The Tool

```yaml
hints:
  SoftwareRequirement:
    packages:
    - package: seqtk
      version:
      - r93
```

Running with cwltool

```sh
$ pip install 'cwltool[deps]'
$ cwltool --beta-conda-dependencies tests/seqtk_seq.cwl tests/seqtk_seq_job.json
$ cwltool --beta-use-biocontainers tests/seqtk_seq.cwl tests/seqtk_seq_job.json
```

Running with Toil (fork)

```sh
$ pip install 'git+https://github.com/jmchilton/toil@cwldeps#egg=toil[cwl]'
$ cwltoil --beta-conda-dependencies tests/seqtk_seq.cwl tests/seqtk_seq_job.json
$ cwltoil --beta-use-biocontainers tests/seqtk_seq.cwl tests/seqtk_seq_job.json
```

---

## CWL Support - An Application

---

## Quick Aside: It isn't just Conda

The Galaxy library support added to cwltool doesn't just provide Conda resolution.
Galaxy entire dependency resolution plugin system is available - meaning support
for Enviornment Modules and Homebrew (beta) are in cwltool as well.

You don't have to use the Bioconda/BioContainers stack to use the plugin infrastructure to
really enable CWL on HPC!

---

- Conda is uniquely qualified to manage packages for scientific computational platforms -
  used by both Galaxy & SnakeMake.
- BioConda + Biocontainers provides reproducibility in and out of containers.
- Galaxy, cwltool, and Toil* use these provide reproduciblity across packages and Docker.
- Galaxy extends reproduciblity by providing support for Singularity, persistent provenance 
  tracking, and usable UIs for everything from job management and workflow building to data
  sharing.

---

## Thanks

.enlarge120[
- Whole Bioconda and Biocontainers communities.
- Conda-ifying Galaxy and tools: Martin Cech, Brad Langhorst, and really the whole IUC and even beyond.
- Building container scheduling job runners: Pablo Moreno, Thodoris Sotiropoulos
- Multi-package mulled hash explorer for BioContainers: Evgeny Anatskiy
- Common Workflow Language integration: James Taylor, Michael Crusoe, Peter Amstutz
]

---

## Generic Containers are Good Slide

.center[.image-75[![](images/container_vs_vm.svg)]]

* Isolation and Security\*
* Reproducibility
* Flexibility\*

.center[.footnote[\* the industry is getting there]]

???

I'm not going to belabor the point made frequently - containers are light
weight alternatives to more traditional virtualization technology. The have
real possiblity of providing more secure, reproducible, and flexible ways
to execute tools in Galaxy.

---

## Galaxy in Containers?

.center[
.image-75[![](images/docker-galaxy-stable-github.png)]
]

???

When people think Galaxy and containers I think this project comes to mind.
Three years ago Björn created this docker-galaxy-stable project and it has
been wildly successful. It has proven to be a useful toolkit for advanced
deployment options, an extremely portable way to bring Galaxy to new domains,
and a brilliant tool for training users.

https://github.com/bgruening/docker-galaxy-stable/commit/27ef7966508958dfec9ce35ff1c5f076ffccf80f

Also mention Galaxy KickStart as a way to run Galaxy itself in a container.

---

## Containerizing Galaxy vs Tools

.enlarge120[
We are going to discuss *containerizing jobs* instead of all of Galaxy.

Small containers for a job's tool.

However you deploy Galaxy, including in a container, tool execution can still be containerized.
]

???

So we are talking about running Galaxy's jobs in containers, not Galaxy itself. We will be discussing
matching minimal best practice containers for particular tools.

Small, best-practice containers for a job's tool.

Even if you deploy Galaxy via a container, you can still containerize its jobs - so this approach is
complimentary to docker-galaxy-stable not competing.s 

---

class: left

## Containerizing Tools is Still Important

.enlarge120[
* Isolated tool execution environment.
* Isolated file system access.
* Added layer of security.
* *Increased re-computability*.
* New deployment options - Kubernetes, Mesos Chronos, AWS EC2 Container Service, etc.
]

---

class: left

## Containerizing Tool Execution

#### Decomposes into two basic problems:

.enlarge120[

* <i class="fa fa-square-o" aria-hidden="true"></i> Instruct Galaxy where to find a container for the tool.
* <i class="fa fa-square-o" aria-hidden="true"></i> Instruct Galaxy to run the tool in a container.

]

---

### Over Three Years Ago...

.border[.center[.image-75[![](images/bbpr401.png)]]]

???

Opened this PR within a few months of Björn.
	
---

## Configuring Galaxy to Use Containers

Just configure the destination. For instance, transform the cluster destination:

```xml
<destination id="short_fast" runner="slurm">
    <param id="nativeSpecification">--time=00:05:00 --nodes=1</param>
</destination>
```

as follows:


```xml
<destination id="short_fast" runner="slurm">
    <param id="nativeSpecification">--time=00:05:00 --nodes=1</param>
    <param id="docker_enabled">true</parma>
    <param id="docker_sudo">false</param>
</destination>
```

---

class: left

## Explicit Container Dependencies

```xml
<requirements>
    <requirement type="package" version="1.2">seqtk</requirement>
</requirements>
```

into

```xml
<requirements>
    <container type="docker">dukegcb/seqtk</container>
    <requirement type="package" version="1.2">seqtk</requirement>
</requirements>
```

---

## Containerizing Tool Execution

#### Decomposes into two basic problems:

.enlarge120[

* <i class="fa fa-check-square-o" aria-hidden="true"></i> Instruct Galaxy where to find a container for the tool.
* <i class="fa fa-check-square-o" aria-hidden="true"></i> Instruct Galaxy to run the tool in a container.

We're done... right?

]

???

---

## To Put it Another Way

.enlarge120[

This:

```xml
<requirements>
    <requirement type="package" version="1.2">seqtk</requirement>
</requirements>
```

should have been sufficient!

]

???

We shouldn't have had to modify the tool to get the containers working, and
indeed specifying containers separately from requirements introduced a strong
possibility of environment drift between package and container execution.

---

## Another Problem...

.enlarge120[

```xml
<requirements>
    <requirement type="package" version="1.2">seqtk</requirement>
</requirements>
```

In 2014, this wasn't even sufficient for normal, containerless execution.

The resolution of these requirements required a `tool_dependencies.xml` file and required the tool to be published to a tool shed before it could be resolved.

]

???

How can I build a container for that requirements set if that requirements set
is useless until the tool has been published to the tool shed.

---

![](images/conda_logo.png)

Package, dependency and environment management

???

We needed a new package manager to be able to build and publish containers for a set of requirements.

---


# Conda Has Flurished Since the Last GCC

---

## Enabled by Default

.center[.border[![](images/conda_by_default_pr.png)]]

---

## Installing Tools with Conda

.center[![](images/tool_install.png)]

Huge thanks to Marius van den Beek, Brad Langhorst, and Martin Cech.

???

This work started at last years GCC hackathon by Brad and Marius and was improved and refined
by Martin.

---

## Managing Tool Dependencies

.center[![](images/dependency_manage.png)]

???

Marius followed up and added detailed admin controls for managing sets of dependencies. With
the older Tool Shed dependency management - sets of dependencies were dependent on the tool and
install context - so GUIs like this wouldn't have worked well.

---

## Planemo

.center[.image-90[.border[![](images/conda_planemo_docs.png)]]]

Conda is enabled by default in Planemo and the documentation has been overhauled with detailed
Conda information.

---

## Training Material
 
---

# Conda has been a revolution for tool development and deployment.

---

## Back to Requirements...

.enlarge120[

```xml
<requirements>
    <requirement type="package" version="1.2">seqtk</requirement>
</requirements>
```

We can now install and resolve dependencies using this alone!

Therefore, we can *find and build containers using just this also*.

]

---

## Bioconda

.border[.center[![](images/bioconda_github.png)]]

???

For comparison to similar projects - Homebrew-science has had 261 commits from 34 authors during the same period. DebianMed has had no new activity in 2017.

---

## BioContainers - Bioconda for Containers

.pull-right[.center[![](images/biocontainers.png)]]

**All** Bioconda packages are built into minimal containers.

This setup allows the **same binaries to be used within the container or on traditional/HPC resources**.
Without any extra work by tool authors, Galaxy can automatically find or build “the correct” container for a best-practice tool.

Over 2,300 containers already available.

---

## BioContainers - Already Published

.center[![](images/quayioseqtk.png)]

---

class: left

## ~~The Problems with Making Docker Explicit~~

.enlarge120[

* ~~Setting up a `Dockerfile` and publishing a Docker image more process for the tool even though the *dependencies have already been completely defined*.~~
* ~~An arbitrary Docker image is a blackbox and there is *no guarantee Galaxy will execute the same binaries* as the Conda requirements.~~

Galaxy can now just find or build BioContainers from `requirement` tags in best practice tools.

]

---

## BioContainers - For Administrators

- In `galaxy.ini`, set `enable_beta_mulled_containers = True`
- In `job_conf.xml`, configure destination

```
<destination id="short_fast" runner="slurm">
    <param id="docker_enabled">true</parma>
    <param id="docker_sudo">false</param>
</destination>
```

???

Should work with all traditional cluster job runners, the local job runner, Pulsar, etc...

---

## BioContainers - For Developers (1 / 2)

<dl>
  <dt><b><code>planemo lint --conda_requirements my_tool.xml<code></b></dt>
  <dd><i>Check if all tool requirements are available in best practice Conda channels - this is required for BioContainers.</i></dd>
  <dt><b><code>planemo lint --biocontainers my_tool.xml<code></b></dt>
  <dd><i>Check if a BioContainers container has been published for this tool.</i></dd>
  <dt><b><code>planemo test --biocontainers my_tool.xml<code></b></dt>
  <dd><i>Run tool tests using a BioContainer (these will build on the fly if not published).</i></dd>
  <dt><b><code>planemo serve --biocontainers my_tool.xml<code></b></dt>
  <dd><i>Interactive testing using biocontainers.</i></dd>
  <dt><b><code>planemo mull my_tool.xml<code></b></dt>
  <dd><i>Build and locally cache a BioContainer for specified tool.</i></dd>
</dl>

---

## BioContainers - For Developers (2 / 2)

.border[.center[![](images/biocontainers_planemo_docs.png)]]

.footnote[.center[Also a Training Materials module!]]

---

## BioContainers - Multi-requirement Tools

```xml
<requirements>
    <requirement type="package" version="0.7.15">bwa</requirement>
    <requirement type="package" version="1.3.1">samtools</requirement>
</requirements>
```

.center[![](images/quay_io_mulled.png)]

---

## Exploring Mulled Hashes - by Evgeny Anatskiy

.center[![](images/multi-package-containers.png)]

.footer[.center[http://biocontainers.pro/multi-package-containers/]]

???

---

### Publishing **Your** Multi-Package Containers

.center[.border[![](images/planemo-monitor.png)]]

???

We are currently monitoring tools-iuc, tools-devteam, and a half a dozen other repositories
daily for package combinations to publish.

---

## Containerizing Tool Execution

#### Decomposes to basic problems

* <i class="fa fa-check-square-o" aria-hidden="true"></i><i class="fa fa-check-square-o" aria-hidden="true"></i> Instruct Galaxy where to find a container for the tool.
* <i class="fa fa-check-square-o" aria-hidden="true"></i> Instruct Galaxy to run the tool in a container.

---

.enlarge120[

Infrastructure for build and finding containers for *all* best practice tools allows us to start doing some awesome things!

I'll talk about a few - CWL, Singularity, and container scheduling - and mention a few more in the Future Work slide.

]

---

## HPC Centers and the Docker Problem

.enlarge120[

HPC centers are incredibly important to Galaxy's past and future.

HPC centers mostly dislike Docker.

]

---

.center[![](images/singularity_paper.png)]

---

From Singularity's about page:

> <i class="fa fa-quote-left" aria-hidden="true"></i> ***Singularity blocks privilege escalation*** within the container so if you want to be root inside the container, you first must be root outside the container. This usage paradigm mitigates many of the security concerns that exists with containers on multi-tenant shared resources.<i class="fa fa-quote-right" aria-hidden="true"></i>

http://singularity.lbl.gov/about

---

.border[.center[![](images/singularity_mulled_pr.png)]]

.center[Build Singularity images beside Docker images.]

---

.border[.center[![](images/singularity_gx_pr.png)]]

.center[Uses the same find or build strategy as Docker.]

---

.border[.center[![](images/singularity_images.png)]]

.center[We've started publishing Singularity images.]

---

## Using Singularity (requires Galaxy 17.09)

- In `galaxy.ini`, set `enable_beta_mulled_containers = True`
- In `job_conf.xml`, configure destination

```
<destination id="short_fast" runner="slurm">
    <param id="singularity_enabled">true</parma>
</destination>
```

---

## We didn't forget about HPC - we made packages more robust for HPC clusters as we also integrated HPC friendly containerization techonology.

---

## Common Workflow Language Support

.border[.center[![](images/cwl_pr.png)]]

The same reproducibility stack using the same libraries can be integrated into CWL (Conda + BioContainers).

---

## Scheduling - From Clusters to Containers

**Cluster Jobs**

- Submit a script to execution infrastructure.
- Execution infrastructure is DRM (e.g. SLURM) schedules job script.
- Run directly on cluster node (bare metal or VM).

**Containerized Cluster Jobs (Before 2017)**

- Submit a script to execution infrastructure.
- Execution infrastructure is DRM (e.g. SLURM) schedules job script.
- *Script runs command via Docker host on cluster node.*

**Native Container Jobs (2017 and Beyond)**

- Submit container ID and command to infrastructure.
- Execution infrastructure (e.g. Kubernetes) schedules container execution directly.
- No need to manage clusters or Docker hosts.

---

## Docker Execution Goals

In moving from traditional cluster jobs to containerized cluster jobs the goal is *increased isolation and reproducibility*.

In moving from containerized cluster jobs to native container scheduling technologies is *elimination of cluster management software and delegating job scaling to service providers (e.g. Google and Amazon)*.

---

## Container Scheduling

From Amazon ECS

> <i class="fa fa-quote-left" aria-hidden="true"></i>Amazon ECS ***eliminates the need to install, operate, and scale your own cluster management infrastructure***, and allows you to schedule Docker-enabled applications across your cluster based on your resource needs and availability requirements. Amazon ECS enables you to ***grow from a single container to thousands of containers across hundreds of instances without any additional complexity*** in how you run your application.<i class="fa fa-quote-right" aria-hidden="true"></i>

https://aws.amazon.com/ecs/details/

---

### Container Scheduling - Three Current Galaxy Approaches

- Condor 
- Kubernetes
- Chronos for Mesos

---

class: center

### Container Scheduling - Condor

.center[.border[![](images/pr_condor.png)]]

---

class: center

### Container Scheduling - Kubernetes

.center[.border[![](images/pr_kubernetes.png)]]

---

class: center

### Container Scheduling - Chronos for Mesos

.center[.border[![](images/pr_chronos.png)]]

---

## Kubernetes

BoF tomorrow at lunch!

---

## Future Work

.enlarge120[

* Amazon EC2 Container Service
* GUI Management of Containers
* Infrastructure for Container Caching and Local Repository Mirrors

]
---

## Thanks

.enlarge120[ 
- Conda-ifying Galaxy and tools: Martin Cech, Brad Langhorst, and really the whole IUC and even beyond.
- Building container scheduling job runners: Pablo Moreno, Thodoris Sotiropoulos
- Multi-package hash explorer: Evgeny Anatskiy
- Common Workflow Language integration: James Taylor, Michael Crusoe, Peter Amstutz
]

---

###.image-25[![](images/conda_logo.png)] <br/> Conda distributions

```


```
![](images/miniconda_vs_anaconda.png)

---

###.image-25[![](images/conda_logo.png)] <br/> Channels

- Packages through channels within Continuum.Anaconda
- Conda channel searched by Galaxy for packages:
    - iuc
    - bioconda
    - defaults
    - conda-forge

---

###.image-25[![](images/conda_logo.png)] <br/> Conda Terminology


Conda **recipes** build **packages** that are published to **channels**.

---

## Containerizing Tool Execution

#### Decomposes into two basic problems:

.enlarge120[

* <i class="fa fa-square-o" aria-hidden="true"></i> Instruct Galaxy where to find a container for the tool.
* <i class="fa fa-check-square-o" aria-hidden="true"></i> Instruct Galaxy to run the tool in a container.

]
