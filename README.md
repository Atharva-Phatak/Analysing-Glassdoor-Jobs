# Glassdoor Jobs Data-Analysis 

I came up with this personal personal project to test my skills to the fullest and learn new things. In this project I scraped job postings related to the position of 'Data Scientist' from glassdoor.com,analysed the gathered data and framed a machine learning problem out of it. In the below write up I'll mention the details on what I learned. 
I selected states of California,Washington,New York as major areas to find the roles.

# Please read the readme.md file to understand what I found.

***The project consists of three main jupyter notebooks***

1. **The first notebook** : scrape_data.ipynb is a python script for scraping job postings from glassdoor.com
2. **The second notebook** : glassdoor_eda.ipynb is the notebook contaning exploratory data analysis and contains the insights which I was able to glean from it.
3. **The third notebook** :modelling.ipynb contains a basic machine learning model to solve the framed problem and most importantly it includes sections on 'Machine Learning Explainability'

## About Glassdoor

![glass](https://upload.wikimedia.org/wikipedia/commons/e/e1/Glassdoor_logo.svg)

Glassdoor is a website where current and former employees anonymously review companies. Glassdoor also allows users to anonymously submit and view salaries as well as search and apply for jobs on its platform.Glassdoor launched its site in 2008 , as a site that “collects company reviews and real salaries from employees of large companies and displays them anonymously for all members to see,” according to TechCrunch. The company then averaged the reported salaries, posting these averages alongside the reviews employees made of the management and culture of the companies they worked for—including some of the larger tech companies like Google and Yahoo. The site also allows the posting of office photographs and other company-relevant media.

## Stage I : Data Scraping

In this part of the project I developed a webscraper which scrapes data from glassdoor.com.
Here's how I went about creating it.
1. The most important part of web scraping is understanding the website which your are scraping and by understanding I meant looking at the source code of the website in your browser.(I spent about 2 days understanding the structure of the website and locating the elements which I needed to find.
2. The elements which I was gonna scrape were Company name,Job Title,Salary,Ratings,Job Description etc. The easiest part was to scrape the elements except job description because to see the whole job description one has to click on the company tab which contains job description.
![img](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/Job_Desc.PNG)

***There were two approaches to deal with it*** : 
  * I use selenium to click on the tabs and extract the information I needed.
  * I used a different one, I used selenium to extract all the links present on the webpage and fitered the links having /jobPosting in    their URL.
  
3. Once I extracted all the links from all the pages that were present on the glassdoor.com. I figured out how many jobs were present on each page which turned out to be 30.

4. Then I went to every exracted link using selenium got the page source code using a beautiful soup object and extracted the required elements.
  
    * Interesting thing was all the details like job title,Company name,salary,etc were present in a Json file under </script> tag, so I       extracted the content of the json and got most of  my info.
![img](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/Json_ld.PNG)
    * I found another class_ id for <div> tage which contained job description so I used it to extract the job description.
5 After extracting the information I stored it in various .csv files according the search query.
  
## Stage II : Exploratory Data Analysis a.k.a EDA

First of all I used interactive plots in it. I used plotly and didn't use the offline version to create the plots sorry if you cannot see the plots, If you want to see the notebook with the plots here is link : [eda notebook](https://beta.deepnote.com/publish/8bef745c-0d96-4e88-89e8-36bd581ff1e0-8e38a13f-c62b-49a3-afb8-28aec94fa195)

I ran all of my code on deepnote, you should look them up.

1) When I read  all the data from the CSV files. I found that it contained duplicated rows, so my first task was to delete them. After that pretty much the data was clean because of the good scraping we did.

2)***Beginning of EDA***

There are 12 columns in the data they are as follows:
1. Job_title: The title of job which you are applying to
2. Company : Company name
3. State/City : State/City in which the companies job posting is listed.
4. Min_Salary : Minimum yearly salary in USD.
5. Max_Salary : Maximum yearly salary in USD.
6. Job_Desc : The job description which included skills,requirements,etc
7. Industry : The industry in which the company works.
8. Date_posted : The date  on which the job was posted on glassdoor
9. Valid_until: The last date of applying to the job.
10.Job_Type : Type of job full-time , part-time,etc.
11.Rating : Rating of the company
 
a.  **State column**  
   
   * As mentioned above I used California, Washington and New york as place queries. But the scraper also collected some data from other states like Texas,Maryland,Virgina,etc maybe because these resultes were also the part of the search.
   * First step I did is  I saw the number of jobs in each city of the state and found out the top 5 cities. Turns out CA and TX have the most number of jobs. City of San Fransico was dominating with the most number of jobs.
   ![jobs](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/State_jobs.png)
   
   * The next step was to see the average annual minimal and maximal salaries in the states
   ![sals](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/Sal_states.png)
   
   ***What's the Inference about salaries ?***
   
   * Minimal Average salary for NY is greater CA & DC, that can be because of the less data points for NY state indicating it is an outlier.
    
   * Both DC and California offer almost the same average salaries both minimal and maximal.
 
 * Now I saw which city offered average minimal annual salary.
 ![img](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/city_sal.png)
 
 * Outcome : South San Fransico is the city with highest minimal annual salary.
 
b. **Industry Column** 

![img](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/ind_dom.png)
* As expected companies which come under IT sector have max number of jobs followed by industries which come under Business services.
* Though one intersting thing for me was Biotech requires more amount data science related individual than finance atleast for this data set.


* Salaries for the top 8 industires having most number of jobs
![img](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/min_max_sal_ind.png)
* Outcome : Agriculture and Forestry is an outlier if because in whole of the data there is only one example of it. **IT industry pays the best avg minimal and maximal yearly salary.**

c. **Exploring the Company Columns***

* There are in total 959 unique company names meaning some companies have many openings even during covid-19 pandemic. Here a plot of top 20 companies javing most job openings

![alt](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/comp_jobs.png)

* According to the data and the above plot Genentech,Booz Allen Hamilton Inc.,Amazon are the companies with the most number of openings.

* Average annual minimal & maximal salaries for top 5 companies with max job postings in each state. The plots sequence of plots is
  **California,Texas,DC,Virginia,Maryland**


![alt](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/CA_plot.png)



![alt](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/TX_plot.png)



![alt](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/DC_plot.png)



![alt](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/VA_plot.png)


*
![alt](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/MD_plot.png)

d. **Job Titles**

* There were many unique values for the job title feature. Here is the plot for top 8 job titles with max openings

![alt](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/title_njobs.png)

* The top 3 titles were : Data Scientist,Data Engineer,Data Analyst.

* The salaries for the top 3 titles
![alt](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/top3_sals.png)


e. **Job Description** 

* I decided to use the count vectorizer to find the most occuring bi-grams in the job description and use them as features

![alt](https://github.com/Atharva-Phatak/Glassdoor-Jobs_Data-Analysis/blob/master/Images/skills_ngrams.png)

* The terms machine learning,data scientist, experience occur most number of times in job description.

## Stage III : Modelling 

I have exhaustive explanation in the modelling notebook you can look at it there. There are explanations for every plot and method I used.

# If you liked reading this please show your appreciation by giving my work some stars.

## Requirements

* Python 3.7
* Plotly
* Shap
* eli5
* seaborn/matplotlib
* Scikitlearn


 
   


  
 
 
  
 

