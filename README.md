# Uncertainty & Regression
## 1. Usage
### Configs
To specify the parameters and training strategies of the model, `yaml` file format
is being used. 


```yaml
estimator: # basic MLP layer configuration
  class: 'ensemble'
  num_networks : 5
  network:
    estimator_network:
      - fc1 : {class : Linear, in_features : 8, out_features : 50}
      - projection : {class : LinearVarianceNetworkHead, in_features : 50, out_features : 1}
    predictor_network: 
      - fc1 : {class : Linear, in_features : 8, out_features : 50}
      - projection : {class : Linear, in_features : 50, out_features : 1}
  optimizer:
    class : 'QHAdam'
    lr : 0.01
dataset:
  class: 'xls'
  path: "regression_datasets/Concrete_Data.xls"
  batch_size : 512  
  cv_split_num: 10
  test_ratio: 0.10
transforms:
  x :
    - {class : Standardize}
  y :
    - {class : Standardize}
train:
  train_type : epoch
  num_iter : 40
  weight_type : both
logger:
  type: 'wandb'
  project: 'uncertainty-estimation'
  entity: 'kbora'
  name: 'Toy Dataset Complex Weighted'
```
> Example config file for Concrete dataset. 

Similar to `mmdetection`, we allow anyone to define & use their own classes in any of the blocks. Simply, define your own class under the folder that your object belongs to and add corresponding class to the `*REGISTRY` dictionaries defined under each sub-modules `init.py` file.

## 2. File Structure
```
.
└── regression-uncertainty
    ├── LICENSE
    ├── README.md
    ├── configs
    │   ├── toy_dataset.yaml
    │   └── xls_dataset.yaml
    ├── datasets
    │   ├── __init__.py
    │   ├── toydata.py
    │   ├── toyfunc.py
    │   └── xlsdata.py
    ├── estimations
    │   ├── __init__.py
    │   └── ensemble.py
    ├── figures
    │   ├── non-weight.png
    │   ├── regression-uncertainty.png
    │   ├── weighted-ru.png
    │   └── weighted.png
    ├── tools
    │   └── train.py
    ├── utils
    │   ├── __init__.py
    │   ├── device.py
    │   └── logger.py
```
---
## Papers:
This section covers some of the well-known paper for imbalanced learning and uncertainity estimation for regression tasks.
### Imbalanced Regression:
Imbalanced regression are the collection of method that try to increase model performances for 
the areas where model in unsure due to the lack of training data.
- [Balanced MSE for Imbalanced Visual Regression](https://arxiv.org/abs/2203.16427) | [github]()
- [Density‑based weighting for imbalanced regression](https://link.springer.com/article/10.1007/s10994-021-06023-5) | []()
- [Delving into Deep Imbalanced Regression](https://arxiv.org/abs/2102.09554) | []()
### Uncertainity Estimation:
Uncertainity of the models and the data can be estimated with various methods which are usually
classified as (i) bayesian and (ii) non-bayesian methods.

#### (i) Bayesian Methods
- [Depth Uncertainty in Neural Networks](https://arxiv.org/abs/2006.08437) | [github](https://github.com/cambridge-mlg/DUN)
- [Dropout as a Bayesian Approximation: Representing Model Uncertainty in Deep Learning](https://arxiv.org/abs/1506.02142) | [github](https://github.com/cambridge-mlg/DUN) 

#### (ii) Non-bayesian Methods
- [Simple and Scalable Predictive Uncertainty Estimation using Deep Ensembles](https://arxiv.org/abs/1612.01474)

