For this assingment I had to do is present the data by type, budget,size and average scores and percentage.
I had to use some fuctions to get the data above. iloc, sort,groupby, and mapping. 

# Dependencies and Setup
import pandas as pd
import numpy as np


# File to Load (Remember to Change These)
```
school_data_to_load = "Resources/schools_complete.csv"
student_data_to_load = "Resources/students_complete.csv"
```

# Read School and Student Data File and store into Pandas Data Frames
```
school_data = pd.read_csv(school_data_to_load)
student_data = pd.read_csv(student_data_to_load)
```
# Combine the data into a single dataset
```
school_data_complete = pd.merge(student_data, school_data, how="left", on=["school_name", "school_name"])
```

# In[2]:
```

#cleaning the datad 

clean_school_data_df = school_data.dropna(how="any")
#clean_school_data_df["budget"] = clean_school_data_df["budget"].map("${:,.2f}".format)
clean_school_data_df.head(50)

```
# In[3]:


```
#clean_school_data_df["budget"] = clean_school_data_df[["budget"]].astype(float)

clean_school_data_df.dtypes

```
# In[4]:

```
#total number of school in the data frame
total_schools = clean_school_data_df["school_name"].value_counts().sum()
total_schools

```
# In[5]:

```
# total number of student 
total_student = clean_school_data_df["size"].sum()
total_student
```

# In[6]:

```
# total budget 
total_budget = clean_school_data_df["budget"].sum()
total_budget
                                                   
```

# In[7]:

```
clean_student_data = student_data.dropna(how="any")
clean_student_data

```
# In[8]:

```
# average math score  
math_score = clean_student_data["math_score"].mean()
math_score

```
# In[9]:

```
# average math score  
reading_score = clean_student_data["reading_score"].mean()
reading_score

```
# In[10]:

```
#percent math rate 


#total_number_mpass = clean_student_data.loc[clean_student_data["Student ID"]]
math_70_df = clean_student_data.loc[clean_student_data["math_score"] >= 70]
read_70_df = clean_student_data.loc[clean_student_data["reading_score"] >= 70]
mandr_70_df = math_70_df.loc[math_70_df["reading_score"] >= 70]
#math_70 = math_70_df.loc[math_70_df["Student ID"]]
                                   
math_rate = math_70_df["math_score"].count() / total_student *100
reading_rate = read_70_df["math_score"].count() / total_student *100

```

# In[11]:
```

#overall passing rate
overall_df = (math_rate + reading_rate)/2
```

# In[12]:

```
total_df = pd.DataFrame(
    {
        'Total School':total_schools,
        'Total Students': total_student,
        'Total Budget': total_budget,
        'Average Math Score': math_score,
        'Average Reading Score': reading_score,
        'Passing Math': math_rate,
        'Passing Reading': reading_rate,
        'Overall Passing Rate': overall_df
        
    },index=[0]

)

```
# In[13]:

```
total_df.dtypes

``````
# In[14]:

```
total_df["Total Budget"] = total_df["Total Budget"].map("${:,}".format)
total_df["Average Math Score"] = total_df["Average Math Score"].map("{:.2f}%".format)
total_df["Average Reading Score"] = total_df["Average Reading Score"].map("{:.2f}%".format)
total_df["Passing Math"] = total_df["Passing Math"].map("{:.2f}%".format)
total_df["Passing Reading"] = total_df["Passing Reading"].map("{:.2f}%".format)
total_df["Overall Passing Rate"] = total_df["Overall Passing Rate"].map("{:.2f}%".format)


total_df
```

# ## District Summary
# 
# * Calculate the total number of schools
# 
# * Calculate the total number of students
# 
# * Calculate the total budget
# 
# * Calculate the average math score 
# 
# * Calculate the average reading score
# 
# * Calculate the overall passing rate (overall average score), i.e. (avg. math score + avg. reading score)/2
# 
# * Calculate the percentage of students with a passing math score (70 or greater)
# 
# * Calculate the percentage of students with a passing reading score (70 or greater)
# 
# * Create a dataframe to hold the above results
# 
# * Optional: give the displayed data cleaner formatting

# In[15]:
```

