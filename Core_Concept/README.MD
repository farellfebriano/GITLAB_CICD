# GIT LAB Core Concept


## CICD in high level
![CICD_in_highlevel](https://github.com/farellfebriano/GITLAB_CICD/blob/main/Core_Concept/Image/CICD_in_highlevel.png?raw=true)

### CI and CD
![CICD_in_highlevel](https://github.com/farellfebriano/GITLAB_CICD/blob/main/Core_Concept/Image/CI_and_CD.png?raw=true)

## Software Version-ing Technique 
![CICD_in_highlevel](https://github.com/farellfebriano/GITLAB_CICD/blob/main/Core_Concept/Image/software_versioning.png?raw=true)

Software versioning technique:
<ul>
  <li>Major change</li>
  <li>Minor change</li>
  <li>patch</li>
</ul>

## Continous Deployement
![CICD_in_highlevel](https://github.com/farellfebriano/GITLAB_CICD/blob/main/Core_Concept/Image/continous_dployement.png?raw=true)
<br/>
It is important that we do code analysis test, line test to make sure that the code are clean and also maintainable to be passed to the next developer. 

## Building Blocks of Pipeline

jobs: 
<br/>
jobs are the discrete tasks or steps that are executed within a pipeline. It define a specific job like (building an application, Running test suites, linting code, packaging artifacts, deploying to a server).

commands:
<br/>
refers to the command or instruction that are executed as part of each job in the pipeline.

stages:
<br/>
refers to phase or steps in the pipline and control the flow of task(jobs). each stage is logical grouping of jobs that execute similar tasks, such as building, testing, and deploying. 

dependenciest between jobs under the same stage ("needs"):
<br/>
in certain situation, you want to run a certain job before other job (job's dependenciest). Hence "using command needs are require for doing so. In the example bellow, build image is required to run before the push image.

```yaml

build_image:
  stage: build

push_image:
  stage: build
  needs: # it needs build_image jobs to run before push image runs
    - build_image 
```



