# PawsAnalytics
This study aimed to investigate the factors that influence pet adoption and length of stay in shelters/foster homes in the United States, focusing on dogs and cats listed on
PetFinder. The study also aimed to determine the impact of certain traits and location-based factors on the number of adopted pets. We used three datasets and examined three regions to investigate four research questions. Statistical techniques, including z-tests, chi-square tests, t-tests, ANOVA tests, survival analysis, and linear regression models, were used to compare means and distributions of various variables. We ultimately found that animal traits such as size and gender significantly impacted the length of stay in shelters and time to adoption, while location-based factors such as population density and median household income were associated with the number of adopted pets in a region. Adopted dogs were found to have a higher life expectancy than the general population, although there was no relationship between the life expectancy of dogs in different regions, and the statistical tests rejected the null hypothesis for all pairs of categorical variables except for region vs. gender. Our linear regression model showed that location-based factors such as median household income and population density affected the number of adopted pets. We conclude that animal traits and location-based factors are important in the adoption of pets and that policy makers and animal shelters should consider these factors when developing adoption strategies.

## Introduction
According to the American Society for the Prevention of Cruelty to Animals (ASPCA), approximately 6.5 million companion animals enter animal shelters in the United States each year. Out of those, around 3.3 million are dogs and 3.2 million are cats. Unfortunately, not all these animals find homes. The ASPCA estimates that approximately 1.5 million shelter animals are euthanized each year. This number has decreased in recent years due to increased efforts to promote adoption and spaying/neutering, but it is still an alarming statistic. In addition to euthanasia, dogs and cats in shelters may experience other challenges, such as overcrowding, limited resources, and stress. Furthermore, the shelter environment can be stressful for animals, which can negatively impact their health and behavior. Understanding the factors that influence pet adoption and reduce shelter stay is crucial to improving the lives of dogs and cats in shelters. By identifying traits that make pets more adoptable and analyzing location-based factors, animal welfare organizations can work to increase the number of successful adoptions and reduce the time that pets spend in shelters. The goal of this project is to explore the factors that influence pet adoption and reduce the time that animals spend in shelters. By analyzing the data obtained from the Petfinder API, we can identify traits that make pets more adoptable and location-based factors that impact the number of adoptable pets in different states. The findings from this study can inform animal welfare organizations and help them optimize their adoption processes to increase the number of successful adoptions and reduce the length of stay for pets in shelters.

Our hypotheses are:
- Of cats and dogs listed on PetFinder who were ultimately adopted, more dogs than cats were adopted in the US. Additionally, more dogs were adopted in the Pacific Northwest (PNW) region compared to the South Central (SC) region of the United States.
- The mean days spent by dogs on Petfinder before adoption are equivalent among gender and size groups, and can be tested by comparing the mean days of each group.
- The mean minimum life expectancy of dogs adopted in different regions of the United States is the same.
- The median income, population, and age of the state is correlated with the number of adopted pets.

## Methods
### Data
In this section, we will describe the data that we collected to answer the research questions for this project. We found 4 data sources in total, and for each source, we will describe the collection method and potential limitations and biases within each dataset. For a more detailed explanation of the specific variables from each dataset used in this project, please reference the appendix.

### Petfinder API (PetFinder API calls)
Petfinder API is the largest dataset used in this project, containing up-to-date data on adoptable pets from over 10,000 animal welfare organizations. It includes all adoptable dogs and cats worldwide, with a static sample collected on February 23rd, 2023, filtered for dogs and cats who were ultimately adopted in the US. We’ve used a python wrapper called Petpy to retrieve data into pandas DataFrames for analysis. (Aschleg, 2021)

There are some potential biases and limitations to using data from the PetFinder API. First, the data only includes pets that are registered on the Petfinder platform, which may not represent the entire population of dogs and cats available for adoption in the United States. Additionally, the data is self-reported by animal welfare organizations, and there may be errors or inconsistencies in the data due to differences in reporting methods across organizations. Finally, the dataset may not capture all relevant factors that influence pet adoption and shelter stay, and there may be unobserved factors that impact these outcomes. 

