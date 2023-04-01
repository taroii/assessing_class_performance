# College Student Performance Analysis
UCLA STATS 101A Winter 2023 Group Project  
By Taro Iyadomi, Minhao Han, Marlene Lin, Meha Mukherjee, Laura Ngo, Shiqin Tan, Emre Turan  

## I. Introduction  

  In this project, we aim to investigate the various campus climate factors that could potentially impact undergraduate students’ academic performance, measured by GPA (Grade Point Average out of 4), through regression analysis. Our research question of interest is: How can a student’s GPA be affected by various educational and socioeconomic factors?  

  The data used in this project is derived from a “UCLA Campus Climate Survey”inspired study, where responses from a sample of 940 UCLA undergraduates enrolled in lower division statistics classes were collected in 2015 (Esfandiari, 2018). The questionnaires inquired about the student’s perceptions of campus climates, including a variety of questions on their family background, demographics, perceptions of respect towards their identities, their GPA, etc. Initially, we suspected gender, relationship status, and ethnicity to have the biggest effect on GPA.  

  From the preliminary data exploratory analysis, we decided to try fitting the data with multiple linear regression (MLR). Intuitively, we expected to see gender, relationship status, and ethnicity to affect class performance the most. However, the results indicate that year of study, parental education level, whether the student was leaving UCLA, perceived sense of people staring at them, exclusion based on language proficiency, academic respect, being hispanic or latino, enrollment in a statistics course, and perceived empathy from the faculty were significant predictors of GPA. These variables were selected after data processing and performing both forward and backward stepwise regressions on the 60+ variables from the questionnaire. We also attempted variable transformation and adding interaction terms, but found that both induced high biases without significantly improving model predictions.  

  The findings of this project would have implications for universities and policymakers in developing effective interventions and policies to support students' academic success. This report consists of the following parts: Introduction, Data Description, Results & Interpretation, and Discussion.

## II. Data Description and Analysis  

<img src="https://user-images.githubusercontent.com/114524578/228988847-8d5ccd20-1065-4733-b31f-35ba7e6dea48.png" width=600 height=350 />  
Figure 1. Visualization of missing values in original dataset.  

<br/>

  Our original dataset contained 75 predictor variables and one response variable (GPA) with over 900 observations. On initial inspection, we found that seven of those predictors contained a high proportion of missing values (<20%) with over 200 missing values (Figure 1). Because of their high NA count, we deemed it unreasonable to impute those missing values and removed those seven predictors, leaving 68 predictor variables. Of those, 49 were numerical while the remaining 19 were categorical, with the numerical predictors ranging either from [0,1] or [1,5]. The response variable GPA had a range of [0.0, 4.0], but the majority of students had GPAs either around 3.0 or 4.0 (Figure 2). Due to the high dimensionality of the data and discrete nature of every variable besides GPA, analyzing their distributions, correlations, and summary statistics were difficult and impractical as the majority of those variables would not be in our final model (Figure 3). Instead, we focused our analysis on the three predictors we suspected to have the greatest impact on GPA.  
  
<br/>
  
<img src="https://user-images.githubusercontent.com/114524578/228988970-775a19a5-3da7-429d-9d27-22045412e210.png" width=600 height=350 />   
Figure 2. Distribution of GPA.  

<br/>

Scatterplot | Matrix    
:------------:|:----------------:
<img src="https://user-images.githubusercontent.com/114524578/228989084-bb7548d9-101d-41d6-a27b-fd98502f4ed6.png" width=500 height=300 /> | <img src="https://user-images.githubusercontent.com/114524578/228989096-e9239c17-b2c4-46ab-b9c9-dfc9292580d5.png" width=500 height=300 />  

Figure 3. Scatterplot matrices of stepwise selected variables. With over 50 variables, analyzing the scatterplot matrices of all variables was infeasible.  

<br/>

<img src="https://user-images.githubusercontent.com/114524578/229263274-3346e9ca-97ba-4d34-93d1-8c6e5adb5bdb.jpg" width=700 height=300 />  

Figure 4. Analysis of gender and its effect on GPA.  