clean_school_data_df.sort_values(["type"], axis=0 ,ascending= True,inplace=True) 
clean_school_data_df
```

# In[16]:
```

clean_school_district_data_df= clean_school_data_df.loc[clean_school_data_df["type"] == "District", :]
clean_school_district_data_df.head()
```

# In[17]:
```

total_schools = clean_school_district_data_df["school_name"].value_counts().sum()
total_schools
```

# In[18]:

```
total_student = clean_school_district_data_df["size"].sum()
total_student

```
# In[19]:

```
total_budget = clean_school_district_data_df["budget"].sum()
total_budget
```

# In[20]:

```
merge_table = pd.merge(clean_school_district_data_df,clean_student_data,on ="school_name",how ='left')
merge_table
```

# In[21]:

```
#average math score
math_score = merge_table["math_score"].mean()
math_score
```

# In[22]:
```

# average reading score  
reading_score = merge_table["reading_score"].mean()
reading_score
```

# In[23]:

```
math_70_df = merge_table.loc[clean_student_data["math_score"] >= 70]
read_70_df = merge_table.loc[clean_student_data["reading_score"] >= 70]
mandr_70_df = math_70_df.loc[math_70_df["reading_score"] >= 70]
#math_70 = math_70_df.loc[math_70_df["Student ID"]]
                                   
math_rate = math_70_df["math_score"].count() / total_student *100
reading_rate = read_70_df["math_score"].count() / total_student *100
```

# In[24]:

```
overall_df = (math_rate + reading_rate)/2

```
# In[25]:

```
total_district_df = pd.DataFrame(
    {
        'Total District School':total_schools,
        'Total District Students': total_student,
        'Total District Budget': total_budget,
        'Average District Math Score': math_score,
        'Average District Reading Score': reading_score,
        'Passing Math (District)': math_rate,
        'Passing Reading(District)': reading_rate,
        'Overall District Passing Rate': overall_df
        
    },index=[0]

)

```
# In[26]:

```
total_district_df["Total District Budget"] = total_district_df["Total District Budget"].map("${:,}".format)


total_district_df

```
# ## School Summary

# In[27]:

```
#group by schools
by_school = school_data_complete.set_index('school_name').groupby(['school_name'])
#by types
school_types =clean_school_data_df.set_index('school_name')['type']
```
# student per each school
```
student_per_school = by_school['Student ID'].count()
```
# school budget
```
school_budget = clean_school_data_df.set_index('school_name')['budget']

#per student budget
student_budget = clean_school_data_df.set_index('school_name')['budget']/clean_school_data_df.set_index('school_name')['size']

#avg scores by school
avgerage_math = by_school['math_score'].mean()
avgerage_read = by_school['reading_score'].mean()
```
# % passing scores
```
math_70_type_df = (school_data_complete[school_data_complete["math_score"] >= 70].groupby('school_name')['Student ID'].count()/student_per_school)*100
read_70_type_df =(school_data_complete[school_data_complete["reading_score"] >= 70].groupby('school_name')['Student ID'].count()/student_per_school)*100
overall = (math_70_type_df + read_70_type_df )/2

school_summary = pd.DataFrame({
    
    "School Type": school_types,
    "Total Students": student_per_school,
    "Per Student Budget": student_budget,
    "Total School Budget": school_budget,
    "Average Math Score": avgerage_math,
    "Average Reading Score": avgerage_read,
    '% Passing Math': math_70_type_df ,
    '% Passing Reading': read_70_type_df,
    "Overall Passing Rate": overall
})

school_summary

