# Part I: Create and initialise AWS resources
© Explore Data Science Academy

---
### 4) Create DynamoDb Database <a id='4_section_id'></a>
---

  
Within the proposed system, a database is required that will store all the data sent from the serverless hosted website. For this project, you will be using the AWS DynamoDB NoSQL database for this purpose. 

The data sent from the website will have the following schema:

> | Variable Name    | Variable Data Type | VariableDescription |
> | ---------------- | ----------------   |  ----------------   |
> | Name             | String             |  Name of the person filling out the form on the website                   |
> |                  |                    |                     |
> | Email Address    | String             |  Email address of the person filling out the form on the website          |
> |                  |                    |                     |
> | Phone Number     | Integer            |  Phone number of the person filling out the form on the website           |
> |                  |                    |                     |
> | Message          | String             |  Message to you from the person filling out the website form              |
> |                  |                    |                     |


We will create an additional parameter called `ResponsesID`.

> | Variable Name    | Variable Data Type | VariableDescription |
> | ---------------- | ----------------   |  ----------------   |
> | ResponsesID      | Integer            | Primary key used to identify reponse instance      |

The following steps will help you set this service up within the AWS ecosystem:
  
  I. Navigate to the AWS DynamoDb service via the AWS Management Console
  
  II. On the DynamoDB `Dashboard` select `Create Table`.

  <p align="center">
  <img src="https://raw.githubusercontent.com/Explore-AI/Pictures/master/db-1.PNG" style="width:550px;"/>
    <br>
    <em>Figure 2: Create a DynamoDB table</em>
  </p>
  
  III. Give your table a relevant name, for example, `my-portfolio-data-table`. Store this name for use in later stages of the project.
  
  IV. In the `Primary Key` field insert `ResponsesID` and set the type to number.
  
  <p align="center">
  <img src="https://raw.githubusercontent.com/Explore-AI/Pictures/master/db-2.PNG" style="width:550px;"/>
    <br>
    <em>Figure 3: Create a DynamoDB table</em>
  </p>  
  
  V. Click on `Create`.
  
  VI. Select the table you just created from the side panel,  and under the `Items` tab,  click on `Create Item`.
  
  <p align="center">
  <img src="https://raw.githubusercontent.com/Explore-AI/Pictures/master/db-3.PNG" style="width:750px;"/>
    <br>
    <em>Figure 4: Add new items and their data types to created table</em>
  </p>  
  
  VII. Set an initial index by entering the number '100' in the `ResponsesID` field. (Note that this field represents a unique primary key that will be generated within the Lambda function upon its execution)
  
  VIII. Click on the `+` icon and select `insert - number`. Name this item `Cell`. Enter `0123456789` in the Value field.
  
  IX. Click on the `+` icon and select `insert - string`. Name this item `Email`. Enter `student@explore-ai.net` in the Value field.
  
  X. Click on the `+` icon and select `insert - string`. Name this item `Message`. Enter `Empty Message` in the Value field.

  XI. Click on the `+` icon and select `insert - string`. Name this item `Name`. Enter `Student` in the value field.
  
  <p align="center">
  <img src="https://raw.githubusercontent.com/Explore-AI/Pictures/master/db-4.PNG" style="width:1000px;"/>
    <br>
    <em>Figure 5: Final populated table after following steps VIII - XI.</em>
  </p>   
  
  XII. Store the table entry by clicking on `Save`.
  
  XIII. Select the created table and navigate to the `Overview` tab.
  
  XIV. Scroll down and note your Amazon Resource Name (ARN)	for the created table. *Save this for later use in the IAM policy creation steps*.
  
--- 
### 5) Create an IAM Role <a id='5_section_id'></a>
---

In step 5 you once again get the chance to show off your cloud computing skills. This project involves using *a lot* of AWS services, and as such we need a comprehensive IAM Role that will adequately govern the access and authority of the AWS Lambda function. In this project you will make use of the following services:

    - AWS Lambda
    
    - AWS API Gateway
    
    - AWS Comprehend
    
    - AWS SES
    
    - AWS DynamoDB
   
  