### AKC Data (reference) (tmfilho, 2020)
This dataset contains information for around 277 dog breeds and was extracted from the website of the American Kennel Club. (American kennel club) The dataset has features including height, weight, life expectancy, shedding value, energy level, trainability, and demeanor value. The limitation of the dataset is that it only has data for dogs but not cats.

### Census Data: Income
To ensure the authenticity of the data, we collected the following two datasets from the US Census Bureau: Income and Population. The income dataset contains the median and mean income estimate for the year of 2021 for 4 types of living units: households, families, married-couple families, and nonfamily households, for all states in the US. According to the American Community Survey (ACS) methodology webpage, the sample size of the housing units completing final interviews is 1,950,832 for the US. (Bureau, Sample size 2022) The coverage rate is 98.5%. (Bureau, Coverage rates 2022)

There are several limitations to this dataset. The first issue is that since the population is big, this dataset is not the raw number of income of each household for every state. Instead, it is a calculated estimate value done by the ACS that exists a certain level of marginal error for every number. The second issue is, since all the money income is pretax, the actual income for every household or individual can be different from what it shows in the dataset, as different state has its own income tax policy.

### Census Data: Population
The source of the population dataset is the DP05: ACS Demographic and Housing Estimates (2021) from the US Census Bureau. This dataset contains the total estimated population for the country and for each state. The sample size and coverage rate are the same as the income data due to the same source of data. 

### Statistical Methods
#### Regions

- Pacific Northwest (PNW): Oregon, Washington, Idaho
- South Central (SC): Kentucky, Tennessee, Alabama, Mississippi, Arkansas, Louisiana, Oklahoma, and Texas
- Northeast (NE): Connecticut, Maine, Massachusetts, New Hampshire, New Jersey, New York, Pennsylvania, Rhode Island, Vermont

### Research Question 1
Are more dogs or cats adopted in the US? Are more dogs adopted within the Pacific Northwest or South Central regions of the United States?

#### One Sample Z-Test for a Proportion.
#### Data.
- Sample size: 372, 647 (203, 880 Dogs and 168, 767 Cats)
- Variables used from PetFinder API data: ‘id’, ‘type’, ‘contact.address.state’, ‘published_at’
- published_at Time Range: 2022-01-01 to 2022-12-31
- Filters: Location: Entire US, Status: Adopted, Type: Dogs and Cats

Descriptive Statistics. Create a boxplot showing the range of values of adopted dogs and cats by state. Each data point in the box plot represents the count of adopted dogs or cats for a specific state within the entire published_at time range. Also, create a heatmap showing the proportion of dogs by state.

Inferential Statistics. Conduct one sample z-test for comparing a proportion to the fixed value of 0.5. The population of interest is Dogs and Cats listed on PetFinder who were ultimately adopted. Our sample is a simple random sample from the population of interest. Need to verify the sample size is sufficiently large to meet the assumption of large sample size. The one sample z-test for comparing a proportion is appropriate for a simple comparison of adopted dogs and cats from the population of interest, without taking any additional factors into consideration. Using the fixed value of 0.5 is relevant since we are only considering dogs and cats as the options for adopted animal types, we would expect them to have equal proportions. 

#### Two Sample Z-Test for comparing two proportions.
#### Data.
- Sample size (PNW): 11, 101 (7, 006 dogs and 4, 095 cats)
- Sample size (SC):  40, 543 (26, 382 dogs and 14, 161 cats)
- Variables used from PetFinderAPI data, and published_at time range same as One Sample Z-Test for a proportion
- Filters: Location: PNW and SC regions, Status: Adopted, Type: Dogs and Cats

Descriptive Statistics. Create a side by side boxplot showing the range in values of proportions of adopted dogs and cats within each region. Each data point in the box plot represents the proportion of adopted dogs or cats for a specific state within the region within the entire published_at time range.

