# Part III: Putting it all together

© Explore Data Science Academy

---
### 7) AWS Lambda Function for Writing to DynamoDB <a id='7_section_id'></a>
---

You are now at a point in the project where you can start building the actual lambda functionality. Initially you will start off with the simple task of using the AWS Lambda + API Gateway to write the data coming from the website form to the previously created DynamoDB database. If needed, you can familiarise yourself with the overall process as represented within **figure 1**.

  <p align="center">
  <img src="https://raw.githubusercontent.com/Explore-AI/Pictures/master/cloud-computing-predict-dynamodb.gif" />
    <br>
    <em>Figure 7: Lambda writing data from the website to DynamoDB</em>
  </p> 


| :triangular_flag_on_post: PROJECT TASK :triangular_flag_on_post:                                                                                      |
| :--------------------                                                                                   |
| **Set up the lambda function to write the website POST data to DynamoDB**. To get the functionality displayed in figure 7, you will need to use Python to tell your AWS Lambda function what to do and how to do it. Luckily we have some stater code for you. You can use the code found [here](student_solution_files/basic_lambda_data_decoding.py) to read and decode the incoming data from the website. Then, using the boilerplate code found [here](student_solution_files/write_data_to_dynamodb.py) as starting point, you can enable your lambda function to write to DynamoDB.| 

---
### 8) AWS Lambda Function for Sending an Email with AWS SES <a id='8_section_id'></a>
---

In this step you will setup AWS Simple Email Service (SES), to programmatically send emails when required. This is a two-part process: 

 - The first part entails the verification of email addresses for the sending and receiving of messages generated by SES. 
 
 - The second part sees you configure your growing lambda function to send emails to specified addresses when triggered by a POST request being received by the API gateway.
   
#### Part 1: Verify your Email with Amazon SES:

When using Amazon SES for the first time, all accounts start out in sandbox mode. This is to prevent new accounts from abusing the service and sending out spam emails. In sandbox mode the following restrictions apply:

 - You can only send mail to verified email addresses and domains, or to the [Amazon SES mailbox simulator](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-email-simulator.html).

 - You can only send mail from verified email addresses and domains.

 - You can send a maximum of 200 messages per 24-hour period.

 - You can send a maximum of 1 message per second.
    
When your account is promoted out of the sandbox, you can send emails to any recipient, regardless of whether the recipient's address or domain is verified. However, you still have to verify all identities that you use as "From", "Source", "Sender", or "Return-Path" addresses.


| :information_source: NOTE :information_source:                                                                                                         |
| :--------------------                                                                                                                                  |
| You do not have to move your account out of Sandbox Mode complete the project. However, for interest, to move out of sandbox mode you can follow [these](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/request-production-access.html) instructions. |

| :triangular_flag_on_post: PROJECT TASK :triangular_flag_on_post:                                                                                      |
| :--------------------                                                                                   |
| There are two project-related tasks for this step. The first of these is the **verification of sender and recipient email addresses**. Here you will need to add `edsa.predicts@explore-ai.net` as a recipient address within SES. This email account forms part of our assessment process and will be used at a later stage for marking. Note that when requesting verification for this email address, *it may take up to 24 hours for confirmation to be given*. In addition to the explore email address, **you need to configure a sender and a recipient address respectively for your testing purposes**.| 

#### Part 2: Programmatically send an email via Amazon SES:

Having registered the necessary email addresses, you are now ready to invoke AWS SES within your lambda function to automate the sending of email messages. 

| :triangular_flag_on_post: PROJECT TASK :triangular_flag_on_post:                                                                                      |
| :--------------------                                                                                   |
| At this point we want you to once again show us your great cloud computing and python skills. The **second task involves sending a sample email to your recipient address from your sender address, using AWS SES**. Here we provide some basic guidance in the form of [this](student_solution_files/send_emails_with_ses.py) this template, which your are required to complete and to add to your lambda function in order to help you accomplish your end goal.| 

---
### 9) AWS Lambda Function for Using Amazon Comprehend <a id='9_section_id'></a>
---

In this final step you will be building out the project's NLP functionality with the help of AWS Comprehend. The NLP functionality will enable you to extract the overwhelming sentiment (categorical variable) from a message, as well as a list of its key phrases as determined by AWS comprehend. With the sentiment information and a list of key phrases, you can build in intelligent, automated email responses into your AWS Lambda function. To help you thoroughly understand this section, we provide a three-part breakdown wherein we describe each key element involved in the formation of an intelligent response. 

The **first part** is the process description; where we go through the logic of how to use AWS comprehend to extract sentiment and key phrases. The **second part** covers the helper functions utilised; where we describe two helper functions and the main lambda function required to orchestrate an intelligent automated response. The third and **final part** is an end-to-end example; where we simulate what should be achieved once the entire AWS Lambda/AWS Comprehend/AWS SES integration is built.