<br/>

  In Figure 4, we saw that while the median GPAs for female, male, and queer identifying students were relatively similar, we found that transgender students had a much higher median GPA than the other students. That being said, there weren’t that many transgender students in our data, so its value as a predictor might not be strong. We also found that while male and female students had overlapping distributions, queer students showed greater proportions of lower GPA students, while transgender students tended to stay in the middle of 3.0 and 4.0 GPAs.  
  
<br/>

<img src="https://user-images.githubusercontent.com/114524578/229263318-9c3ef445-054d-4b93-b5ce-798a4e4a08b8.jpg" width=700 height=300 />  

Figure 5. Analysis of relationship status and its effect on GPA.  

<br/>

  In Figure 5, we saw that there was a slight increase in GPA among students that were single, with similar performances by married students and students in relationships. While single and students in relationships showed similar bimodal distributions, married students showed a more normal distribution. In this context, it meant that while a lot of married students have a GPAs around 3.0, there wasn’t a peak around 4.0, indicating that there was a dropoff of married students striving for perfect GPAs than non-married students.  
  
<br/>
  
<img src="https://user-images.githubusercontent.com/114524578/229263349-f7893653-e5fa-44c7-b4a4-4beeaca4989b.jpg" width=700 height=300 />  

Figure 6. Analysis of ethnicity and its effect on GPA. 

<br/>  

  In Figure 6, we saw that black and hispanic/latino students had an abnormally high peak around 3.0 GPA than the other ethnicities. This, supported by the side-by-side boxplot on the left, indicated that those groups of students had visually significant differences than the other groups. After analyzing Figures 4-6, this was the only significant difference between groups, as the other visualizations showed both a lot of overlap between categories as well as low student counts for the groups that did show any differences. This will become apparent in the feature selection segment of part IV.  

  In terms of preprocessing the data, we converted all of the categorical variables into numerical ones. Most of the categorical variables could be converted to dummy variables, and the rest ranges from [1,3] or [1,5]. For example, we converted the variable year from year names such as “freshman” or “senior” to a scale from 1-5, and highest parental education (meduc/feduc) from “High School diploma” and “PhD” to a scale from 1-4.  

## III. Model Building and Results  

| Model | # of Predictors | F-statistic | P-val of F-statistic | Adj-R2 | MSE (20% test set) | # of Significant Vars |
|-------|-----------------|-------------|----------------------|--------|--------------------|-----------------------|
| Suspected Variables | 3 | 10.99 | < 2.2e-16 | 0.149 | 0.283 | 0 |
| All Variables | 68 | 4.53 | < 2.2e-16 | 0.361 | 0.274 | 25 | 
| Significant Variables | 25 | 10.26 | < 2.2e-16 | 0.238 | 0.311 | 12 |
| Stepwise Selected | 10 | 35.8 | < 2.2e-16 | 0.319 | 0.238 | 9 |
| Transformed | 10 | 35.4 | < 2.2e-16 | 0.316 | 67.45 | 9 |
| With Interaction | 10 | 2.5 | < 2.2e-16 | 0.414 | 1059 | NA |  

Table 1. Model Summary Statistics  

<br/>  

  Before processing the data, we came up with two preliminary models to serve as baselines for any improved models we might consider later on. We first built a MLR model using the three variables we intuited would have an effect on GPA previously (gender, relationship status, and ethnicity). The model statistics are shown in Table 1, and they are consistent with the preliminary analyses from Figures 4-6 as the overlaps in variables implies that these variables are not the strongest predictors for GPA. The low adjusted R2 value shows that this model is very weak, and while there aren’t any bad leverage points, we can see from the Normal Q-Q plot that the distribution of error terms is not completely normal. We can also see that the residuals are not randomly distributed around zero in the residual plot due to some outliers with high GPA’s (Figure 7). So, the model assumptions are violated with this model.  
  
<br/>
  
<img src="https://user-images.githubusercontent.com/114524578/229264167-62e66f37-5a3f-492a-a740-f780751f11b6.png" width=500 height=300 />  

Figure 7. Diagnostic plots for the suspected variables model.  