Inferential Statistics. Conduct two sample z-test for comparing the proportion of dogs in the PNW region to the proportion of dogs in the SC region. The populations of interest are Dogs and Cats listed on PetFinder who were ultimately adopted in the PNW and SC regions of the US. Our sample is a simple random sample from the populations of interest. Need to verify the sample size is sufficiently large to meet the assumption of large sample size. The two sample z-test for comparing two proportions is appropriate for a simple comparison of the proportion of adopted dogs in the PNW and SC regions of the US, without taking any additional factors into consideration. Using a proportion instead of comparing average number of pets adopted per month is more appropriate for this comparison because there are more states in the SC region than the PNW region.

#### Welch Two Sample t-test for comparing two means.
#### Data.
- Sample size for TX and WA: 24 (24 months of counts of adopted dogs in each state)
- Variables used from PetFinderAPI data, and published_at time range same as One Sample Z-Test for a proportion
- published_at Time Range: 2021-01-01 to 2022-12-31
- Filters:
    - Location: TX, WA
    - Status: Adopted
    - Type: Dogs

#### Descriptive Statistics. 
Create a side by side boxplot showing the range in values of counts of adopted dogs per month in TX and WA. Each data point in the box plot represents the count of adopted dogs for a specific month within the state.

#### Inferential Statistics. 
Conduct Welch Two-Sample t-test for comparing two means to compare the average number of adopted dogs per month in WA to the average number of adopted dogs per month in TX. The populations of interest are Dogs listed on PetFinder who were ultimately adopted in WA and TX. Our sample is a simple random sample from the populations of interest. Need to verify the sample size is sufficiently large to meet the assumption of large sample size. Using the welch two sample t-test to relax the requirement of equal variance. The Welch test is the most appropriate test to determine if there are typically more dogs adopted in WA or TX, without accounting for any additional, more complex factors. Comparing average number of adopted dogs per month over a 2 year period should help with minimizing the effect of any strong outliers.	

### Research Question 2
Which characteristics speed up adoption and reduce shelter stay (time a pet is listed on petfinder)?
#### Data.
Sample Size: 
- 163,976 (adopted dogs and cats in the Pacific Northwest, South Central and the     Northeast)
- Variables used:
'type', 'species', 'age', 'gender', 'size', 'coat', 'breed':(mixed,unknown,particular), 'colors.primary', 'colors.secondary', 'attributes.spayed_neutered', 'attributes.house_trained', attributes.declawed', 'attributes.special_needs', 'attributes.shots_current', 'environment.children', 'environment.dogs’,'environment.cats','days_in_shelter'
- To calculate the number of days that an animal was in the shelter (‘days_in_shelter’ variable), the published_at column was subtracted from the status_changed_at date to get a timedelta object representing the amount of time that the animal has been listed.

#### Descriptive Statistics.
- Computed Mean, median, and standard deviation of the days spent on Petfinder before adoption for cats and dogs.
- Created scatterplots of the days spent on Petfinder by adoption and size (cats and dogs)

#### Inferential Statistics. 
#### Welch Two sample t-test on days in the shelter by type of animal.			
Statistical Method. For the Welch Two sample t-test on days in the shelter by type of animal, the statistical method used is the Welch t-test, which is a variation of the t-test that does not assume equal variances between the two groups being compared.

Assumptions. The assumptions of the test are that the two groups (cats and dogs) are independent, and the population distributions of the two groups are approximately normal. 

Rationale for appropriateness of the method. The Welch two-sample t-test is suitable for comparing the mean number of days animals spend in the shelter by type because it is designed for comparing groups with unequal variances, assumes independent samples, and assumes normality. It is also a hypothesis test that allows us to determine whether the difference between the means of the two groups is statistically significant.

Statistical hypothesis being tested. The statistical hypothesis being tested in the Welch two-sample t-test on days in the shelter by type of animal is whether there is a statistically significant difference between the mean number of days spent in the shelter for two independent groups of animals (e.g., cats vs. dogs). The test is used to determine whether any observed difference between the means is likely due to chance or whether it is a true difference that is statistically significant.

ANOVA Test to compare the mean days spent on Petfinder before adoption between male and female dogs and small and large dogs.

