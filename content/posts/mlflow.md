---
title: "Mlflow"
date: 2022-07-24T22:05:56+02:00
---
## MLFlow:
It is a platform that offers different services to manage the ML lifecycle, including experimentation, reproducibility and deployment. It currently offers four main components:
### 1. MLflow tracking:

Record and query experiments. For example, we can store evaluation metric, number of features, the whole notebook against each run of our model. So, we can keep track of stuff happening when we change a certain thing during training.

### 2. MLflow Projects:
Package the whole code to reproduce on any platform.

### 3. MLflow Models:
Deploy models in diverse environments.

### 4. Model Registry:
Store, annotate, and manage models in a central repository.

Hands on tutorial is inside the below mentioned notebook.
In this notebook, first, I have implemented xgboost regressor for the time-series data and then use MLFLow to track the model parameters, metrics and the model itself. Then registered that model in MLFlow to expose it to get prediction from _>request(url).

[MLFlowNotebook](https://github.com/sudo-umar/python/blob/main/mlflowtutorial%20-%20Jupyter%20Notebook.pdf)

### Further integration with EC2 and S3 bucket to show all the plots and parameters in MLFlow on AWS:

It can be done as shown in the youtube video mentioned below.

[S3andAWS-timestamp:17:00 onwards](https://www.youtube.com/watch?v=osYRsBVId-A)

