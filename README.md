# **School District Analysis**

## **Overview**
>For this analysis, Maria and her supervisor, have been notified by the school board that there is suspicion that the scores of the ninth graders from Thomas High School (THS) have been altered and may be displaying inaccurate data. To remedy this situation, we have been asked to prepare multiple data frames excluding the scores grades from to ensure that our summaries on grades and school spending reflect our data and outcomes truthfully. To do this task we will be utilizing [pandas](https://pandas.pydata.org/docs/user_guide/index.html#user-guide), a popular Python data analysis library, to retrieve the desired variables with ease. After we have collected our new results, we can observe whether or not removing the compromised data had any noticeable changes and report back to Maria.

---

## Code


###### Here is a link to the [full challenge code](https://github.com/annaS000/School_District_Analysis/blob/main/PyCitySchools_Challenge.ipynb) that followed the starter code that was given.

---

I decided to revise this code to simplify the steps taken to produce our outputs, which can be viewed here : [full revised challenge code](https://github.com/annaS000/School_District_Analysis/blob/main/city%20schools%20simplified%20code.ipynb). 

Some of the this I did to change the original code included:
1. Created functions that were used throughout the code.

        def getThis(df,col,pandas_stat):
            return getattr(df[col], pandas_stat)()

        def getSeries(df,col):
            return df[col]
                
        def getPercent(x,y):
            return x / y * 100

        def makeDF(dic):
            return pd.DataFrame(dic)

        def formatThis(df,col,how):
            return df[col].map(how.format)
            
        def groupThis(df,group):
            return df.groupby([group])
        
> Making these functions was my attempt to decrease the visual load throughout my program. I was pleased with the result of using these functions but, I am sure they could still be improved upon to make this code even more efficient.
2. Stored column names and variables in lists to be zipped together.

        columnList = ["Total Schools", "Total Students", "Total Budget","Average Math Score", 
                    "Average Reading Score", "% Passing Math","% Passing Reading", "% Overall Passing"]

        #Get values
        schoolCount = len(getThis(mergedDF,'school_name','unique'))
        totalStudents = isnaCounts[False]
        totalBudget = getThis(school_data_df,'budget','sum')
        avgMath = getThis(mergedDF,'math_score', 'mean')
        avgReading = getThis(mergedDF,'reading_score','mean')
        passingMath = getPercent(getThis(passedMath,'Student ID','count'),totalStudents)
        passingReading = getPercent(getThis(passedReading,'Student ID','count'),float(totalStudents))
        passingOverall = getPercent(getThis(passedOverall,'Student ID','count'),float(totalStudents))

        districtVars = [schoolCount, totalStudents, totalBudget, avgMath,  
                        avgReading, passingMath, passingReading, passingOverall]

        #Make dictionary of column names and values
        districtDict = dict(zip(columnList,districtVars))

        #Make data frame
        districtSummary = makeDF([districtDict])
> I felt that creating these lists will made it easier for me to manipulate the data and make any edits. Additionally, I prefer using `zip()` and `dict()` to create the dictionary for my data frame.

3. Utilized list comprehensions to get values instead of creating manying variables

        gradeLevels = ['9th','10th','11th','12th']

        grade = [mergedDF[mergedDF['grade'] == i] for i in gradeLevels]

        #The average math score for each grade level from each school (3 pt)
        mathBygrade = [getThis(groupThis(i,'school_name'),'math_score','mean') for i in grade]

        #The average reading score for each grade level from each school (3 pt)
        readingBygrade = [getThis(groupThis(i,'school_name'),'reading_score','mean') for i in grade]

        #Save to data frames
        math_scores_by_grade = makeDF(dict(zip(gradeLevels,mathBygrade)))
        reading_scores_by_grade = makeDF(dict(zip(gradeLevels,readingBygrade)))
> Since each series being created here used the same function to populate their values list comprehensions will be faster. This method also means if I make an error, I only have to correct it once rather than having to correct multiple variables.


4. Used `for loops` to do my formatting 

        for i in gradeLevels:
            math_scores_by_grade[i] = formatThis(math_scores_by_grade,i,"{:.1f}")
            reading_scores_by_grade[i] = formatThis(reading_scores_by_grade,i,"{:.1f}")
> Using a `for loop` takes up less room in the cell so, it looks a lot less cluttered this way
---

## **Results** 
* **How is the district summary affected?**

    



    ##### **District Summary Before**
    ![District Summary Before](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/distr_sum_before.png) 

    ##### **District Summary After**
    ![District Summary After](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/district%20summary%20after.png) 

    ##### **District Summary Comparison**
    > ##### Here we can see the **District Summary** data frame before and after the THS 9th grade scores have been converted into NaNs. While there are some numbers that appear different in the after chart, this difference is so small that it would not be considered a significant change. If the formatting had rounded to the whole number the charts would be identical.

    ###### [back to summary](#summary)
    <br/>

* **How is the school summary affected?**

    ##### **School Summary Before**
    ![School Summary Before](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/school_sum_before.png) 

    ##### **School Summary After**
    ![School Summary After](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/school_sum_after.png) 

    ##### **School Summary Comparison**
    > ##### Initially, the **School Summary** charts display a much more considerable difference in values. The before chart shows THS has passing rates of 90% and above. Replacing THS's ninth grade data reveals much lower passing rates. This makes sense because the total number of students stayed the same while the number of student from THS went down.
        
    <br/>

    * **Since the scores of the ninth graders of THS are being excluded. The total number of students used to make these calculations must be adjusted to get the correct values for THS.**

    <br/>

    ##### **School Summary Before**
    ![School Summary Before](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/school_sum_before.png) 

    ##### **School Summary After with THS Adjusted Percents**
    ![School Summary After](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/THS%20replaced%20percent.png) 

    ##### **New School Summary Comparison**
    > ##### Now that we have fixed those values, we can see there was still a very slight fractional change but, like the district summary, I would consider this negligable.

    ###### [back to summary](#summary)
<br/>


* **How does replacing the ninth graders’ math and reading scores affect Thomas High School’s performance relative to the other schools?**

    * Since the other schools are independent of THS, their stats stayed constant throughout the changes that have been made. Now, we can take a look at the charts regarding the top and bottom 5 schools of the district.

    <br/>

    ##### **Top 5 Schools Before**
    ![Top 5 Schools Before](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/top5before.png) 

    ##### **Bottom 5 Schools Before**
    ![Bottom 5 Schools Before](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/bottom5before.png) 

    ##### **Top and Bottom 5 Schools Before**
    > ##### Before any changes were made Thomas High School was in second place for the top schools in the district based on the overall passing rates.

    <br/>

    ##### **Top 5 Schools After**
    ![Top 5 Schools After](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/top5after.png) 

    ##### **Botton 5 Schools After**
    ![Botton 5 Schools After](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/bottom5after.png) 

    ##### **Top and Bottom 5 Schools After**
    > ##### Even after the changes to the scores were made, THS remained in second place.

    ###### [back to summary](#summary)
    <br/>

* **How does replacing the ninth-grade scores affect the following:**
    * **Math and reading scores by grade:**

        <br/>

        ##### **Math Scores by Grade**
        <img src="https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/bygrademathbefore.png" width="200" height="250" >  <img src="https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/bygrademathafter.png" width="200" height="250" >
        ##### **Before and After Math Scores by Grade**
        > ##### There are no changes in math scores by grade after taking out the THS ninth grade scores.

        <br/>

        ##### **Reading Scores by Grade**
        <img src="https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/bygradereadbefore.png" width="200" height="250" >  <img src="https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/bygradereadafter.png" width="200" height="250" >
        ##### **Before and After Reading Scores by Grade**
        > ##### There are no changes in reading scores by grade after taking out the THS ninth grade scores.

        ###### [back to summary](#summary)
    <br/>

    * **Scores by school spending:**
        #### **Spending Summary Before No Formatting**
        ![](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/noformatspendingbefore.png)
        #### **Spending Summary Before**
        ![](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/spendingbefore.png)  

        #### **Spending Summary After No Formatting**
        ![](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/noformatspendingafter.png)
        #### **Spending Summary After**
        ![](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/spendingafter.png)

        ##### **Spending Summary Comparison**
        > ##### The **Scores by Spending Summary** does not have any changes

        ###### [back to summary](#summary)
        <br/>

    * **Scores by school size:**
        #### **Size Summary Before No Formatting**
        ![](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/noformatsizebefore.png)
        #### **Size Summary Before**
        ![](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/sizebefore.png)  

        #### **Size Summary After No Formatting**
        ![](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/noformatsizeafter.png)
        #### **Size Summary After**
        ![](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/sizeafter.png)

        ##### **Size Summary Comparison**
        > ##### There are virtually no changes in the **Scores by School Size Summary**

        ###### [back to summary](#summary)
    <br/>
    
    * **Scores by school type:**

        #### **Type Summary Before No Formatting**
        ![](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/noformattypebefore.png)
        #### **Type Summary Before**
        ![](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/typebefore.png)  

        #### **Type Summary After No Formatting**
        ![](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/noformattypeafter.png)
        #### **Type Summary After**
        ![](https://raw.githubusercontent.com/annaS000/School_District_Analysis/main/Resources/typeafter.png)

        ##### **Type Summary Comparison**
        > ##### The **Scores by School Type Summary** did not have any major changes as well.
    
---

## Summary
To recap, here are some of the changes that were in the updated school district analysis after reading and math scores for the ninth grade at Thomas High School have been replaced with NaNs:

1. One change that was made in the school district analysis was having to recalculate the passing rates for 10th-12th grades in Thomas High School since the total number of students went down from excluding the 9th grade class.
2. There were some fractional decreases in the THS scores and passing rates in all of the summary charts after the students were dropped from the data set. These decreases become unnoticable after the charts are rounded from the formatting.
3. In the Scores by Grade Charts the only difference is there a `nan` in the spot where the THS 9th grade average used to be because there is not data for that calculation anymore.
4. Overall, it was expected to have majority of the data in the summaries to stay the same because only some of the results are affected by the Thomas High School student count and scores. THS's statistics that are specific to their school would be changed by eliminating a whole grade because that affects them directly. As for other results, any statistics that are dependent on categories that THS fits in would also be affected.
    * The [District School Summary](#district-summary-before) was affect because THS is included in the calculations for that chart.
    * In the [Per School Summary](#school-summary-before), the only data subject to change was the THS row.
    * The [Top and Bottom 5](#top-5-schools-before) charts stayed the same because the diffences made to THS's stats were not big enough to raise or lower their standing.
    * The [Spending per Student Summary](#spending-summary-before-no-formatting) only had changes in the **$630-$644** row because that was the category that THS fell in with $638 per student.
    * The [School Size Summary](#size-summary-before-no-formatting) saw change in the **medium size** row because the school has 1,635 students.
    * The [School Type Summary](#type-summary-before-no-formatting) chart changed in the **Charter** row because THS is a charter school.

