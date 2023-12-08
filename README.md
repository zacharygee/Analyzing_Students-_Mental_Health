# Analyzing Students Mental Health
This analysis aims to look at the effects of attending a university in a different country on students' mental health. The dataset is based on a 2018 survey conducted by a Japanese international university. 

## Table of Contents
1. [Exploring and Understanding the Data](#exploring-and-understanding-the-data)
2. [Understanding the Data for Each Student Type](#understanding-the-data-for-each-student-type)
3. [Querying the Summary Statistics of the Diagnostic Scores for All Students](#querying-the-summary-statistics-of-the-diagnostic-scores-for-all-students)
4. [Summarizing the Data for Only International Students](#summarizing-the-data-for-only-international-students)
5. [Seeing and Understanding the Impact of Length of Stay](#seeing-and-understanding-the-impact-of-length-of-stay)

## Exploring and Understanding the Data
To start off the analysis, I start by looking at the entire dataset from the CSV file:
```
SELECT * 
FROM 'students.csv';
```
This code also saves the CSV file as students. I move on to looking at the number of students included within the dataset while also identifying how many NULL values there are:
```
SELECT inter_dom, 
COUNT(*) AS count_inter_dom
FROM students
GROUP BY inter_dom;
```

## Understanding the Data for Each Student Type
To better understand the data, I filter to understand the data for each student type using the following queries:
```
SELECT * 
FROM students
WHERE inter_dom = 'Inter';
```
This query looks at all the fields for international students.
```
SELECT *
FROM students
WHERE inter_dom = 'Dom';
```
This query looks at all the fields for domestic students.
```
SELECT *
FROM students
WHERE inter_dom IS NULL;
```
Just for fun, this query looks at all the fields for NULL inter_dom values.

## Querying the Summary Statistics of the Diagnostic Scores for All Students
To begin my analysis, I start by querying the summary statistics of the diagnostic scores for all students. This invovles looking at the minimum, maximum, and average scores from the PHQ-9, SCS, and ASISS tests.
```
SELECT inter_dom,
	   ROUND(MIN(todep), 2) AS min_phq,
	   ROUND(MAX(todep), 2) AS max_phq,
	   ROUND(AVG(todep), 2) AS avg_phq,
	   ROUND(MIN(tosc), 2) AS min_scs,
	   ROUND(MAX(tosc), 2) AS max_scs,
	   ROUND(AVG(tosc), 2) AS avg_scs,
	   ROUND(MIN(toas), 2) AS min_as,
	   ROUND(MAX(toas), 2) AS max_as,
	   ROUND(AVG(toas), 2) AS avg_as
FROM students
GROUP BY inter_dom;
```
This query groups the data by student type (international or domestic) and pulls the minimum, maximum, and average scores, rounded to two decimal places, for each student type. 

## Summarizing the Data for Only International Students
Since I only want to look at international students, I write a simple query to look at the results I just pulled for only international students:
```
SELECT *
FROM df5
WHERE inter_dom = 'Inter';
```
Note: the previous query's results were saved under a dataframe titled "df5".

## Seeing and Understanding the Impact of Length of Stay
Finally, I am able to look at the impact of length of stay:
```
SELECT stay, 
       ROUND(AVG(todep), 2) AS average_phq, 
       ROUND(AVG(tosc), 2) AS average_scs, 
       ROUND(AVG(toas), 2) AS average_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay DESC;
```
Based on the results from this query we see that fewer years stayed at the university are often associated with lower depression scores and, to some extent, lower acculturative stress scores. Social connectedness scores show some variability across the number of years a student stays at the university.
