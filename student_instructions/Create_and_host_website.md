# Part I: Create and host a website
© Explore Data Science Academy

---
### 1) Fork the Template Repository <a id='1_section_id'></a>
---

This repository acts as a resource containing all the requisite files and instructions that you need to successfully complete this project. To continue development and before you can move on to the following steps, you need to create a private fork of this repo using your own GitHub profile.

If you have any trouble forking the repo, you might find [this link](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/duplicating-a-repository) helpful.

| :zap: WARNING :zap:                                                                                     |
| :--------------------                                                                                   |
| This project represents an individual project. As such when forking this repo, you need to ensure that it is hosted *privately* within your GitHub account, free from collaboration form your peers or access from the public.| 

---
### 2) Modify the Portfolio Webpage Template <a id='2_section_id'></a>
---

Step two in the process is where you will create your website and add your relevant data science and machine learning portfolio projects. For this step we will provide you with a baseline template which you can change freely in certain areas. 

Below follows a brief description of the files in the bootstrap template:

| Folder           | File Name                | Description      |
| ---------------- | ----------------         | ---------------- |
| Assets/mail    | contact_me.js            | Contains all the code required to send data from the filled website form through the defined endpoint to the AWS Lambda function    |
| Assets/mail    | jqBootstrapValidation.js | This JavaScript file is used to check if data entered in the contact form on the website is in the correct format. If the data is in the expected format, the script will attempt to send this to a specified URL endpoint. If the data is in an incorrect format, an error message will be displayed prompting the user to input the correct information.    |
| Assets/img     | n/a                      | This folder contains all the images used within the website.                    |
| css            | style.css                | This CSS (Cascading Style Sheets) file is responsible for controlling the style i.e. look and feel of the website    |
| js             | scripts.js               | Various scripts to increase the responsiveness and utility of the website.    |
| Project root            | index.html               | This file defines your static web page, and will be worked on when personalising the website to meet your preferences.   |

| :triangular_flag_on_post: PROJECT TASK :triangular_flag_on_post:                                                                                      |
| :--------------------                                                                                   |
| In this section of the project you are tasked with **modifying the static website template so that it represents your unique blend of technical skills**. Below you will find some helpful tips that might come handy during this website design/modification journey.  | 

**Steps to modify the provided bootstrap template to make the static website your own:**

  I. Open the `index.html` file with an editor of your choice i.e. Pycharm, Brackets, VS Code, etc. and customise the website according to your preference. You can find some general guidance of what to change by reading the code comments we have placed for you. Here is an example comment that you can expect to find in the `index.html` file:  
 
```html
  <!-- ========= CUSTOMISE SECTION =========== -->
  <!-- This comment instructs you on what to change and how to change it -->
  <!-- ======================================= --> 
```

| :zap: WARNING :zap:                                                                                     |
| :--------------------                                                                                   |
| While you are free to modify many aspects of the webpage, you should **NOT** alter its functioning. Do not add/remove any fields from the form section, nor modify the variable names captured by the `contact_me.js` script  | 
 
 II. Once you've modified the bootstrap template according to your preference, you can move to the next step. Here you will get your hands dirty serving the static website with the help of AWS Amplify :)
 

---  
### 3) Serve Static Web Page on AWS Amplify <a id='3_section_id'></a>
---

To serve your static website, you will make use of the AWS Amplify service. As mentioned before, AWS Amplify simplifies the process of web development by providing a serverless framework which removes the need to worry about a running webserver or underlying hosting infrastructure.


| :triangular_flag_on_post: PROJECT TASK :triangular_flag_on_post:                                                                                      |
| :--------------------                                                                                   |
| In this section of the project you again get the chance to showcase your cloud computing skills. You are tasked with using AWS Amplify to serve your modified website in a serverless manner. 

Here it is important to **serve your website with AWS Amplify via your GitHub repository**. That way, when you make any changes to the files sitting in the `deployed` branch, AWS Amplify will automatically pick up the branch changes and re-deploy your website. | 

  
| :information_source: NOTE :information_source:                                                                                                    |
| :--------------------                                                                                                                             |
| By this point in the process your modified website should be running through the AWS Amplify service with a sample domain name: `branch_name.reference_number.amplifyapp.com`. You can view an example of the fully functioning website deployed with AWS Amplify [here](https://main.dajjrrhheaglb.amplifyapp.com/)|
