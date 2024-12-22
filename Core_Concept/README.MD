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

<span style="text-decoration: underline;"><u>JOBS:<u/></span>
<br/>
Jobs are the discrete tasks or steps that are executed within a pipeline. It define a specific job like (building an application, Running test suites, linting code, packaging artifacts, deploying to a server).

<u>COMMANDS:<u/>
<br/>
Refers to the command or instruction that are executed as part of each job in the pipeline. Bellow are example of 3 building blocks of command. before_script, script, after_script.

```yaml
run_unit_tests:
  stage: test
  before_script: # will run before the test data
    - echo "Preparing the test data"
  script:
    - echo "runing test...."
  after_script:
    - echo "cleaning the data test"
...
```

<u>STAGES:<u/>
<br/>
Refers to phase or steps in the pipline and control the flow of task(jobs). each stage is logical grouping of jobs that execute similar tasks, such as building, testing, and deploying. Bellow we differ jobs between test, build, and deploy

```yaml
stages:
  - test
  - build
  - deploy 
...
```

<u>JOB DEPENDENCIES ("needs"):<u/>
<br/>
In certain situation, you want to run a certain job before other job (job's dependenciest). Hence, using command "needs" are require for doing so. In the example bellow, build image is required to run before the push image.

```yaml
build_image:
  stage: build

push_image:
  stage: build
  needs: # it needs build_image jobs to run before push image runs
    - build_image 
...
```

EMBEND SCRIPT:
<br/>
In case that your script has multiple line, we sugest that you embend the script to make the pipline more readable. 

```yaml
run_unit_tests:
  stage: test
  script: # will run before the test data
    - echo "Preparing the test data"
    - chmod +x ./prepare-test.sh # CAUTIONS!!! need to change the permission of the script first 
    - ./prepare-test.sh # this is where we run the code 
...
```

JOBS CONTROL ON SPECIFIC BRANCH
<br/>
There are some cases that we dont want some jobs to run. For, example when we createa branch and we commit the code, we dont want the build_image and push_image to run since the code needs more modification. Hence we only wants the test jobs that is running since we might still create changes to the code. Hence we can use "only" or "execpt" command. 
<br/>

"only"    : we specifiying if the jobs runs "ONLY"  on a or multiple branch<br/>
"except"  : we specifiying if the jobs runs "EXCEPT" on a or multiple branch

```yaml
# in this example we wan to run run_lint_tests jobs on all branches "EXCEPT" main 
# and run build_image jobs only on main branch
run_lint_tests:
  stage: test
  except:
    - main
  script:
    - ...

build_image:
  stage: build
  only:
      - main
  script:
    - ...
...
```

WORKFLOW RULES
<br/>
In certain case we want to create conditional statement on all the jobs in one file. Hence we use "rules" command. This is used when we want to add conditional situation to run jobs in a global socpe, not in individual jobs. 
<br/>


gitlab variable: 
<br/>
https://docs.gitlab.com/ee/ci/variables/predefined_variables.html


<br/>

```
# in this example, if is not in the main brach dont excude all the code
# $CI_COMMIT_BRANCH is variable that gitlab provide. 
workflow:
  rules:
    - if: $CI_COMMIT_BRANCH != "main"
      when: never
    - when: always
.... 
```

PULL REQUEST
<br/>
In this case we want to make sure to test the code on pull request event. Hence we will use "rules" jobs to trigered the jobs only run when its in the main and also if there are pull request event.

```
workflow:
  rules:
    - if: $CI_COMMIT_BRANCH != "main" &&  $CI_PIPELINE_SOURCE !="merge_request_event"
      when: never
    - when: always
...
```