#### Part 1: Process Description

At a high-level, the entire intelligent response process can be described as follow:

  1. The form is filled out on the website and sent. This form contains the following sender data:
     - `name`
     - `cellphone number`
     - `email address`, and
     - `message`.
  
  2. The data passes through the AWS API Gateway and triggers the AWS Lambda function.
  
  3. The sender data is decoded.
  
  4. The `message` is used in AWS Comprehend to extract the message sentiment and a dictionary of key phrases.
  
  5. A custom helper function finds the score and value of the most likely sentiment present in the message.
  
  6. Another helper function is used to convert the list of key phrases, generated by AWS Comprehend, to a numpy array. This array contains individual words present within the key phrase dictionary. This helper function also performs a lookup - producing a set of key phrases which match a custom list of keywords we wish to detect and reply to.  
  
  7. Having extracted the above data from the received message, the following email logic can be applied:
        
```python

if message_sentiment == 'Desired_Sentiment':
    
    if (AWS_Comprehend_Extracted_Words == Provided_List_of_Words):
        generate some personalised response based on message content
        
    else:
        generate some generic response based on message content
        
```

#### Part 2: Helper Function Descriptions

In this section we provide an overview of the helper functions required to send the intelligent, automated email responses.

**Function 1 - `find_max_sentiment(Comprehend_Sentiment_Output):`**

This function extracts and returns the highest probable sentiment from a given message.
  - Input argument:
    - Comprehend_Sentiment_Output: Sentiment dictionary generated by AWS Comprehend.
  - Outputs:
    - overall_sentiment: A string representing the most probable sentiment within the message. 
    - sentiment_score: The value of the highest probable sentiment present in the message.
  - The function implementation can be found [here](student_solution_files/find_maximum_sentiment.py)
   
**Function 2 - `key_phrase_finder(list_of_important_phrases, list_of_extracted_phrases):`**

This function attempts to find a match between the words in a key phrases dictionary produced by AWS Comprehend to a provided custom list of words. The result of the search is returned as a boolean variable.
  - Input arguments:     
    - list_of_important_phrases: A string-based list of words representing topics to be detected i.e. `['CV', 'data', 'science']`
    - list_of_extracted_phrases: An AWS Comprehend key phrases dictionary. 
  - Outputs: 
    - listing: An empty list to append the individual words present in the AWS Comprehend key phrases dictionary.
    - phrase_checker: A boolean variable representing the whether a match is discovered between the function's input lists.
  - The function implementation can be found [here](student_solution_files/find_key_phrases.py)
            
**Function 3 - `email_response(name, email_address, critical_phrase_list, sentiment_list, sentiment_scores, list_of_extracted_phrases, AWS_Comprehend_Sentiment_Dump):`**
    
This function takes in the parsed information from the sender i.e. `name`, `email_address`, the `sentiment` and the `key phrases` output from AWS Comprehend, and a `list of critical phrases`, and uses the logic described in the 'Process Description' section to populate a email. 

  - Input arguments:     
    - name: The name of the requester.
    - email_address: The email address of the requester.
    - critical_phrase_list: A list of words that you want to match to the words in the AWS key phrases dictionary.
    - sentiment_list: A list of possible sentiments that a message could contain i.e. ['Positive', 'Negative', 'Neutral', 'Mixed'].
    - sentiment_scores: The sentiment score attached to each of the listed sentiments in the `sentiment_list` as determined by AWS Comprehend.
    - list_of_extracted_phrases: A list of the individual words present in the key phrases dictionary.
    - AWS_Comprehend_Sentiment_Dump: The sentiment summary dictionary as populated by AWS Comprehend.
  - Outputs: 
    - Text: The intelligently populated email response based on the contents of the sender's message.
  - - The function implementation can be found [here](student_solution_files/email_responses.py)

The email_response method works as follow:

  1. You supply a list of words which correspond to topics which you'd like to extract/filter for within an incoming website enquiry.
  2. The supplied list of words are matched with words extracted from the AWS Comprehend key phrases dictionary. 
  3. A `boolean` response is given for each match detected across various configured keyword categories.
  4. Given the match, or potentially multiple matches, you can set up logic to start building an email response sequentially.

| :triangular_flag_on_post: PROJECT TASK :triangular_flag_on_post:                                                                                      |
| :--------------------                                                                                   |
| The final task of the project requires you to use the provided functions and the boilerplate AWS Comprehend/AWS Lambda code found [here](student_solution_files/aggregated_lambda_function.py) to build out the full functionality of the automated project pipeline.| 
      

# End-to-end Example