<br/>

  Next, we created a MLR model using all of the 68 initial predictors without preprocessing. The results are shown in Appendix Table 1, and, unsurprisingly, we see a stronger correlation with an adjusted R2 value of 0.3612. Although this is still less than 0.5, this value is reasonable for a social dataset, and once again we see that the p-value for the F-statistic is low, indicating that there is significant evidence to support that at least one of the variables significantly affects GPA. Although this model is stronger than the previous one, it suffers from similar assumption violations as the previous model. Particularly, there are discernible patterns in the residual plot and scale-location plot. However, the Normal Q-Q plot shows an improvement in the normality of error terms (Figure 8).  
  
<br/>

<img src="https://user-images.githubusercontent.com/114524578/229264250-a2fe49fd-5615-4fe9-9ada-f028a3bed2a4.png" width=500 height=300 />  

Figure 8. Diagnostic plots for the all variables model.  

<br/>

  After processing the data, we created two improved models using differing feature selection approaches. The first approach consisted of identifying the significant variables from our all-variables model and basing our model off of that. The diagnostic plots of this model (Figure 9) follow closely to the plots of the all-variables model (Figure 8), but the strength of the model significantly drops from an adjusted R2 value of 0.3612 to 0.2376 (Table 1). While this is still better than our first model with three variables, the significant drop in adjusted R2 value indicates that there is a lot of room for improvement, which makes sense because simply choosing the most significant features from the all-variables model is a naive approach. This is because the significance of each variable depends on the presence of other variables, so a better approach would be to test each variable individually.  
  
<br/>
  
<img src="https://user-images.githubusercontent.com/114524578/229264305-2cd4c794-9a25-4864-b49a-3665d01b851d.png" width=500 height=300 />  

Figure 9. Diagnostic plots for significant variables model.  

<br/>  

  The next model used variables selected from both forward and backward stepwise regression. We used these two methods because they are more efficient than an exhaustive search, which is good for our large dataset. Additionally, since they operate in different directions, we lessen the bias the initial order of predictors has on the selection algorithms’ results. The vast majority of metrics we tested (Adjusted R2, Mallow’s Cp, and BIC) concluded that a subset of nine predictors was optimal, and both forward and backward selection agreed on the variables: year, feduc, leavingucla, staringatyou, excluenglish, appearanceresp, academicresp, hispanicLatino. The variable Stats13M was significant in the forward pass, and facunderstand was significant in the backward pass, so we selected both for our model leaving us with 10 predictors. The statistics are in Table 1, and while the adjusted R2 is less than the all-variables model, the lower mean squared error (MSE) indicates that this model performed better on a test set (on an 80-20 train-test split). This implies that this model is less biased than our all-variables model, but the diagnostic plots show the same underlying assumption violations as before (Figure 10). That being said, checking for multicollinearity, we found that none of the variance inflation factors (VIF) are above five. This indicates that none of the predictors for this model are dependent on each other, which is likely an issue with the all-variables model. Because of this, we chose this model as our primary model.   

<br/>

<img src="https://user-images.githubusercontent.com/114524578/229264437-a2c8eba3-aa57-44fe-8f60-95d31a6577c2.png" width=500 height=300 />  

Figure 10. Diagnostic plots for stepwise selected variables model.  

<br/>

  In order to improve our model, we considered implementing variable transformations and interactions between variables. For the former, we utilized the Box-Cox method for transforming both the predictors and the response (Table 1). This resulted in transformations for every variable in our chosen model, and while the resulting adjusted R2 was not too different from the untransformed model, the MSE was drastically higher than all of the models we’d created so far (Table 1). Because of this high level of bias, we discarded this change. As for implementing interaction terms, we created a model that considered the interactions between all of the predictors. This resulted in the highest adjusted R2 yet, however, it also resulted in the highest MSE yet. Similarly to the transformed model, this model suffers from high (if not extreme) bias. Therefore, we discarded this change as well.  
  
<br/>

<img src="https://user-images.githubusercontent.com/114524578/229264471-b52b5af3-86ae-4a82-97e8-b85aeea4f788.png" width=500 height=300 /> 

Figure 11. Mean Squared Error per Model (transformations and interactions not included for visibility). 

<br/>

  The model results are visualized in Figure 11. Here, we can see that the stepwise selected model performs the best in terms of MSE while having a relatively high adjusted R2. Note that the transformed model and the model with interaction terms are not present, because their MSE’s are magnitudes of order greater than the four shown here.  