```
# * Create an overview table that summarizes key metrics about each school, including:
#   * School Name
#   * School Type
#   * Total Students
#   * Total School Budget
#   * Per Student Budget
#   * Average Math Score
#   * Average Reading Score
#   * % Passing Math
#   * % Passing Reading
#   * Overall Passing Rate (Average of the above two)
#   
# * Create a dataframe to hold the above results

# ## Top Performing Schools (By Passing Rate)

# * Sort and display the top five schools in overall passing rate

# In[28]:

```
top_schools = school_summary.sort_values(["Overall Passing Rate"], ascending=False)
top_schools.head(5)
```

# ## Bottom Performing Schools (By Passing Rate)

# * Sort and display the five worst-performing schools

# In[29]:

```
bottom_schools = school_summary.sort_values(["Overall Passing Rate"], ascending=True)
bottom_schools.head(5)
```

# ## Math Scores by Grade

# * Create a table that lists the average Reading Score for students of each grade level (9th, 10th, 11th, 12th) at each school.
# 
#   * Create a pandas series for each grade. Hint: use a conditional statement.
#   
#   * Group each series by school
#   
#   * Combine the series into a dataframe
#   
#   * Optional: give the displayed data cleaner formatting

# In[30]:

```
clean_student_data
ninth_grade_data = clean_student_data.loc[clean_student_data['grade']=='9th'].groupby('school_name')["math_score"].mean()
tenth_grade_data = clean_student_data.loc[clean_student_data['grade']=='10th'].groupby('school_name')["math_score"].mean()
eleventh_grade_data = clean_student_data.loc[clean_student_data['grade']=='11th'].groupby('school_name')["math_score"].mean()
twelfth_grade_data = clean_student_data.loc[clean_student_data['grade']=='12th'].groupby('school_name')["math_score"].mean()

math_data_summary = pd.DataFrame({
     "School Type": school_types,
     "9th Grade" : ninth_grade_data,
     "10th Grade" : tenth_grade_data,
     "11th Grade": eleventh_grade_data,
     "12th Grade" : twelfth_grade_data
    
    
    
})
math_data_summary["9th Grade"] = math_data_summary["9th Grade"].map("{:.2f}%".format)
math_data_summary["10th Grade"] = math_data_summary["10th Grade"].map("{:.2f}%".format)
math_data_summary["11th Grade"] = math_data_summary["11th Grade"].map("{:.2f}%".format)
math_data_summary["12th Grade"] = math_data_summary["12th Grade"].map("{:.2f}%".format)
                        
math_data_summary

```
# ## Reading Score by Grade 

# * Perform the same operations as above for reading scores

# In[31]:
```

clean_student_data
ninth_grade_data = clean_student_data.loc[clean_student_data['grade']=='9th'].groupby('school_name')["reading_score"].mean()
tenth_grade_data = clean_student_data.loc[clean_student_data['grade']=='10th'].groupby('school_name')["reading_score"].mean()
eleventh_grade_data = clean_student_data.loc[clean_student_data['grade']=='11th'].groupby('school_name')["reading_score"].mean()
twelfth_grade_data = clean_student_data.loc[clean_student_data['grade']=='12th'].groupby('school_name')["reading_score"].mean()

reading_data_summary = pd.DataFrame({
     "School Type": school_types,
     "9th Grade" : ninth_grade_data,
     "10th Grade" : tenth_grade_data,
     "11th Grade": eleventh_grade_data,
     "12th Grade" : twelfth_grade_data
    
    
    
})
reading_data_summary["9th Grade"] = reading_data_summary["9th Grade"].map("{:.2f}%".format)
reading_data_summary["10th Grade"] = reading_data_summary["10th Grade"].map("{:.2f}%".format)
reading_data_summary["11th Grade"] = reading_data_summary["11th Grade"].map("{:.2f}%".format)
reading_data_summary["12th Grade"] = reading_data_summary["12th Grade"].map("{:.2f}%".format)

```
# In[32]:

```
reading_data_summary

```
# ## Scores by School Spending

# * Create a table that breaks down school performances based on average Spending Ranges (Per Student). Use 4 reasonable bins to group school spending. Include in the table each of the following:
#   * Average Math Score
#   * Average Reading Score
#   * % Passing Math
#   * % Passing Reading
#   * Overall Passing Rate (Average of the above two)

# In[33]:

# Sample bins. Feel free to create your own bins.
```
spending_bins = [0, 585, 615, 645, 675]
group_names = ["<$585", "$585-615", "$615-645", "$645-675"]