Statistical Method. For the test to compare the mean days spent on Petfinder before adoption between male and female dogs, an ANOVA test is used. The test determines whether there are any statistically significant differences between the means of two or more independent groups. 

Assumptions. The assumptions of the ANOVA test are that the data is normally distributed, the variances of the groups being compared are equal, and the groups are independent.

Rationale for the appropriateness of the method. The ANOVA test is an appropriate statistical method for comparing the mean days spent on Petfinder before adoption between different groups of dogs because it can handle multiple groups and determine if there is a significant difference between them. ANOVA assumes independence between groups and normality within each group. It also allows for the examination of interactions between groups.

Statistical hypothesis being tested. The statistical hypothesis being tested is whether there is a statistically significant difference in the mean days spent on Petfinder before adoption between male and female dogs and small and large dogs. Specifically, the null hypothesis is that there is no difference between the groups, while the alternative hypothesis is that there is a difference. The ANOVA test allows us to test this hypothesis by comparing the variability within each group to the variability between the groups, using an F-test.

Survival analysis to predict adoption probability over time and identify factors impacting adoption duration (e.g., age, size, Petfinder duration).	

Statistical Method(s). For the Survival analysis to predict adoption probability over time and identify factors impacting adoption duration (e.g., age, size, Petfinder duration), several statistical tests are used. First, a Likelihood ratio test, Wald test, and Score (logrank) test are used to determine if there is any relationship between age, size, or days spent on Petfinder before adoption and the time to adoption for pets. If there is evidence of a relationship, further analysis is done using a Cox proportional hazards model to examine the coefficients of the variables and identify which variables have a significant relationship with adoption. Finally, regression is used to identify which variables are statistically significant predictors of adoption. 

Assumptions. The assumptions for these tests are that the data is independent, the events are non-informative, and the hazard rates are proportional over time.

Rationale for the appropriateness of the method. The use of survival analysis and regression to predict adoption probability over time and identify factors impacting adoption duration is appropriate for several reasons. Firstly, survival analysis is specifically designed for modeling time-to-event data, which is applicable in this case as we are interested in predicting the time until adoption. Regression analysis is useful for identifying the factors that are associated with the outcome variable (adoption duration), allowing us to determine which factors may be important predictors of adoption.

Furthermore, survival analysis and regression allow us to account for the fact that some animals may still be in the shelter at the end of the study period, and the outcome variable (adoption duration) may be right-censored. This means that we may not know the exact duration of time an animal spent in the shelter, but only that they were still in the shelter at the end of the study period. Survival analysis can handle censored data and estimate the probability of adoption over time, while regression can account for the effects of censoring on the estimated coefficients.

Overall, the use of survival analysis and regression to predict adoption probability over time and identify factors impacting adoption duration is appropriate and allows us to gain insights into the factors that may influence adoption and how they impact the duration of time an animal spends in the shelter.

Statistical hypothesis being tested. The statistical hypothesis being tested for survival analysis and regression to predict adoption probability over time and identify factors impacting adoption duration would be whether the chosen predictor variables (age, size, Petfinder duration) have a statistically significant effect on the hazard rate or survival probability of adoption, after controlling for other variables in the model.

### Research Question 3
Do adopted dogs have the same life expectancy as compared to the population ?
#### One Sample Z-Test for comparing life expectancy of adopted dogs to the population of all dogs of US
#### Data
- Sample size: 61,610 Adopted Dogs
- Variables used from PetFinder API data: ‘id’, ‘type’, ‘breeds.primary’
- Variables used from AKC data: ‘max_expectancy’,  ‘max_expectancy’, ‘Breed’
- Time Range: 2022-12-14 to 2022-02-23
- Filters:
    - Location: Entire US
    - Status: Adopted
    - Type: Dogs

Descriptive Statistics. Calculate Mean, median, mode, standard deviation, variance, and range of the minimum life expectancy and max life expectancy variables. 
Plotted histogram of min_expectancy and max_expectancy

