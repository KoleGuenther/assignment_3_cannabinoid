
## Overall Setup
- Tasks: Regression (cb1_p) and Binary Classification (cb1_active)
- Models: 2 FNNs
- Feature Sets: Morgan, MACCS, RDKit desc, ALL
- Evaluation: 5-fold cv
- Tuning: Hyperparameter GridSearch
- Sample Size: 0.4

# Regression

## Regression Setup
- Feature Sets: Morgan, MACCS, RDKit desc, ALL
- 5-fold cv

Gridsearch:
```
reg_grid = {
    "hidden_layers": [(64,), (128, 64)],
    "activation": ["relu", "tanh"],
    "dropout": [0.2],
    "learning_rate": [1e-3, 5e-4],
    "batch_size": [32],
    "epochs": [20]
}
```



## Regression Results

Across all feature sets, desc had the single best model, while 'all' had the best-performing models on average.
The table below summarizes performance metrics (RMSE, MAE, and R²) for each feature set:
<img width="1084" height="135" alt="image" src="https://github.com/user-attachments/assets/fcaa84ac-762c-426e-9794-50d9b16b2ffb" />

The following table shows the top 15 models ranked by lowest RMSE:
<img width="1268" height="413" alt="image" src="https://github.com/user-attachments/assets/298adc66-8899-4184-8612-9d8013bf4f9d" />

The top two best models (desc & all) had these hyperparameters:
```
{
'activation': 'relu',
'batch_size': 32,
'dropout': 0.2,
'epochs': 20,
'hidden_layers': (128, 64),
'learning_rate': 0.001
}
```

## Regression Graphs
<img width="533" height="470" alt="image" src="https://github.com/user-attachments/assets/19603009-6255-4fe0-a647-c00a32c83e51" />
<img width="531" height="470" alt="image" src="https://github.com/user-attachments/assets/e9de16ca-c9c3-4c6b-b5b4-76a9520eeea0" />


## Interpretation
Although the descriptor-based model achieved the best single performance, the combined feature set consistently ranked among the top configurations. This suggests that integrating multiple feature types provides a robust representation, even if it does not always yield the absolute best model.

Looking at the graphs, you can tell that the combined descriptor dataset (all), has a much easier time getting closer to the predicted value on average, which is easier seen on this graph, rather than through the table.

Performance differences between feature sets were relatively small, indicating that the neural network was able to extract similar levels of predictive signal from each representation.

One limitation is that high-dimensional feature sets, such as Morgan fingerprints and combined features, may require larger sample sizes to fully realize their potential. Under reduced sample conditions, descriptor-based features may perform slightly better due to a more favorable feature-to-sample ratio.

Assuming a 1.0 sample size, this might favor Morgan fingerprints and combined features over any other feature set.


# Classification

## Classification Setup
- Feature Sets: Morgan, MACCS, RDKit desc, ALL
- 5-fold cv

Gridsearch:
```
cls_grid = {
    "hidden_layers": [(64,), (128, 64)],
    "activation": ["relu", "tanh"],
    "dropout": [0.2],
    "learning_rate": [1e-3, 5e-4],
    "batch_size": [32],
    "epochs": [20]
}
```

## Classification Results

Across all feature sets, 'morgan' had the single best model, AND the best performing models on average.
The table below summarizes classification performance metrics (Accuracy, Precision, Recall, F1-score, and ROC-AUC):
<img width="1852" height="192" alt="image" src="https://github.com/user-attachments/assets/215433fa-30df-4fd4-ad16-a9f26c57596d" />


The following table shows the top 15 models ranked by F1-score:
<img width="1509" height="410" alt="image" src="https://github.com/user-attachments/assets/85be3be6-1370-4770-8b9e-805fa9e5a84b" />

The best model (Morgan) had these hyperparameters:
```
{
'activation': 'tanh',
'batch_size': 32,
'dropout': 0.2,
'epochs': 20,
'hidden_layers': (128, 64),
'learning_rate': 0.001
}
```

## Classification Graphs
<img width="479" height="393" alt="image" src="https://github.com/user-attachments/assets/f51b2828-a916-46c5-b78f-8844bf78d694" />
<img width="479" height="393" alt="image" src="https://github.com/user-attachments/assets/b7b4ff16-1763-4719-84f6-a476928965e9" />
<img width="479" height="393" alt="image" src="https://github.com/user-attachments/assets/15e62ae0-ffeb-42f6-bf11-2e2a2d3c8668" />

<img width="536" height="470" alt="image" src="https://github.com/user-attachments/assets/53e0cf25-94c7-43f8-b33e-b873545fc62f" />
<img width="536" height="470" alt="image" src="https://github.com/user-attachments/assets/ba36ce9d-5f00-4e23-bff4-f501c2e9bb82" />
<img width="536" height="470" alt="image" src="https://github.com/user-attachments/assets/d3e779ba-101c-4034-bbb4-3c21004e57e7" />
<img width="536" height="470" alt="image" src="https://github.com/user-attachments/assets/10808826-60cd-42fd-a8da-3ab05699329d" />



## Interpretation
Classification performance was stronger and more stable than regression, suggesting that the underlying signal in the dataset is more effectively captured in a categorical form (active vs inactive) rather than as a continuous binding affinity value. (Although as we tested, these numbers can be slightly exaggerated because being correct, is often much easier than being close to right)

When looking at the confusion matrices, you can see a much worse performance with 'desc' in classification, getting beat out by a ton when compared to 'morgan' and 'all' feature sets.

When looking at the ROC graphs, you immediately can tell that the higher dimensionality feature sets are better in classification.

Morgan fingerprints outperformed other feature sets in classification, likely because they capture the complex structural patterns that are highly relevant for distinguishing chemical activity.

In contrast to regression, where feature performance was similar across representations, classification showed a clearer advantage for a specific feature type.

# Final Comparison
Regression and classification have very different results.

Regression had an overall moderate performance, with small differences between feature sets and honestly modest R² values of about 0.45. This suggests that predicting continuous values such as binding affinity is much more challenging than a classification task for this dataset and mdoel.

In contrast, classification performance was a lot stronger and more consistent, indicating that the model was able to easily distinguish between the active and inactive categorical compounds than predicting exact binding affinity as a value.

These results show how different feature sets can be better suited for different tasks, especially between regression and classification.
