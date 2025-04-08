### CICD for Machine Learning with Weights and Biases

The tutorial incorporates Weights and Baises with CICD for Machine Learning. ```Github Actions``` is a CICD automation process for traditional software engineering used in testing and deploying codes when there is an update of codes. Github Actions usage is limited in ML because ML infrastructure is much more than codes update ML also involves new data upload, data drift, model performance etc, and Github Actions cannot track every experiment required for ML models. To perform CICD for ML when there's a new data upload, data drift, model performance checks, code testing, experiment tracking, monitoring and deployment, ***Weights and Biases*** will be included alongside Github actions.

Weights and Biases can also be used to cretae reports, tables, artifacts and monitoring for ML infrastructure.

* [1. create_runs.ipynb](https://github.com/bluemusk24/cicd_wandb/blob/main/1.%20create_runs.ipynb) --> follow this notebook to create ```PROJECT and RUNS``` on Weights and Biases. Check Weights and Biases UI

* create a simple workflow template using the following Github action configurations [Github QuickStart Workflow](https://docs.github.com/en/actions/writing-workflows/quickstart). Paste this code below in ```.github/workflows/ci.yaml```. 

* Edit the workflow template to run hello world action, triggered on a push event.

```bash
mkdir .github/workflows

cd .github/workflows

touch ci.yaml

name: GitHub Actions Demo                                                                                         # name of the Github workflow

on: [push]                                                                                                        # github event to trigger a workflow

jobs:                                                                                                             # workflow jobs
  my-first-job:                                                                                                   # name of job
    runs-on: ubuntu-latest                                                                                        # os the workflow runs on
    steps:
      - name: hello                                                                                               # name of the first step to run
        run: |                                                                                                    # first step to run. use | for multiple lines
          echo "hello world"                                                                                   

```

* commit to Github repo and trigger the action. Click on Action on Github to see each run jobs.

### Python Script with Github Actions

```bash
python3 ci.py
```

* Update the ```ci.yaml``` file to run Python Script [ci.py](https://github.com/bluemusk24/cicd_wandb/blob/main/ci.py) with Github actions, using checkout actions. Github checkout action clones the contents in the current repo into ```actions/checkout/@v3```, to have access to the files in the repository.

```bash
name: GitHub Actions Demo                                                                                         

on: [push]                                                                                                        

jobs:                                                                                                             
  my-first-job:                                                                                                   
    runs-on: ubuntu-latest                                                                                        
    steps:
      - uses: actions/checkout@v3
      - name: hello                                                            
        run: |                                                                                                    
          echo "hello world"
      - name: run Python script
        run: |
          pip install -r requirements.txt
          python3 ci.py                                                                                   

git status

git add .

git commit -m 'create workflow that runs python script'

git push <git-repo-url>
```

### Github Actions Secrets

* Github action secrets allow the usage of sensitive information such as passwords or tokens inside your Github actions workflow, so as not to open the tokens for everyone to see. To create a secret on Github, ```On the github repo, go to settings, secrets and variables, actions, new repository secrets```

* To access the secrets from workflow, create a [secret.yaml]()

```bash
on: push

jobs:
  secrets:
    runs-on: ubuntu-latest
    steps:
    - run: echo $MY_VAR
      env:
        MY_VAR: ${{ secrets.MY_SECRET }}
```

* Print the length of the Github Secrets from the updated [secret.yaml](https://github.com/bluemusk24/cicd_wandb/blob/main/.github/workflows/secret.yaml)

```bash
on: push

jobs:
  secrets:
    runs-on: ubuntu-latest
    steps:
    - run: |
        import os
        print(len(os.getenv('MY_VAR')))
      shell: python
      env:
        MY_VAR: ${{ secrets.MY_SECRET }}
```

### Event Triggers

* Github action workflows can be triggered by different [event triggers](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows). Eg. ```push event, pull request event, workflow dispatch etc```. 
* Note: pull request and issues comment are similar events for workflows. ```Workflow dispatch event``` allows you to manually trigger a workflow. It can be handy especially for debugging.

* create a [trigger.yaml](https://github.com/bluemusk24/cicd_wandb/blob/main/.github/workflows/trigger.yaml) to accomodate branch and different events trigger. commit and check actions on Gitub.

```bash
name: trigger-demo
on:
  push:
    branches:
      - main
  pull_request:
  workflow_dispatch:

jobs:
  trigger-demo:
    runs-on: ubuntu-latest
    steps:
      - name: Just saying Hello
        run: echo "hello"
```

### Setting Up Github Actions Environment

* Basically, this is creating a boilerplate ```actions/checkout/@v3``` to clone all contents from a repo into it.

### Special Variables

* create [variables.yaml](). This contains special variables like: ```${{ github.event_name }}, ${{ github.ref }}, ${{ github.repository }}, ${{ github.actor }}, ${{ github.workspace }}, ${{ job.status }}, ${{ runner.os }} etc```. 

* For more info of variables starting with ```${{ github. }}```, read about [github context](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs#github-context).

* commit to repo and trigger the workflow