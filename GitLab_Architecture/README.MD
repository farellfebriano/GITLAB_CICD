# GIT LAB ARCHITECTURE

## What is a runners.

Program that is install on a machine, that seperate from the one that hosts the gitlab instance. So basically a runner is a mediator that will fetch the job from the gitlab server and hand over the commands of the job to the shell of the server that it's running on.

## What is an executor

An executor refers to the component or environment responsible for running the jobs, tasks, or pipelines defined in the CI/CD process. <br/>
types of executor:

<ul>
  <li>shell</li>
  <li>docker</li>
  <li>Virtual Machine</li>
</ul>

## Job execution flow

![CICD_in_highlevel](https://github.com/farellfebriano/GITLAB_CICD/blob/main/GitLab_Architecture/Image/gitlab_execution_flow.png?raw=true)

1.) Runner request new jobs from GITLAB instance <br/>
2.) Rnner complies and sends the job's payload to executor <br/>
3.) Excutor clones source or downloads artifacts from gitlab instance and executes the job <br/>
4.) Executor returns job output and status to the Runner <br/>
5.) Runner updates job outoyt and status to Gitlab instance <br/>

## Docker Execution

With using a docker execution each jobs will use fresh docker container. Bellow are the example on how to specify image to execute the command. <br/>

The code bellow will specified what image that will use for entire jobs.
```yaml
image: node:17-alpine
...
```
To specify what image to use each job. simply use `image` in the job level.
```yaml
run_unit_tests:
  image: node:17-alpine
  stage: test
  script:
    - echo "runing test...."
...
```
In condition where a runner dosent have a docker, the image will simply be ignored and will run under shell executor.

## Specific Runner: Runner for specific project

![CICD_in_highlevel](https://github.com/farellfebriano/GITLAB_CICD/blob/main/GitLab_Architecture/Image/runner_or_specific_project.png?raw=true?raw=true)
