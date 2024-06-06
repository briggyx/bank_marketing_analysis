# Bank Marketing Analysis & Dashboard 
 
-------------------------------
### Abstract




-----------------------------
### Table Of Contents

1. [Repository Structure](#repository)
2. [Dataset](#dataset)
3. [Dashboard](#features)
4. [Software Requirements](#installation)
5. [Design Process](#design)
6. [Analysis & Key Findings](#analysis)
7. [Concluding Remarks](#conclusion)
8. [Contact](#contact)
9. [Acknowledgements & Sources](#acknowledgements)
10. [License](#license)


----------------------------------
### Repository Structure<a name="repository"></a> 


-------------------------------
### Software Requirements<a name="installation"></a>
The version of Excel that I used is [Professional Plus 2021](https://learn.microsoft.com/en-us/deployoffice/ltsc2021/overview), which requires Windows 10/11.

----------------------------------
### Dataset<a name="dataset"></a>
 [UC Irvine ML Library-- Bank Marketing]('https://archive.ics.uci.edu/dataset/222/bank+marketing')


The dataset concerns the direct marketing initiatives conducted by a bank in Portugal, primarily executed through phone calls. It was common for multiple attempts to be made to engage the same client, aimed at having them subscribe to a bank term deposit. The original intention of this dataset was for creating machine learning models to predict whether a client will subscribe to a term deposit (last column).

**What is a term deposit and how is it important?**
> When funds are deposited in a regular bank account, the bank can lend the funds to others. In exchange for this, depositors receive interest on their balance. Most accounts allow withdrawals at any time, making it hard for banks to predict lending capacity. To address this, banks offer **term deposit accounts**. Customers agree not to withdraw funds for a fixed period in exchange for higher interest rates. Typically, these investments have short-term maturities, lasting from a month to several years, and require varying minimum deposits. The funds cannot be withdrawn until the term ends. ([Investopedia]('https://www.investopedia.com/terms/t/termdeposit.asp'))

> Term deposits increase banks' lending capabilities by enlarging the pool of funds from which they can lend to individuals and businesses, thus earning more interest revenue. This is particularly significant for small and emerging banks with limited resources. Analyzing the factors that influence customer enrollment in term deposits through marketing campaigns can empower banks to design more impactful strategies, resulting in increased enrollment and, consequently, a larger lending pool and higher revenues.


Dataset Size: 45,211 records, 17 columns.
Each row represents an event in which a customer was contacted as a part of the marketing campaign, and several rows may refer to the same customer, although there is no key for customer IDs. 


| Column No. | Attribute | Description |
|-----|-----------|-------------|
| 1   | age       | Numerical: the age of the client. |
| 2   | job       | Categorical: the type of job ("admin.", "unknown", "unemployed", "management", "housemaid", "entrepreneur", "student", "blue-collar", "self-employed", "retired", "technician", "services"). |
| 3   | marital   | Categorical: marital status ("married", "divorced", "single"; note: "divorced" includes widowed). |
| 4   | education | Categorical (Ordinal): level of education ("unknown", "secondary", "primary", "tertiary"). |
| 5   | default   | Binary: has credit in default? ("yes", "no"). |
| 6   | balance   | Numeric: average yearly balance in euros. |
| 7   | housing   | Binary: has housing loan? ("yes", "no"). |
| 8   | loan      | Binary: has personal loan? ("yes", "no"). |
| 9   | contact   | Categorical: contact communication type ("unknown", "telephone", "cellular"). |
| 10  | day       | Numeric: last contact day of the month. |
| 11  | month     | Categorical: last contact month of the year ("jan", "feb", "mar", ..., "nov", "dec"). |
| 12  | duration  | Numeric: last contact duration in seconds |
| 13  | campaign  | Numeric: number of contacts performed during this campaign and for this client (includes last contact) |
| 14  | pdays     | Numeric: number of days that passed by after the client was last contacted from a previous campaign (-1 means the client was not previously contacted). |
| 15  | previous  | Numeric: number of contacts performed before this campaign and for this client. |
| 16  | poutcome  | Categorical: outcome of the previous marketing campaign ("unknown", "other", "failure", "success"). |
| 17  | y         | Whether the client will subscribe to a term deposit ("yes", "no"). |




----------------------------------
### Initial Assumptions & Questions<a name="assumptions"></a>


Given what I know about term deposits and human behaviour, these are my initial assumptions about the features, without having looked at the data at all. These assumptions serve as a point at which I'll begin my analysis, to accept or refute:
- Positive correlations exhibited between: age, job, marital, education, balance, default, y (term loan success). 
- Negative correlations between: housing, loan, default, y (term loan success). The reason for this assumption is that, people with more loans are more in need to cash and less able to put it away for a period.
- negative correlation between personal/house loans and balance 

Questions:
- Holidays in Spain that might affect the final decision
- what types of loans are these "personal loans"?
- what are the values of personal and housing loans?
- incomes?
- number of loans in total?
- duration, interest rate, amount of the term deposit account being marketed 



----------------------------------
### Data Prep<a name="prep"></a>

- created dummy variables for categorical variables e.g. marital, house, default, ...
- created separate df for the dummies only, saved as xlsx 
- kept separate df without dummies, and only mapping from months (str) to numbers 1-2; saved as xlsx
- combined the two dfs along column axis so that I can have all the dummies as well as the original columns on one sheet; saved as xlsx 



----------------------------------
### Analysis<a name="analysis"></a>

- the analysis toolpak's descriptive statistics module swiftly calculates metrics like mean, SE, variance, etc... but this is based on the data distribution and not the sample distribution
- looking at the values of skewness and kurtosis, many columns are not normal, and since normality is preferred for calculating the confidence level, and is assumed for other statistical tests like the t-test, I need to calculate the sample means and SE with bootstrapping with replacement, which will be more "normal" than the original distribution due to the CLT 
- Distributions are considered highly skewed if their skewness is below -1 or above 1, moderately skewed if skewness falls between -1 and -0.5 or between 0.5 and 1, and approximately symmetric if skewness lies between -0.5 and 0.5.
- for my bootstrap with replacement, each sample will be the same as the size of the original set of data to ensure that the bootstrap samples are representative of the original data. 
- I'm going to use 1000 as my number of repeats (n)
-since there are so many samples, I will use VBA--> this threw and error when I tried to run the macros because of the sheer size of n and samples per n
- calculate trimmed mean for ea. column since it's more robust against outliers
- noticed that, for features with relatively "normal" skewness and kurtosis (or close to normal), the trimmed mean is closer to the original mean 




- instead, I'll do the bootstrap with python 

The kurtosis of a normal distribution is 3. Having a lower kurtosis (platykurtic) indicates that the distribution is less peaked and has lighter tails than a normal distribution, meaning it has fewer extreme values. Conversely, having a higher kurtosis (leptokurtic) indicates that the distribution is more peaked and has heavier tails than a normal distribution, suggesting more extreme values present in the data.







----------------------------------
### Dashboard<a name="features"></a>



--------------------
### Key Findings<a name="key"></a>



-------------------------
### Concluding Remarks<a name="conclusion"></a>

- being able to predict whether a client will subscribe to a term plan in the next contact during the campaign can help small banks optimize their resources to strategically decide when to contact, who to contact
- more term deposits means a larger pool of money the bank can loan out to generate revenues from interest, which means growth for the bank 

-----------------------
### Contact<a name="contact"></a>
Feel free to contact me if you have any suggestions or feedback. 

<a href="mailto:brigitte.xyan@gmail.com">
  <img src="https://cdn-icons-png.flaticon.com/512/281/281769.png" alt="Email Me" width="30" height="30">
</a>



--------------------------
### Acknowledgements & Sources<a name="ackowledgements"></a>

  [UC Irvine Machine Learning Library-- Bank Marketing]('https://archive.ics.uci.edu/dataset/222/bank+marketing')
  
Moro, S., Cortez, P., & Rita, P. (2014). [A data-driven approach to predict the success of bank telemarketing.]('https://www.semanticscholar.org/paper/A-data-driven-approach-to-predict-the-success-of-Moro-Cortez/cab86052882d126d43f72108c6cb41b295cc8a9e') Decis. Support Syst., 62, 22-31.

----------------------
### License<a name="license"></a>
This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).


-------------------------------