Assumptions. Our sample is a simple random sample from the population of interest. We verified the sample size is sufficiently large to meet the assumption of a large sample size. 

Rationale for appropriateness of the method. The one-sample Z-test is an appropriate statistical test to compare the minimum and maximum life expectancy of adopted dogs with the overall US dog life expectancy values of 10-13 years.This is because we know  that the sample is large enough to have normal distribution from central limit theorem and we also know the population parameters. We can assume that the life expectancy of adopted dogs is independent of the overall US dog life expectancy values.

Inferential Statistics. It has been computed that dogs' life expectancy in the US is 10-3 years. Conduct one sample z-test for comparing a min_expectancy of adopted dogs to the fixed value of 10( population min_expectancy). The population of interest is Dogs and on PetFinder who were adopted. The one-sample z-test for comparing a proportion is appropriate for a simple comparison of adopted dogs from the population of interest, without taking any additional factors into consideration. Using the fixed value of 10 is relevant since we that is the population min_expectancy in the US.
Similarly, we also conduct one sample Z  test for max_expectancy comparing it with 13 years which is the population max_expectancy in the US.

One-way ANOVA test to find if there is relationship between min_expectancy of dogs of various regions
#### Data
- Sample size: 61,610 Adopted Dogs ( from all 3 regions ) 
- Variables used from PetFinder API data: ‘id’, ‘type’, ‘breeds.primary’, ‘contact.address.state’
- Variables used from AKC data: ‘max_expectancy’, ‘Breed’
- Time Range: 2022-12-14 to 2022-02-23
- Filters:
    - Location: Entire US
    - Status: Adopted
    - Type: Dogs

Descriptive Statistics. Computed 95% confidence intervals of min_expectancy of adopted dogs for all three regions (Pacific Northwest, South Central, Northeast ) 

Assumptions. The assumptions of the ANOVA test are that the data is normally distributed, the variances of the groups being compared are equal, and the groups are independent. All the assumptions hold true for conducting the tests for adopted dogs between different regions. 

Rationale for the appropriateness of the method. The one way ANOVA test is an appropriate statistical method for comparing the min_expectancy between dogs of different regions because it can handle multiple groups and determine if there is a significant difference between them. ANOVA assumes independence between groups and normality within each group. It also allows for the examination of interactions between groups.

Inferential Statistics. For the test to compare the min_expectancy of dogs in the three regions, an ANOVA test is used. The test determines whether there are any statistically significant differences between the means of min_expectancy in the three independent groups of dogs of regions. 

### Research Question 4
#### Chi-squared Test of Independence
#### Data.
- Sample size: 163,976
- From PetFinder API data: age, gender, size, species, coat, contact.address.state, region
    - Regions are outlined in the Data subsection of Research Question 1

Descriptive Statistics. Provide contingency tables with the percentage that lists the number of pets in different regions with their category in features.

Inferential Statistics. Perform chi-square test to test the association between the following sets of  2 categorical variables: region vs pet age, region vs pet gender, region vs pet size, region vs species, region vs coat.

Assumptions. The assumptions to perform chi-squared tests for categorical variables are the sample size is large, and the variables are mutually exclusive.

#### Simple Linear Regression for Number of Adopted Pets
#### Data.
- Sample size: 163,976
- From PetFinder API data: age, gender, size, species, coat, contact.address.state, region
- From income data: median_income, state
- From population data: total_population, median_age, state

Descriptive Statistics. Calculate min and max values for the following variables from the census income and population datasets: median_income, total_population, median_age. Provide a side-by-side boxplot showing median_income for each household type, and a plot for the relationship between total_population and median_age.

Inferential Statistics. Fit linear regression model with the data from PetFinder API and Census Bureau, including age, gender, size, species, coat, contact.address.state, region, population, median income, and median_age. Test the significance of the variables in the regression model and test the significance of the overall model. Examine the model for the assumptions using residual plots.

Assumptions. The assumptions for simple linear regression are independent observations, linear relationships between the mean response and predictors, normally distributed response variables, and independent and constant variance.