In this section we will review a basic example of how an email response is generated with the full solution architecture.

Let's say someone visiting your deployed webpage posts the following sample message using the form:

> Hi Explore Data Science Student,
>
> I love your website and your portfolio projects. I also had a look at your GitHub page and I am quite impressed with the quality of your work.
>
> Your medium articles also captured my attention.
>
> Could you please forward me your CV so that I can present you for a potential job at one of my clients. It is for a six month contract and I feel that your experience and skills match the job specifications perfectly.
>
> Thank you so much for your time.
> 
> Kind regards,
> 
> Charlotte Regression
> 
> Senior HR Manager, Real Analytics Resources

Running this message through our AWS Comprehend service produces [these](student_solution_files/Example_AWS_Comprehend_Key_Phrases_Output.txt) key phrases and [this](student_solution_files/Example_AWS_Comprehend_Sentiment_Output.txt) sentiment classification output.

From this output it can be seen that AWS Comprehend picked up that the message has a positive sentiment, and that it contains key phrases such as: 'your portfolio projects', 'your GitHub page', 'Your medium articles', 'a potential job', etc. We can therefore set up some logic along with our helper functions to build out our responses.

To define this logic, we start by providing some sample `string` replies that we can add to our email response if certain supplied words match the words in our AWS Comprehend key phrases dictionary. These sample replies can take the following form:

**1. Sample reply if a CV related word is matched**:

```
CV_Text = 'I see that you mentioned my C.V in your message. I am happy to forward you my C.V in response. If you have any other questions or C.V related queries please do get in touch. '
```

**2. Sample reply if a portfolio project related word is matched**

```
Project_Text = 'The projects I listed on my site only include those not running in production. I have several other projects that might interest you.'
```
**3. Sample reply if a Medium article related word is matched**

```    
Article_Text = 'In your message you mentioned my blog posts and data science articles. I have several other articles published in academic journals. Please do let me know if you are interested - I am happy to forward them to you.'
```
**4. Sample reply if the detected sentiment is negative**  

```
Negative_Text = 'I see that you are unhappy in your response. Can we please set up a session to discuss why you are not happy, be it with the website, my personal projects or anything else. \n\nLooking forward to our discussion. \n\nKind Regards, \n\nMy Name'
```

**5. Sample reply if the detected sentiment is neutral** 

```
Neutral_Text = 'Thank you for your email. Let me know if you need any additional information.\n\nKind Regards, \n\nMyName'
```

**6. Sample farewell reply** 
    
```    
Farewell_Text = 'Again, Thank you for your email.\n\nIf there is anything else I can assist you with please let me know and I will set up a meeting for us to meet in person.\n\nKind Regards, \n\nMyName'
```    
    
With sample replies now defined, we can use conditional logic and string manipulation techniques to *build* up a response.

For instance, we define a `Phrase_Matcher_Project` variable that makes use of the `key_phrase_finder` function and passes a list of words that might be project related, for example `['github', 'git', 'Git', 'GitHub', 'projects', 'portfolio', 'Portfolio']`. 

If any of these words now match the extracted keywords in the Comprehend dictionary, the value of the `Phrase_Matcher_Project` variable will be `True`. We can then use conditional logic and string manipulation to add a greeting, the project text and the farewell text to an `email_reply` variable. In the example we've presented, this `email_reply` variable will take on the following form:

> Good day Charlotte Regression,
>
> The projects I listed on my site only include those not running in production. I have several other projects that might interest you.
> 
> Again, thank you for your email.
>
> If there is anything else I can assist you with please let me know and I will set up a meeting for us to meet in person.
>
> Kind Regards, 
>
> MyName

Remember that in the example additional keywords were present such as: `CV`, `projects`, `articles`, etc. We could therefore set up a CV, Project and Article phrase checker and use the same logic as discussed above to generate an intelligent automated response. The `email_response` variable would then take on the following value for the full example:

> Good day Charlotte Regression,
> 
> I see that you mentioned my C.V in your message. I am happy to forward you my C.V in response. If you have any other questions or C.V related queries please do get in touch. 
> 
> In your message you mentioned my blog posts and data science articles. I have several other articles published in academic journals. Please do let me know if you are interested - I am happy to forward them to you
> 
> The projects I listed on my site only include those not running in production. I have several other projects that might interest you.
> 
> Again, thank you for your email.
>
> If there is anything else I can assist you with please let me know and I will set up a meeting for us to meet in person.
>
> Kind Regards, 
>
> MyName
   
| :information_source: NOTE :information_source:                                                                                                         |
| :--------------------                                                                                                                                  |
| The boilerplate lambda function for the full solution can be found [here](student_solution_files/aggregated_lambda_function.py) |