You therefore need an IAM role to give the Lambda function the required access to the various services that we will be using in this project. In total, you need to create four policies for this IAM Role:

 | **Policy** | **Description** |
 |------------|-------------|
 |**AWS Comprehend Policy** | Allows the Lambda function to call AWS Comprehend in order calculate a sentiment score and extract key phrases from received text entered on the website. |
 |**AWS SES Policy** | Allows the Lambda function to invoke the AWS SES service in order to send automated responses via email. |
 |**AWS Basic Lambda Execution Policy** | Grants the Lambda function permission to access AWS services and resources. |
 |**AWS DynamoDB Policy** | Gives the Lambda function write permissions in order to store data from the website to a designated (existing) AWS DynamoDB table. |

| :triangular_flag_on_post: PROJECT TASK :triangular_flag_on_post:                                                                                      |
| :--------------------                                                                                   |
| You are tasked with **using the AWS IAM service to set up the necessary policies as described above**. It is important to remember that the DynamoDB policy will be of type inline, and that you will need to use the specific DynamoDB table ARN associated with the table you created in [Step 4](#4_section_id) to appropriately set up this policy.| 


---
### 6) Set-up the Initial AWS Lambda Function and the AWS API Gateway Trigger <a id='6_section_id'></a>
---
  
In this step you will set up the AWS API Gateway and the initial AWS Lambda function. This will be a three-part process:

| :triangular_flag_on_post: PROJECT TASK :triangular_flag_on_post:                                                                                      |
| :--------------------                                                                                   |
| Below is a brief description of the required three part process. You are, however, tasked with executing the three parts and little-to-no additional guidance will be give here. This is because, setting up an AWS Lambdas function is a key outcome of this course and should be something you are very familiar with by this time.| 


#### Part 1: Lambda Initialisation

Part 1 involves creating the AWS Lambda Function with a Python 3.7 runtime. You will also attach the IAM role created within step 5 to govern the lambda function's access control.
  
  
#### Part 2: Layer Addition

In part 2, the objective is to add an ARN  layer to the generated lambda function - one for Numpy. You need to add this layer so that you may use the Numpy library building your solution i.e. the Python code telling the lambdas function what to do. Remember these layers are `region` and `runtime` specific. Find your relevant ARN [here](https://github.com/keithrozario/Klayers).
 
#### Part 3: API Gateway Creation

The final part within step 6 is to create the AWS API Gateway and set this endpoint as your lambda's function trigger. At a high level, this gateway is responsible for interfacing between your deployed website (running from AWS Amplify) and the lambda function you have created - invoking the lambda when the gateway receives an API call (HTTP POST request sent by the webpage) and passing through its data payload to the function. This process sets off a chain of programmatic events which ultimately will generate and send your intelligent email automatically.

Your API should have the following characteristics: 
 - **API Type**: HTTP.
 - **Supported Method**: POST.
 - **Utilised Authorization**: None.
 - **[Cross-origin resource sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)**: Enabled.
 - **Detailed billing**: Disabled.

Once you have created the API Gateway, you should replace the URL of the endpoint in your `contact_me.js` file to your specific endpoint URL. The following steps might help you in your endeavour. 

 
  I. Note the `API endpoint` address under the configured trigger.
  
  II. Use this API and navigate to the `contact_me.js` file in your GitHub repo created in step 1.
  
  <p align="center">
  <img src="https://raw.githubusercontent.com/Explore-AI/Pictures/master/lam-3.PNG" style="width:500px;"/>
    <br>
    <em>Figure 6: Created API Gateway</em>
  </p> 
  
  III. Replace the existing API URL with your API endpoint.
  
  IV. Commit the changes and wait for the branch to be redeployed by AWS Amplify.
  
  
| :information_source: NOTE :information_source:                                                                                                    |
| :--------------------                                                                                                                             |
| By this point in the process your website should enable you to fill out the form with the appropriate information and, upon submission, you should receive the `Your message has been sent` notification. |