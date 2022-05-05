# hehelm

## Developed to simplify integration and management of Kubernetes projects in GitLab.

### Usage
1. Create __.gitlab-ci.yml__ file in your project.
2. Copy content of [.gitlab-ci.yml-example](./.gitlab-ci.yml-example) to __.gitlab-ci.yml__ file in your project.
3. Set values in __"variables"__ section in __.gitlab-ci.yml__ file in your project.

__Note:__ \
It is possible to define __custom pipeline__ for project. To do that
1. Set ```CUSTOM_PIPELINE``` to ```"true"``` in __"variables"__ section in __.gitlab-ci.yml__ file in your project
2. Create file named __.$CI_PROJECT_NAME-gitlab-ci.yml__ in __pipelines__ directory in this project. \
Example: pipelines/.project-1-gitlab-ci.yml

*If the project URL is gitlab.example.com/group-name/project-1, CI_PROJECT_NAME is project-1.*



### Overrides

- #### Override application configs
  1. In project create __helm-app-configs__ directory and sub directories named as your environments (ex. test).\
  So for test environment configs it should look like: ```helm-app-configs/test```
  2. Add application config files there
  <br/><br/>
- #### Override helm template files
  1. In project create __helm-templates-override__ directory.
  2. Add your templates there. (Proper way of doing that is using templates in this repository as base files)
  <br/><br/>
- #### Override default values
  As only most changing variables exposed in .gitlab-ci.yml file you may want to edit others or even add new variables.
  1. In project create __helm-values-override.yaml__ file.
  2. Make changes. (Proper way of doing that is using values.yaml in this repository as base file)


### How it works
1. You start pipeline in your project.
2. It triggers child-pipeline using file in ./pipelines/ directory in this project.
3. During child-pipeline this project get cloned to your project repository named as helm-chart.
4. Helm chart get installed using variables you defined in .gitlab-ci.yml file in your project.

### Real use case
- Problem: \
There are too many Kubernetes projects in GitLab in X company. Each project has its own kubernetes manifest and gitlab files. Simple but not flexible, no ability to apply uniform pipeline policy or perform improvements as there need to manually change files in all projects, human factor and time waste during copy/paste when new project come.

- Solution: \
This project eliminates all problems described above as there centralized and standartized approach and at the same time we can perform individual approach for each project by describing aproppriate files in the project.