# Home Credit Scorecard Model
Home Credit Indonesia Data Scientist Virtual Internship Program by Rakamin Academy

### *Problem Statement*:
Many people struggle to get loans due to insufficient or non-existent credit histories. And, unfortunately, this population is often taken advantage of by untrustworthy lenders.

Home Credit strives to broaden financial inclusion for the unbanked population by providing a positive and safe borrowing experience. In order to make sure this underserved population has a positive loan experience, Home Credit makes use of a variety of alternative data--including telco and transactional information--to predict their clients' repayment abilities.

While Home Credit is currently using various statistical and machine learning methods to make these predictions. Doing so will ensure that clients capable of repayment are not rejected and that loans are given with a principal, maturity, and repayment calendar that will empower their clients to be successful.

### *Goal*: 
Minimize the number of clients who are rejected but actually have the capability of repayment.

### *Objective*:
Create predictive model to determine potential client and default client.

### *Business Metrics*:
Decreased Loss Given Default (LGD).

The amount of money a financial institution loses when a borrower defaults on a loan, after taking into consideration any recovery, represented as a percentage of total exposure at the time of loss.

Source: https://www.investopedia.com/terms/l/lossgivendefault.asp#:~:text=Loss%20Given%20Default%20(LGD)%20FAQs,-What%20Does%20Loss&text=Given%20Default%20Mean%3F-,Loss%20given%20default%20(LGD)%20is%20the%20amount%20of%20money%20a,at%20the%20time%20of%20loss.

Assumption: losses due to default are only calculated from the client's total credit.

### *Model Evaluation*
Area under the ROC curve.

### *Datafile Selection*
Three datafiles are selected for modeling:

1. **application_train**: main dataset with TARGET variable. Every loan has its own row and is identified by the feature SK_ID_CURR.
2. **bureau**: All client's previous credits provided by other financial institutions that were reported to Credit Bureau (for clients who have a loan in our sample). For every loan in the sample, there are as many rows as number of credits the client had in Credit Bureau before the application date.
3. **previous_application**: all previous applications for Home Credit loans of clients who have loans in the sample. There is one row for each previous application related to loans in the data sample.
I define two new columns from previous_application.csv and bureau.csv then joined the columns with application_train main dataset. 

After the model is obtained, default probability for the TARGET variable predicted for each SK_ID_CURR in the **application_test** set.

#### Define new columns from previous_application.csv and bureau.csv
- TOTAL_BUREAU_LOAN: Total of credits the client had in Credit Bureau.
- TOTAL_PREV_APP: Total previous applications Home Credit loans of clients.

![image](https://user-images.githubusercontent.com/93385657/159718935-622f05d3-fa90-450b-b0bc-ec4fc082ce5b.png)

### *Exploratory Data Analysis*
**Basic info of the clients**

![image](https://user-images.githubusercontent.com/93385657/159715953-e4f637d5-5cc2-4eb9-8de0-41eaa8574910.png)

![image](https://user-images.githubusercontent.com/93385657/159716198-e887bcbb-0499-4ad0-a379-96c2fcc6423b.png)

### *Feature Engineering*:
Seven new features made from existing features

### *Data Preprocessing*:
- Main dataframe has a total of 307511 rows and 122 columns
- TARGET column: 1 is client with payment difficulties (default), 0 is client with good repayment abilities (not default)
- Replace XNA values with NaN
- No duplicate data
- Keep columns that include less than equal to 60% of missing values
- Handling missing values using imputation
- Scaling numerical features using .MinMaxScaler()
- Feature encoding using Label Encoding for any categorical variable with 2 unique categories and One Hot Encoding for more than 2 unique categories

### *Aligning Data Test and Data Train*:
One-hot encoding has created more columns in the training data because there were some categorical variables with categories not represented in the testing data. To remove the columns in the training data that are not in the testing data, we need to align the dataframes.

### *Feature Selection*:
Based on:
- VIF (Variance Inflation Factor): Drop Features with VIF > 10 (indicator of multicollinearity)
- Correlation with Target: After trying several combinations, I decide to pick 15 features which most correlated with the TARGET
- Domain Knowledge: 11 feature selected

### *Modeling*:
- Splitting data train and data test with ratio 80 : 20
- Handling imbalance using oversampling SMOTE with ratio 2 : 1

![image](https://user-images.githubusercontent.com/93385657/159719585-7d1e4696-7daa-4cee-8736-ced18de3842e.png)


- Performance of the models tested

  Metric evaluation: area under ROC curve
  
![image](https://user-images.githubusercontent.com/93385657/159722658-1d8623e1-aae8-4faf-9c25-0bb817193408.png)

### *Summary*:
- **Best Model**: Logistic Regression with AUC 0.63
- **Potential impact**: 
  Decresed Loss Given Default (LGD)
  The amount of money a financial institution loses when a borrower defaults on a loan, after taking into consideration any recovery, represented as a percentage of total exposure at the time of loss.
  
  **Assumption**: losses due to default are only calculated from the client's total credit.

### *Business Suggestion*:
Based on top 4 feature importance from the model

![image](https://user-images.githubusercontent.com/93385657/159723668-ed139f3e-1b2c-4a94-bd1f-d648014af5a5.png)

1. EXT_SOURCE3 can be used as one of the most important indicators in determining client’s repayment abilities as it is the most important feature from the model

  ![image](https://user-images.githubusercontent.com/93385657/159731684-79e12b32-57c5-41ae-910b-0967e0980c12.png)

2. It is important to ensure the client's ability, especially for clients who purchase a larger income annuity

  ![image](https://user-images.githubusercontent.com/93385657/159732232-a40aca3e-5ddf-49ef-944f-c41072265379.png) 

3. Young clients, which means that their current employment is not long, they have more potential to not repay the loan. Maybe they should be provided with more guidance or financial planning tips

  ![image](https://user-images.githubusercontent.com/93385657/159732424-7ac92116-7f89-4e99-8272-8ca4356105d0.png) 