## IV. Discussion  

  To summarize our findings, we found several correlative relationships between the variables we examined and a student’s GPA. The variable with the most impact at 0.07493 was referring to whether the student’s father was highly educated, and the variable with the least impact within the ten we selected was referring to whether students felt understood by faculty at 0.02323 which we considered to be not statistically different from 0. Consistent with our findings, a paper published in the European Journal of Public Health examining the role of parenting and child intelligence states, “Parental education is one of the best predictors of child school achievement [...] Parents with a high education are able to provide social and material resources promoting higher offspring school achievement.”  

  Looking more closely at each variable with an effect on GPA, we saw a positive impact on a student’s GPA if they had a father with high levels of education, if they felt responsible for their own academic success, and if they felt socially accepted despite only speaking English. On the other hand, we found several negative relationships between GPA and the remaining variables. The most significant relationships include whether students felt they were perceived negatively, and whether students felt responsible for negative reactions to their appearance, and what year a student was in. Moreover, the other variables with negative relationships to GPA with slightly lower coefficient values included whether the student had considered leaving UCLA due to negative experiences, whether the student was enrolled in Stats 13M, and whether the student was Hispanic/Latino. In terms of translating these findings into the real world, justifying these findings is fairly logical and straightforward. For instance, several variables that would have a negative impact on a student’s mental health are variables that drive GPA down, such as feeling negatively perceived and judged by peers or being a senior with a tougher course load versus a freshman just getting started. Similarly for variables that seemingly improve a student’s GPA, such as feeling socially accepted, the data transitions well into real world applications and has the opposite effect of rising GPA due to positive benefits on mental health.  

  Diving deeper into the information about mental health impacting GPA, data published about Consequences of Student Mental Health Issues by the Suicide Prevention Resource Center states that, “Mental health problems can affect a student’s energy level, concentration, dependability, mental ability, and optimism, hindering performance. Research suggests that depression is associated with lower grade point averages, and that co-occurring depression and anxiety can increase this association”. These findings are in line with the variables we found to be the most harmful to a student’s GPA, as the top four most impactful negatively correlated variables are directly associated with mental health struggles.  

  Some challenges we encountered while working on the project include fixing and changing how correlated certain variables are depending on their distribution in relation to one another. We struggled with finding relevant transformations, we as changed variables numerous times given our original hypothesis was incorrect or not statistically significant enough. Moreover, we encountered difficulty in determining what types of variables would be best to analyze, whether they be categorical or numerical. All of our variables, including the numeric ones, were either categorical or discrete numeric variables, making it much harder to interpret visualizations of the data. In addition, we tried to conduct Box Cox transformations on each of the variables and created a new model with those updated variables, but it ended up including a high level of bias and we decided to avoid using it. In the future, alternative transformation methods beyond the scope of this class may be useful in interpreting the data in a more effective way, and perhaps including alternative factors such as sleep quality, prevalence of anxiety medication, and stress levels in the dataset to take a closer look at the specificities of mental health.  

## V. References  

Esfandiari, M. (2018). STATISTICS: A WINDOW TO UNDERSTANDING DIVERSITY.
https://iase-web.org/icots/10/proceedings/pdfs/ICOTS10_C117.pdf  

Tamayo Martinez, Nathalie et al. "Double Advantage Of Parental Education For Child Educational
Achievement: The Role Of Parenting And Child Intelligence". European Journal Of Public
Health, vol 32, no. 5, 2022, pp. 690-695. Oxford University Press (OUP),
doi:10.1093/eurpub/ckac044. Accessed 14 Mar 2023.  

"Consequences Of Student Mental Health Issues – Suicide Prevention Resource Center". Sprc.Org, 2023,
https://sprc.org/settings/colleges-and-universities/consequences-of-student-mental-health-issues/#
:~:text=Mental%20health%20problems%20can%20affect%20a%20student's%20energy%20level
%2C%20concentration,%2C%20and%20optimism%2C%20hindering%20performance.&text=Re
search%20suggests%20that%20depression%20is,anxiety%20can%20increase%20this%20associa
tion. Accessed 14 Mar 2023.
