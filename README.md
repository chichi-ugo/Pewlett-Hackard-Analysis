# Pewlett-Hackard-Analysis
Company-wide Database analysis using Postgres SQL

## Overview
In this project, we have been tasked with assisting in a company wide analysis for Pewlett Hackard to provide detailed information concerning the numbers of employees who will be retiring soon. 

### Purpose
The goal of this analysis is to determine the impact that the company will face by how many employees are posed to leave due to retirement, and to also provide information to help prepare the company's hiring initiative. 

## Results
To get a sense of the scope, below depicts the total number of employees at Pewlett Hackard: 300,024.
  ![](https://github.com/chichi-ugo/Pewlett-Hackard-Analysis/blob/main/Images/tot_emps.PNG?raw=true)

In order to move forward with our analysis, we first defined the retirement criteria. Employees considered as 'at retirement age' have birthdates between January 1, 1952 and December 31, 1955.

With this parameters, we determined that 72,458 employees retiring.
  ![](https://github.com/chichi-ugo/Pewlett-Hackard-Analysis/blob/main/Images/tot_retiring.PNG?raw=true)

With this established, we were able to determine the following in our further analysis:
- Retiring employees were grouped together in a table to see what titles were held by these individuals.
  ![](https://github.com/chichi-ugo/Pewlett-Hackard-Analysis/blob/main/Images/retire_titles.PNG?raw=true)
  - Here we see that many of these individuals have held multiple titles in their time working for PH (Pewlett Hackard). We also see that many of the employees included in this dataset have already left - based off of thier 'to_date'.
  
 - In order to procede with a more accurate picture of the current employees, we filtered out the employees who were no longer there - keeping only those with a indefinite 'to_date' - and we also removed the duplicates of the employes who have held mulitple titles, and only kept their more recent title.
  ![](https://github.com/chichi-ugo/Pewlett-Hackard-Analysis/blob/main/Images/unique_titles.PNG?raw=true)
    - We are now left with the 72,458 retiring employees and thier current titles.
  
- From here, we found the number of employees that held each title and created a table to display these values.
  ![](https://github.com/chichi-ugo/Pewlett-Hackard-Analysis/blob/main/Images/title_count.PNG?raw=true)
     - This table helps us to understand what roles will need to be filled when hiring new employees and how many people within each role they will need to hire. 

- A mentorship initiative is proposed to help ease the transition for new-hires in those roles. In order to quantify how many employees are eligible to become mentors, we set the parameter that mentors birthdates fell in the range of January 1, 1965 and December 31, 1965. We also excluded all non-current employees from this dataset. There are a total of 1,549 eligible mentors.

Total Number of Mentors | Mentors by Title
:----------------------:|:-----------------:
![](https://github.com/chichi-ugo/Pewlett-Hackard-Analysis/blob/main/Images/mentor_eligible.PNG?raw=true) | ![](https://github.com/chichi-ugo/Pewlett-Hackard-Analysis/blob/main/Images/tot_mentors.PNG?raw=true)

## Summary
- How many roles will need to be filled as the "silver tsunami" begins to make an impact?
  - 72,458 out of 300,024 employees are retiring, which is almost a quarter (24.2%) of Pewlett Hackard's employees.
  - To gain a better understanding of what roles would need to be filled, we can look at the data table quantifying the number of retirees for each title. However, to broaden this view, we also suggest looking at how many retirees are leaving from each department. Using the following queries in SQL, we were able to garner these results:
    ```
    SELECT DISTINCT ON (de.emp_no) d.dept_name, 
        de.emp_no, 
        de.dept_no, 
        e.first_name, 
        e.last_name
    INTO retire_dept
    FROM departments AS d
    INNER JOIN dept_emp AS de
    ON (d.dept_no = de.dept_no)
    INNER JOIN employees AS e
    ON (de.emp_no = e.emp_no)
    WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
        AND (de.to_date = '9999-01-01')
    ORDER BY de.emp_no;
    ```
    ![](https://github.com/chichi-ugo/Pewlett-Hackard-Analysis/blob/main/Images/retire_dept_count.PNG?raw=true)

- Are there enough qualified, retirement-ready employees in the departments to mentor the next generation of Pewlett Hackard employees?
  - Based off the criteria for our mentorship elegibility, we can see that there are far fewer people to be mentors than there are vacancies after the retirees leave. The image below shows the breakdown between titles of eligible mentors:
  ![](https://github.com/chichi-ugo/Pewlett-Hackard-Analysis/blob/main/Images/mentor_count_bytitle.PNG?raw=true)
  - We see that each tile is lacking in Mentors and this could put the next generation of PH employees at a disadvantage. We suggest for Pewlett Hackard to consider broadening their mentorship eligibility criteria in order to increase the numbers.
