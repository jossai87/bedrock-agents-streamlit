
# AWS Setup Guide for Bedrock Agent with Streamlit

## Introduction
This guide details the setup process for an Amazon Bedrock agent on AWS, which includes S3 buckets, a knowledge base, and a Lambda function. The agent is designed for creating and managing company portfolios based on specific criteria. We will use the Streamlit framework for the user interface.

## Prerequisites
- An active AWS Account.
- Familiarity with AWS services like Amazon Bedrock, S3, Lambda.

## Configuration and Setup
### Step 1: Creating S3 Buckets
- **Domain Data Bucket**: Set up an S3 bucket for domain data (.txt, .csv, .pdf). Add the .pdf files located in the s3Docs folder to this S3 bucket.

- **Artifacts Bucket**: Set up another S3 bucket for storing artifacts and the API schema. Add the provided API schema file "WorkingSchema.json" to this s3 bucket:


### Step 2: Knowledge Base Setup in Bedrock Agent
- Navigate to the Amazon Bedrock console, and start creating a knowledge base. Sync the S3 bucket that has the .pdf files from earlier to this knowledge base.

![KB setup](Streamlit_App/images/KB_setup.png)

- Select default OpenSearch Serverless as the vector store.
 
![Vector Store Config](Streamlit_App/images/vector_store_config.png)


### Step 3: Lambda Function Configuration
- Develop a Lambda function (Python 11) for the Bedrock agent's action group. Copy the provided code from the "WorkingLambda.py" file into your Lambda function, then deploy it. Also, be sure to increase the memory on the Lambda function to 512MB, and timeout to 10 seconds: 

![Lambda config](Streamlit_App/images/lambda_config.png)

- Make sure that the Bedrock agent role can invoke the Lambda function.
- Apply a resource policy to the Lambda to grant Bedrock agent access. Here is an example of the resource policy (be sure to use the correct Bedrock agent Source ARN for your Lambda resource policy):  

![Lambda resource policy](Streamlit_App/images/lambda_resource_policy.png)

### Step 4: Bedrock Agent Creation
- Create an agent with instructions on what the agent is used for. For example, use the following: “This Agent is used to create Portfolios of companies based on the number of companies, industry, and portfolio name input. This agent can also search company data.” (when creating the agent, select the Lambda function created prior. Make sure to include the lambda code provided. Also, select the s3 bucket that contains the artifacts, which should include the API schema provided)
 
![Model select](Streamlit_App/images/select_model.png)

### Step 5: Integrating Knowledge Base with Bedrock Agent
- When integrating the KB with the agent, you will need to provide basic instructions on how to handle the knowledge base. For example, use the following: “knowledge base for answering queries to prompts. After every response, provide a citation link. Ask if anything else is needed.”
 
![Knowledge base add](Streamlit_App/images/add_knowledge_base.png)

### Step 6: Create an alias
-Create an alias (new version), and choose a name of your liking. 
 
![Create alias](Streamlit_App/images/create_alias.png)

## Testing the Setup
### Testing the Knowledge Base
- Use the Bedrock console to test the knowledge base after its creation.
- Test Prompts:
  1. "Give me a summary of financial market developments and open market operations in January 2023."
  2. "What is the SEC's view on current economic conditions for September 2023?"
  3. "Can you provide information about inflation or rising prices?"
  4. "What can you tell me about the Staff Review of the Economic & Financial Situation?"

### Testing the Bedrock Agent
- After connecting the agent to the knowledge base, test it using the Bedrock console.

- Tests knowledge base:
    1. "Give me a summary of development in financial market and open market operations in january 2023"
    2. "What is the SEC participants view on current economic conditions and economic outlook for september 2023"
    3. "Can you provide any other important information I should know about inflation, or rising prices?"
    4. "What can you tell me about the Staff Review of the Economic & financial Situation?"

- Test prompts for action groups:
    1. "Create a portfolio with top 3 company profit earners in real estate."
    2. "Create another portfolio of top 3 profit earners in technology."
    3. "Provide more details on these companies."
    4. "Help me create a new investment portfolio of companies."
    5. "Do company research on TechNova Inc."

## Setting Up and Running the Streamlit App
1. **Obtain the Streamlit App ZIP File**: Ensure you have the ZIP file containing the Streamlit app.
2. **Upload to Cloud9**:
   - In your Cloud9 environment, upload the ZIP file.

![Upload file to Cloud9](Streamlit_App/images/upload_file_cloud9.png)

3. **Unzip the File**:
   - Use the command `unzip <filename>.zip` to extract the contents.
4. **Navigate to Streamlit_App Folder**:
   - Change to the directory containing the Streamlit app, which is “/environment/bedrock-agents-streamlit-main/Streamlit_App”
5. **Update Configuration**:
   - Open the `InvokeAgent.py` file.
   - Update the `agentId` and `agentAliasId` variables with the appropriate values, then save it.

![Update Agent ID and alias](Streamlit_App/images/update_agentId_and_alias.png)

6. **Install Streamlit** (if not already installed):
   - Run `pip install streamlit`. Additionally, make sure boto3, and pandas dependencies are installed.
7. **Run the Streamlit App**:
   - Execute the command `streamlit run app.py --server.address=0.0.0.0 --server.port=8080`.
   - Streamlit will start the app, and you can view it by selecting "Preview" within the Cloud9 IDE at the top, then "Preview Running Application"

![Running App ](Streamlit_App/images/running_app.png)


