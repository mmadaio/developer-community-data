# Online Developer Community Surveys 
Analyses of the gender gap in online software developer communities for the UNU-CS Gender Tech Lab  
August, 2018  

## Overview

Online developer communities boast millions of users - over 29 million on GitHub and over 8 million on Stack Overflow, in 2018. Participation in these communities is becoming one of the primary ways software developers learn new programming languages, improve their skills, develop collaborative projects, and find new job opportunities. (David and Shapiro, 2008; Ford et al., 2016; Vasilescu et al., 2015)  

Developers on these sites may ask and answer coding questions to improve their skills (e.g. Stack Overflow), use those skills to contribute to open-source code (e.g. GitHub) and participate in coding challenges (e.g. HackerRank). These platforms are becoming increasingly important to hiring decisions, as recruiters look at GitHub contributions or reputation on Stack Overflow as indicators of developers' skill.

However, despite the promise for online software developer communities to support software developers in their professional development, there are indicators that there may be serious difference in women and men's\* participation in these communities - differences which may further exacerbate existing gender gaps in the global ICT workforce.

\* *  See the UNU-CS EQUALS [project page](https://cs.unu.edu/research/equals-inaugural-report/) for how gender is being used by the UN and the EQUALS project for the purposes of this research. 


### Research questions

To understand the extent and nature of the gender gap in online software developer communities, we ask the following research questions:

1. How do male and female developers differ in their participation in online software developer communities?
2. How do male and female developers differ in their perceptions of belonging and kinship in online software developer communities?
3. How do male and female developers in online software developer communities differ in their employment and prior experience with coding?


### Data
We use publicly available survey data from 3 major online developer communities:

- Stack Overflow [survey](https://insights.stackoverflow.com/survey/2018/) (download latest survey results [here](https://drive.google.com/uc?export=download&id=1_9On2-nsBQIw3JiY43sWbrF8EjrqrR4U))
- GitHub [survey](http://opensourcesurvey.org/2017/) (download latest survey results [here](https://github.com/github/open-source-survey/releases/download/v1.0/data_for_public_release.zip))
- HackerRank [survey](https://www.kaggle.com/hackerrank/developer-survey-2018/home) (download latest survey results [here](https://www.kaggle.com/hackerrank/developer-survey-2018/))

### Files

- Statistical analyses of survey data (Chi-squares, log-linear models, etc)
- Visualizations of descriptive statistics of survey data (bar charts, cross-tabulated heatmaps, etc)
- Re-usable data cleaning scripts (cleaning country names, etc)

### Usage

- To view the results of the analyses, clone or download this repository using the green button, then open the `.html` files in your browser.
- To modify or re-run the code, some basic familiarity with Python and the Jupyter development environment may be necessary. See a tutorial [here](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html) for assistance beginning to work with Jupyter.
- Re-usable data cleaning scripts are located in the `preprocessing` folder
- The primary analysis results are in the `developer-surveys` folder. The `developer_survey_analyses` script is the main analysis file, with additional visualizations in the `developer_survey_visualizations` file.

## Authors
* Michael A. Madaio (mmadaio@cs.cmu.edu)
* Dr. Araba Sey
