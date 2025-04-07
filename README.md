### CICD for Machine Learning with Weights and Biases

* The tutorial incorporates Weights and Baises with CICD for Machine Learning. ```Github Actions``` is a CICD automation process for traditional software engineering,

used in testing and deploying codes when there is an update of codes. Github Actions usage is limited in ML because ML infrastructure is much more than codes update.

ML also involves new data upload, data drift, model performance etc, and Github Actions cannot track every experiment required for ML models. To perform CICD for ML

when there's a new data upload, data drift, model performance checks, code testing, experiment tracking, monitoring and deployment, ***Weights and Biases*** will be

included alongside Github actions.

Weights and Biases can also be used to cretae reports, artifacts and monitoring for ML infrastructure.


* [1. create_runs.ipynb]() --> follow this notebook to create ```PROJECT and RUNS``` on Weights and Biases. Check Weights and Biases UI