
# AWS Setup Guide for Bedrock Agent with Streamlit

## Introduction
This guide details the setup process for an Amazon Bedrock agent on AWS, which will include setting up S3 buckets, a knowledge base, an action group, and a Lambda function. The agent is designed for dynamically creating an investment company portfolio based on specific parameters, and providing a Q&A capability to domain data. We will use the Streamlit framework for the user interface.

## Prerequisites
- An active AWS Account.
- Familiarity with AWS services like Amazon Bedrock, S3, Lambda, and Cloud9.

## Configuration and Setup

### Step 1: Creating S3 Buckets
- **Domain Data Bucket**: Create an S3 bucket to store the domain data. For example, call the S3 bucket "knowledgebase-bedrock-agentr-alias". We will use the default settings. After creation, add the .pdf files located [here](https://github.com/jossai87/bedrock-agents-streamlit/tree/main/s3Docs) to the s3 bucket.

![Bucket create 1](Streamlit_App/images/bucket_pic_1.png)

![Bucket create 2](Streamlit_App/images/bucket_pic_2.png)

- **Artifacts Bucket**: Create another S3 bucket to store artifacts. For example, call it "artifacts-bedrock-agent-creator-alias". You will need to download, then add the API schema file to this S3 bucket. This .json file can be found [here](https://github.com/jossai87/bedrock-agents-streamlit/blob/main/WorkingSchema.json).

![Loaded Artifact](Streamlit_App/images/loaded_artifact.png)

### Step 2: Knowledge Base Setup in Bedrock Agent
- Navigate to the Amazon Bedrock console, then create a knowledge base. You can use the default name, or enter in your own. Sync S3 bucket "knowledgebase-bedrock-agentr-alias" to this knowledge base.

![KB setup](Streamlit_App/images/KB_setup.png)

- Select the default option OpenSearch Serverless as the vector store.
 
![Vector Store Config](Streamlit_App/images/vector_store_config.png)


### Step 3: Lambda Function Configuration
- Create a Lambda function (Python 11) for the Bedrock agent's action group. Copy the provided code from the "WorkingLambda.py" file into your Lambda function. After,  select the deploy button.

![Lambda deploy](Streamlit_App/images/lambda_deploy.png)

- Make sure that the IAM role associated with the Bedrock agent can invoke the Lambda function.
- Apply a resource policy to the Lambda to grant Bedrock agent access. Here is an example of the resource policy (be sure to use the correct Bedrock agent Source ARN for your Lambda resource policy):  

![Lambda resource policy](Streamlit_App/images/lambda_resource_policy.png)

### Step 4: Bedrock Agent Creation
- Create an agent with instructions on what the agent is used for. For example, use the following: “This Agent is used to create Portfolios of companies based on the number of companies, industry, and portfolio name input. This agent can also search company data.” (when creating the agent, select the Lambda function created prior. Make sure to include the lambda code provided. Also, select the s3 bucket that contains the artifacts, which should include the API schema provided)
 
![Model select](Streamlit_App/images/select_model.png)

### Step 5: Integrating Knowledge Base with Bedrock Agent
- When integrating the KB with the agent, you will need to provide basic instructions on how to handle the knowledge base. For example, use the following: “knowledge base for answering queries to prompts. After every response, ask if anything else is needed.”
 
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

Optionally, you can review the trace events in the left toggle of the screen.

![Trace events ](Streamlit_App/images/trace_events.png)


