# Credit Line Increase Model Card

### Basic Information

* **Person or organization developing model**: Kaixuan Han, `ferryman1130@gmail.com`
* **Model date**: August, 2021
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: [DNSC_6301_Project_Group5.ipynb](https://github.com/kaixuanhan1130/GWU_DNSC_6301_Project/blob/main/DNSC6301_project_group5.ipynb)

### Intended Use
* **Primary intended uses**: This model is an *example* probability of default classifier, with an *example* use case for determining eligibility for a credit line increase.
* **Primary intended users**: Students in GWU DNSC 6301 bootcamp.
* **Out-of-scope use cases**: Any use beyond an educational example is out-of-scope.

### Training Data

* Data dictionary: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**ID**| ID | int | unique row indentifier |
| **LIMIT_BAL** | input | float | amount of previously awarded credit |
| **SEX** | demographic information | int | 1 = male; 2 = female
| **RACE** | demographic information | int | 1 = hispanic; 2 = black; 3 = white; 4 = asian |
| **EDUCATION** | demographic information | int | 1 = graduate school; 2 = university; 3 = high school; 4 = others |
| **MARRIAGE** | demographic information | int | 1 = married; 2 = single; 3 = others |
| **AGE** | demographic information | int | age in years |
| **PAY_0, PAY_2 - PAY_6** | inputs | int | history of past payment; PAY_0 = the repayment status in September, 2005; PAY_2 = the repayment status in August, 2005; ...; PAY_6 = the repayment status in April, 2005. The measurement scale for the repayment status is: -1 = pay duly; 1 = payment delay for one month; 2 = payment delay for two months; ...; 8 = payment delay for eight months; 9 = payment delay for nine months and above |
| **BILL_AMT1 - BILL_AMT6** | inputs | float | amount of bill statement; BILL_AMNT1 = amount of bill statement in September, 2005; BILL_AMT2 = amount of bill statement in August, 2005; ...; BILL_AMT6 = amount of bill statement in April, 2005 |
| **PAY_AMT1 - PAY_AMT6** | inputs | float | amount of previous payment; PAY_AMT1 = amount paid in September, 2005; PAY_AMT2 = amount paid in August, 2005; ...; PAY_AMT6 = amount paid in April, 2005 |
| **DELINQ_NEXT**| target | int | whether a customer's next payment is delinquent (late), 1 = late; 0 = on-time |

* **Source of training data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **How training data was divided into training and validation data**: 50% training, 25% validation, 25% test
* **Number of rows in training and validation data**:
  * Training rows: 15,000
  * Validation rows: 7,500

### Test Data
* **Source of test data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **Number of rows in test data**: 7,500
* **State any differences in columns between training and test data**: None

### Model details
* **Columns used as inputs in the final model**: 'LIMIT_BAL',
       'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6', 'BILL_AMT1',
       'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6',
       'PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6'
* **Column(s) used as target(s) in the final model**: 'DELINQ_NEXT'
* **Type of model**: Decision Tree 
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: 0.22.2.post1
* **Hyperparameters or other settings of your model**: 
```
DecisionTreeClassifier(ccp_alpha=0.0, class_weight=None, criterion='gini',
                       max_depth=6, max_features=None, max_leaf_nodes=None,
                       min_impurity_decrease=0.0, min_impurity_split=None,
                       min_samples_leaf=1, min_samples_split=2,
                       min_weight_fraction_leaf=0.0, presort='deprecated',
                       random_state=12345, splitter='best')
```
### Quantitative Analysis
* **Metrics used to evaluate final model - AUC and AIR**
<img width="492" alt="截屏2022-08-29 08 13 03" src="https://user-images.githubusercontent.com/112351745/187100863-15ff36fb-a913-4ecb-bcc1-c5a08a1392db.png">
 From the table describing the AUC of training data and validation data, we choose the depth of 6 as the best model.<br>

 Then we use AIR to evaluate whether there exists data discrimination. In the Bias Testing, we found that the value of hispanic-to-white is 0.76, which is lower than 0.8. In this case, there exists discrimination towards hispanic. (The result is shown in the following table)<br>
```
White proportion accepted: 0.568
Hispanic proportion accepted: 0.434
hispanic-to-white AIR: 0.76

White proportion accepted: 0.568
Black proportion accepted: 0.465
black-to-white AIR: 0.82

White proportion accepted: 0.568
Asian proportion accepted: 0.568
asian-to-white AIR: 1.00


Male proportion accepted: 0.503
Female proportion accepted: 0.533
female-to-male AIR: 1.06
```
 Therefore, we need to do the bias remediation to improve the performance of the 	model. After the remediation, we get the final AIR Value.
 
<img width="298" alt="截屏2022-08-29 08 23 57" src="https://user-images.githubusercontent.com/112351745/187101344-76bad964-eb79-4aef-8135-227844b3dd22.png">

 When the depth of tree is 6 which we select as the best model, the Hispanic-to-white AIR is 0.83>0.8, which is a satisfactory result. In conclusion, the depth of 6 is selected finally for the model.
 
* **Description of the final values**

   **(1)Final Model Values**
```
Model with 6 Depths
Training AUC - 0.783722
Validation AUC - 074961
Test AUC-0.7438
Hispanic - White AIR - 0.833205
```

   **(2)Histogram of each column**
![下载](https://user-images.githubusercontent.com/112351745/187101797-7f2246ca-a6e2-4789-ac1e-c1ce7b5da4b3.png)

This figure demonstrates the distribution of each column which provides visualized insights of features. For instance, in the sex column, ‘1’, ‘2’ refers to male and female and the y-axis refers to the number of each gender. Detailed information of variables in each figure are interpreted in Data Dictionary.

   **(3)Correlation Heatmap of data**
   
![heatmap](https://user-images.githubusercontent.com/112351745/187102023-2516ad4b-647f-496f-856f-22cf6e933590.png)

The figure demonstrates the correlation of each variable. Deeper the color is, more negatively related two variables are, ranging approximately in (-0.4,1). 

   **(4)Iteration Plot for Depth vs Training and Validation AUC**
   
![Tr Va AUC](https://user-images.githubusercontent.com/112351745/187102172-b8b07423-4abe-4d06-837b-8ee0463b6335.png)

The figure demonstrates the AUC score of training and validation data. The AUC score represents how the model would performance in predicting unseen data. For training data, the AUC score increase with the depth of tree. For validation data, the AUC score first increases, reaches the top at the depth of 6 and then decreases. Therefore, the depth of 6 is chosen for the built of the best model.

   **(5)Decision Tree for human interpretation**
   
![decision tree](https://user-images.githubusercontent.com/112351745/187102270-23f9c814-1bc8-4208-90c4-0d30c2bf0300.png)

   **(6)Plot for variable Importance**

![variable importance](https://user-images.githubusercontent.com/112351745/187102296-62e4fa8d-a38f-434f-8feb-772331d13009.png)

The figure demonstrates the importance of the variables ranked by the importance. The importance of the variable refers to how it could influence on the prediction results. Therefore, the variables ranked higher in this figure is more crucial of determining eligibility for a credit line increase.

   **(7)Final Iteration Plot with AIR**
   
 ![3 AUC](https://user-images.githubusercontent.com/112351745/187102327-eed404a8-fc03-4edd-9689-323384e7e1c6.png)

The figure demonstrates that at the depth of 6, the Hispanic-to-White AIR is over 0.8, which means our model is free from data discrimination.

### Ethical considerations
* **Potential negative impacts of using the model**
**i.Math or software problems** 

(1)The model only consider one parameter(max_depth) of the DecisionTreeClassifier. Failure to consider other parameters might lead to a decrease in prediction accuracy.

(2)Decision Tree does not perform well when dealing with data of large scale, especially with continuous independent variables. A potential improvement, which the project fails to implement, could be applying other models including random forest and neural network.

**ii.Real-world risks** 

The model used in the project generates the result that AUC is 0.7438 on test data, which might be unsatisfactory for particular companies whose business is highly sensitive to  debit and credit. A slight change on the precision of the model performance could result in huge shift of company’s profit.

* **Potential uncertainties relating to the impacts of using your model**

**i.Math or software problems**

The model only consider 26 variables to evaluate one’s credit. With the development of big data technology, more kinds of data would be added to evaluate one’s credit. However, the model used in the project might not accommodate these new variables, which may lead to a decline on the performance of the model. Therefore, whether the model could perform well with potential varied datasets in the future is a major uncertainty, especially in the era of information explosion.

**ii.Real-world risks** 

Companies may benefit from model of good performance and therefore they are inclined to collect as much data as they can to improve the model, which might lead to data privacy issues such as data leaking and illegal data trading.
