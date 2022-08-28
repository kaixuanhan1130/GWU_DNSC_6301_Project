# GWU_DNSC_6301_Project
Credit Line Increase Model Card

Basic Information
●Person or organization developing model: 
kaixuanhan2022@gmail.com
●Model date: August 21, 2022
●Model version: 1.0
●License: MIT
●Model implementation code: “link”

●Intended Use

●Primary intended uses:  This model is an example probability of default classifier, with an example use case for determining eligibility for a credit line increase.

●Primary intended users:  Students in GWU DNSC 6301 bootcamp

●Out-of-scope use cases:  Educational purpose only


Training data 

●Data dictionary:
Name	Modeling Role	Measurement Level	Description 
ID	ID	int	unique row indentifier
LIMIT_BAL	Input	float	amount of previously awarded credit
SEX	demographic information	int	1 = male; 2 = female
RACE	demographic information	int	1 = hispanic; 2 = black; 3 = white; 4 = asian
EDUCATION	demographic information	int	1 = graduate school; 2 = university; 3 = high school; 4 = others
MARRIAGE	demographic information	int	1 = married; 2 = single; 3 = others
AGE	demographic information	int	age in years
PAY_0, PAY_2 - PAY_6	Inputs	int	history of past payment; PAY_0 = the repayment status in September, 2005; PAY_2 = the repayment status in August, 2005; ...; PAY_6 = the repayment status in April, 2005. The measurement scale for the repayment status is: -1 = pay duly; 1 = payment delay for one month; 2 = payment delay for two months; ...; 8 = payment delay for eight months; 9 = payment delay for nine months and above
BILL_AMT1 - BILL_AMT6	Inputs	float	amount of bill statement; BILL_AMNT1 = amount of bill statement in September, 2005; BILL_AMT2 = amount of bill statement in August, 2005; ...; BILL_AMT6 = amount of bill statement in April, 2005
PAY_AMT1 - PAY_AMT6	Inputs	float	amount of previous payment; PAY_AMT1 = amount paid in September, 2005; PAY_AMT2 = amount paid in August, 2005; ...; PAY_AMT6 = amount paid in April, 2005
DELINQ_NEXT	Target	int	whether a customer's next payment is delinquent (late), 1 = late; 0 = on-time

●Source of training data: GWU Blackboard
●How training data was divided into training and validation data: 
            50% training, 25% validation, 25% testing
●Number of rows in training and validation data:
           Training data: 15000 rows 
           Validation data: 7500 rows 

Test Data

●Source of test data:GWU Blackboard
●Number of rows in test data: 7,500
●State any differences in columns between training and test data: None


Model details

●Columns used as inputs in the final model: 
 'LIMIT_BAL', 'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6', 'BILL_AMT1', 'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6', 'PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6'

●Column(s) used as target(s) in the final model: 
'DELINQ_NEXT'
●Type of model:
 Decision Tree Classification
●Software used to implement the model:
       Python, scikit-learn
●Version of the modeling software:
       Python version: 3.7.13 
sklearn version: 1.0.2
● Hyperparameters or other settings of your model 
       DecisionTreeClassifier(ccp_alpha=0.0, class_weight=None, criterion='gini',
                    max_depth=6, max_features=None, max_leaf_nodes=None,
                       min_impurity_decrease=0.0, min_impurity_split=None,
                       min_samples_leaf=1, min_samples_split=2,
                       min_weight_fraction_leaf=0.0, presort='deprecated',
                       random_state=12345, splitter='best')

Quantitative analysis 

●Metrics used to evaluate final model - AUC and AIR
	Training AUC	Validation AUC	5-Fold SD
1	0.645748	0.64388	0.009275
2	0.699912	0.687752	0.012626
3	0.742968	0.72949	0.017375
4	0.757178	0.741696	0.017079
5	0.769331	0.74248	0.019886
6	0.783722	0.74961	0.017665
7	0.795777	0.742115	0.022466
8	0.807291	0.73999	0.015567
9	0.822913	0.727224	0.012042
10	0.838052	0.720562	0.013855
11	0.855168	0.709864	0.010405
12	0.874251	0.688074	0.008073

From the table describing the AUC of training data and validation data, we choose the depth of 6 as the best model.

Then we use AIR to evaluate whether there exists data discrimination. In the Bias Testing, we found that the value of hispanic-to-white is 0.76, which is less than 0.8. In this case, there is discrimination towards hispanic. (The result is shown in the following table)

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

Therefore, we need to do the bias remediation to improve the performance of the 	model. After the remediation, we get the final AIR Value.

	Hispanic-to-White AIR
1	0.894148
2	0.850871
3	0.799546
4	0.792435
5	0.829336
6	0.833205
7	0.835886
8	0.8113
9	0.811561
10	0.803621
11	0.837806
12	0.844889

When the depth of tree is 6 which we select as the best model, the Hispanic-to-white AIR is 0.83>0.8, which is a satisfactory result. In conclusion, the depth of 6 is selected finally for the model.

●Description of the final values
(1)Final Model Values

Model with 6 Depths
Training AUC - 0.783722
Validation AUC - 074961
Test AUC-0.7438
Hispanic - White AIR - 0.833205

(2)Histogram of each column

This figure demonstrates the distribution of each column which provides visualized insights of features. For instance, in the sex column, ‘1’, ‘2’ refers to male and female and the y-axis refers to the number of each gender. Detailed information of variables in each figure are interpreted in Data Dictionary.

(3)Correlation Heatmap of data
 		   
The figure demonstrates the correlation of each variable. Deeper the color is, more negatively related two variables are, ranging approximately in (-0.4,1). 

(4)Iteration Plot for Depth vs Training and Validation AUC

The figure demonstrates the AUC score of training and validation data. The AUC score represents how the model would performance in predicting unseen data. For training data, the AUC score increase with the depth of tree. For validation data, the AUC score first increases, reaches the top at the depth of 6 and then decreases. Therefore, the depth of 6 is chosen for the built of the best model.

(5)Decision Tree for human interpretation


(6)Plot for variable Importance
          
The figure demonstrates the importance of the variables ranked by the importance. The importance of the variable refers to how it could influence on the prediction results. Therefore, the variables ranked higher in this figure is more crucial of determining eligibility for a credit line increase.

(7)Final Iteration Plot with AIR 

The figure demonstrates that at the depth of 6, the Hispanic-to-White AIR is over 0.8, which means our model is free from data discrimination.
          


