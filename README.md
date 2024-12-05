# STAT184-HW-Template
# Introduction 
In this activity, I analyzed the Berkley Dataset. The history behind this dataset is stated by the creater as the following:

> The year is 1973. UC Berkeley has been sued for sex-based admissions discrimination. Specifically, the plaintiff claims that women are discriminated against in admissions policies. The court has provided admissions data to you and tasked you with answering the question — if a student applies to the school, will their gender play a significant role in their chance of admission?

**Our goal in this project was to determine if gender plays a significant role in a students chance of admission at UC Berkeley in 1973.** Therefore, the purpose of this repository is to assist in looking at various relationships amongst the variables in this datatset-- ultimately answer this research question.

In order to coprehensively answer this question, we went through **six specfic tasks**: Data inventory, Exploratory Data Analysis (EDA), Admission Rates by Gender, Applications Received Grouped by Gender and Major, Admission Rates Grouped by Gender and Major, Gender Difference in Admission Rates by Major. 

In the Berkely dataset, individual student are cases, and their attributes are Year, Major, Gender, and Admission. 

# Implementation 
1. Data Inventories: I worked to load the admissions data set and the libraries needed for our data visualization. The libabries that I included was ggplot2, dplyr, tidyr, and knitr.
```
 # Load the admissions dataset and additional packages 
  url <- "https://waf.cs.illinois.edu/discovery/berkeley.csv"
  data <- read.csv(url)
```

2. Exploratory Data Analysis (EDA): I used the head() function for the first six cases. Next, I used the summary() function to display details such as the year or how many students are represented in this study, which in this case is 12763.
```
# EDA
head(data)
summary(data)
```

3. Admission Rates by Gender: In this step, I looked at the admission rates by gender. This caused more problems then expected because there was no percentage that stored the admission rate based on gender. In order to do this, I realized I needed to created a table using the group_by(Gender) function and summarized the total number of accepted versus rejected students. Then, I could calculate admission rate by taking accepted/total number of students per gender. Finally, I created a bar graph of this relationship.
```
# Calculate the admission rates by gender 
table(data$Gender, data$Admission)

admission_rates_gender <- data %>%
  group_by(Gender) %>%
  summarize(
    Total = n(),
    Accepted = sum(Admission == "Accepted"),
    Admission_Rate = Accepted / Total
  )
```

4.  Applications Received Grouped by Gender and Major: I found the gender distribution within each major. I used select(Major, Gender) function to choose two specific rows from the data set and group_by(Major, Gender). With this, I created a graph that plots the Major on the x-axis, applicants on the y-axis, and gender through the colors (Reddish pink represents female and blue represents male)
```
#Applications by Gender, Major
applications_gender_major <- data %>%
  select(Major, Gender) %>%
  group_by(Major, Gender) %>%
  summarize(
    Total = n()
  )
```
   
5.  Admission Rates Grouped by Gender and Major: The next key is plotting  admissions rates by gender and major. To get the admission rates we use the previous formula:
```
Accepted = sum(Admission == "Accepted"),
Admission_Rate = Accepted / Total)
```
In this case, I used the group_by(Major, Gender) and summarize() function to to get the total and calculate the admission rates. Finally, I created a bar graph that plots the major on the x-axis, admission rates on the y-axis, and assigns the color of the bar based on the gender.
```
#See admission rates broken down by gender and major
admission_rates_gender_major <- data %>%
  group_by(Major, Gender) %>%
  summarize(
    Total = n(),
    Accepted = sum(Admission == "Accepted"),
    Admission_Rate = Accepted / Total
  )
head(admission_rates_gender_major)
```

6. Gender Difference in Admission Rates by Major: Now, to answer the main question posed at the beginning of this activity. Does gender play a significant role in a students chances of getting into the college in each major?

I struggled in the logic behind this step. I knew we needed to select(Major, Gender, Admission_Rate); however, how was I going to look at the difference. First, I tried usinf color in the graph to represent different genders, but I still needed to look at easily comparing the genders together. In solve this issue, I had to mutated the original table to take into account the difference between the Male and Female admission rates. Therefore, we need two functions pivot_wider() and mutate(Difference = M-F) to solve this issue. This will be how thefollowing code looks.
```
#See the admissions rate gender difference by major
admission_rate_diff <- admission_rates_gender_major %>%
  select(Major, Gender, Admission_Rate) %>%
  pivot_wider(names_from = Gender, values_from = Admission_Rate) %>%
  mutate(Difference = M - F)
```

In the end, the final graph plots majors on the x-axis, the difference between male and female on the y-axis, and gender and whose admission rates are higher by the colors represented. 

# Does gender plays a significant role in a students chance of admission at UC Berkeley in 1973?
**Yes!** *Depending on your major*
<img width="944" alt="image" src="https://github.com/user-attachments/assets/bbf32fd7-3c6e-450e-88af-1ea79d38180e">

When looking at the resulting plot, we see that female's have a higher admission rate in major A,B,D, and F (A being the highest with around 10% difference in admission rates). In contrast, males have a higher admission rate in major C, E, and other.

Therefore, we can conclude that gender does plays a significant role in a students chance of admission at UC Berkeley in 1973 depending on what major they are choosing to go into.