school_data_complete['spending_bins'] = pd.cut(school_data_complete['budget']/school_data_complete['size'], spending_bins, labels = group_names)
```

# In[34]:

```
by_spending = school_data_complete.groupby('spending_bins')
```

# In[35]:

```
avg_math = by_spending['math_score'].mean()
avg_read = by_spending['reading_score'].mean()
pass_math = school_data_complete[school_data_complete['math_score'] >= 70].groupby('spending_bins')['Student ID'].count()/by_spending['Student ID'].count()
pass_reading = school_data_complete[school_data_complete['reading_score'] >= 70].groupby('spending_bins')['Student ID'].count()/by_spending['Student ID'].count()
Overall_df = (pass_math + pass_reading)/2
```

# In[36]:

```
#dataFrame
scores_per_spend = pd.DataFrame({
    "Average Math Score": avg_math,
    "Average Reading Score": avg_read,
    '% Passing Math': pass_math,
    '% Passing Reading': pass_reading,
    "Overall Passing Rate": Overall_df

})

```
# In[37]:

```
#re-ordering th columns 
scores_per_spend = scores_per_spend[[
    "Average Math Score",
    "Average Reading Score",
    '% Passing Math',
    '% Passing Reading',
    "Overall Passing Rate"
]]

scores_per_spend.index.name = "Spending ranges"
scores_per_spend = scores_per_spend.reindex(group_names)

#formating
scores_per_spend.style.format({'Average Math Score': '{:.2f}%', 
                              'Average Reading Score': '{:.2f}%', 
                              '% Passing Math': '{:.2%}', 
                              '% Passing Reading':'{:.2%}', 
                              'Overall Passing Rate': '{:.2%}'})

```
# ## Scores by School Size

# * Perform the same operations as above, based on school size.

# In[38]:
```

# Sample bins. Feel free to create your own bins.
size_bins = [0, 1000, 2000, 5000]
group_names = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]

school_data_complete['size_bins'] = pd.cut(school_data_complete['size'], size_bins, labels = group_names)
```

# In[39]:

```
by_size = school_data_complete.groupby('size_bins')

#calculations 
avg_math = by_size['math_score'].mean()
avg_read = by_size['reading_score'].mean()
pass_math = school_data_complete[school_data_complete['math_score'] >= 70].groupby('size_bins')['Student ID'].count()/by_size['Student ID'].count()
pass_reading = school_data_complete[school_data_complete['reading_score'] >= 70].groupby('size_bins')['Student ID'].count()/by_size['Student ID'].count()
Overall_df = (pass_math + pass_reading)/2


#dataFrame
scores_per_size = pd.DataFrame({
    "Average Math Score": avg_math,
    "Average Reading Score": avg_read,
    '% Passing Math': pass_math,
    '% Passing Reading': pass_reading,
    "Overall Passing Rate": Overall_df

})

scores_per_spend.index.name = "School size"
scores_per_spend = scores_per_spend.reindex(group_names)

scores_per_size.style.format({'Average Math Score': '{:.2f}%', 
                              'Average Reading Score': '{:.2f}%', 
                              '% Passing Math': '{:.2%}', 
                              '% Passing Reading':'{:.2%}', 
                              'Overall Passing Rate': '{:.2%}'})
```

# ## Scores by School Type

# * Perform the same operations as above, based on school type.

# In[40]:

```
by_type =school_data_complete.groupby('type')

avg_math = by_type['math_score'].mean()
avg_read = by_type['reading_score'].mean()
pass_math = school_data_complete[school_data_complete['math_score'] >= 70].groupby('type')['Student ID'].count()/by_type['Student ID'].count()
pass_reading = school_data_complete[school_data_complete['reading_score'] >= 70].groupby('type')['Student ID'].count()/by_type['Student ID'].count()
Overall_df = (pass_math + pass_reading)/2


#dataFrame
scores_by_type = pd.DataFrame({
    "Average Math Score": avg_math,
    "Average Reading Score": avg_read,
    '% Passing Math': pass_math,
    '% Passing Reading': pass_reading,
    "Overall Passing Rate": Overall_df

})

scores_by_type.index.name = "School type"


scores_by_type.style.format({'Average Math Score': '{:.2f}%', 
                              'Average Reading Score': '{:.2f}%', 
                              '% Passing Math': '{:.2%}', 
                              '% Passing Reading':'{:.2%}', 
                              'Overall Passing Rate': '{:.2%}'})

```





