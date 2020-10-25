---
layout: default
title: "Automated Feature Selection"
date: 2020-10-25 17:41:00
tags: machinelearning
published: true
---
Feature selection is an important topic in machine learning. The more features you have, the more likely you are to overfit and fail to generalize to your cross-validation set. Additionally, with fewer features, your model can be more performant (faster).
One way that you can use feature selection is manually prune the features - i.e., remove or add a feature one-by-one, and see how the score changes. The problem with this is that it is very laborious.

The first page I would read as a starting point is Sklearn's page on feature selection: https://scikit-learn.org/stable/modules/feature_selection.html

Your options are: Recursive Feature Elimination, Boruta, ELI5, Permutation Importance.
You could also try writing a loop and trying various combinations of features on cross-validation.

If you have a lot of time and compute, you can try ExhaustiveFeatureSelector: http://rasbt.github.io/mlxtend/api_subpackages/mlxtend.feature_selection/#exhaustivefeatureselector

If you want a quick answer, you could run SHAP on your features and remove the least important features. SHAP is nice because you can cut off any features that generally have low impact anyway, but that's determined by the amount of movement of your predictions, not the quality of that movement. You might eliminate features that only make small movements to the prediction but are actually of very high quality

Ahmet has created a model agnostic validation scheme called LOFO Importance: https://github.com/aerdem4/lofo-importance
One of the benefits of LOFO Importance is that you can group features together. This is useful if you have OHE or TF-IDF features.

Another way is to use forward feature selection with Ridge Regression (trying ti improve AIC or BIC)

There is a paper about Null Importances which I find is also reliable. You can read it here: https://academic.oup.com/bioinformatics/article/26/10/1340/193348
Basically, for each feature, you refit the model but with the column shuffled, and compare the feature importance before and after. If the feature remains as/more important as last time, then you can perceive the column as unimportant.

Remember to always have your validation set be reflective of your future testing set. Or else you may begin to even overfit to the validation set!
