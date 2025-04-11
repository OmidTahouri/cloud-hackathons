# Apigee and Application Integration 101

## Introduction

Apigee is Google Cloud's API management platform. It can secure and protect APIs hosted not just on GCP, but on other clouds and on-prem backends. Application Integration is Google Cloud's native integration platform. The Apigee and Application Integration 101 hack will introduce you to the capabilities of Apigee as an API Management and Application Integration as an Integration platform. We would do this by implementing a real usecase of purchase order processing.

## Learning Objectives

In this hack you would learn to create API proxies in Apigee and Integrations in Application Integration. You would do this by creating an automated workflow to automatically process purchase orders:
1. A user working on a CRM, submitts a purchase order for processing, the order has to be processed by a sales agent.
2. Apigee as the API management solution protecting the backend systems, validates the API key and ensures that the purchase order has been sent by a trusted source, in this case, the CRM system.
3. Apigee, after validating the API key, sends the purchase order to Application Integration. If the sales order is less than $1000, no manager approval is needed. Application Integration creates a JIRA ticket for the sales agent to process the order. If the order is more than $1000, an approval needs to be provided by the sales manager.
4. After the JIRA ticket is created, the sales agent completes the JIRA ticket after processing the order.

## Challenges

- Challenge 1: Create an Application Integration flow
  - Create an Application Integration flow with a JIRA connector
- Challenge 2: Import the provided Apigee proxy and connect it to Application integration
  - Create an Apigee proxy which routes a request to the Application Integration backend
- Challenge 3: Configure security on the Apigee proxy
  - Add a policy in the Apigee proxy to c
- Challenge 4: Test the end-to-end flow
  - Test the complete flow and ensure that thhat the "happy-path" works.

## Prerequisites

- Your own GCP project with Owner IAM role.

## Contributors

- Omid Tahouri
- Pulkit Mathur

## Challenge 1: Create an Application Integration flow

### Pre-requisites

- Confirm that Application Integration is provisioned and red to use within your GCP project.
- Confirm that an existing connector called "pubsub-connector" exists
- Create an account on JIRA (https://id.atlassian.com/signup) and create an JIRA API token

### Introduction

Application Integrations enables you to develop workflow in a no-code/low-code way, and provides native connectivity to more than 150 applications, both Google Applications like Bigquery, Google Cloud Storage etc., and third-party applications like Salesforce, Workday, JIRA etc. 

### Description

Create an integration in Application Integration:
- The integration should be triggered via an API call
- Configure the JIRA connector. You would use the JIRA username/password and JIRA API Token created previoulsy to configure the JIRA connector. Create 
- The issue in JIRA should be created with a default description, something like "Process New Sales Order"

### Success Criteria

- Integration successfully tested from the console
- New JIRA issue created and visible in JIRA

### Advanced Challenge:

- Call the API with a curl command from cloud shell

### Learning Resources

- Creating an integration (https://cloud.google.com/application-integration/docs/create-integrations)
- Creating a JIRA connector (https://cloud.google.com/integration-connectors/docs/connectors/jiracloud/configure)
- Testing an integration (https://cloud.google.com/application-integration/docs/test-publish-integrations)
- Blog on creation of the JIRA connector (https://www.googlecloudcommunity.com/gc/Cloud-Product-Articles/Application-Integration-JIRA-Connection-amp-JIRA-Trigger/ta-p/843096)

## Challenge 2: Import the provided Apigee proxy and connect it to Application integration

### Pre-requisites

- Completion of Challenge 1.

### Introduction

Apigee is the answer to your API management needs. Organizations typically expose their backend services through APIs, and frontend applications consume them. Apigee provides complete security and management of these APIs across an organization. In this challenge, you would connect the Application Integration flow you creatd in the previous challenge an Apigee proxy, establishing the Applicatiuon Integration flow as the backend of the Apigee proxy.

### Description

Create an API Proxy in Apigee:
- Import the provided proxy bundle into Apigee
- Change the endpoint to the Integration in Application Integration you created in the last challenge.

### Success Criteria

- Apigee proxy successfully imported and deployed.
- The target endpoint points to the integration.

### Learning Resources

- Uploading an API proxy (https://cloud.google.com/apigee/docs/api-platform/fundamentals/download-api-proxies)
- Setting an Application Integration integration as the target for an API proxy (https://cloud.google.com/apigee/docs/api-platform/integration/getting-started-apigee-target-endpoint)


## Challenge 3: Configure security on the Apigee proxy

### Pre-requisites

- Completion of Challenge 1 and 2.

### Introduction

Apigee provides various ways to secure your services, some of the most common ways are securing them with an API Key or OAuth protection. In addition, it is possible to transform/enrich data in Apigee through one of the various policies available. In this challenge, we are going to secure our API Proxy with an API-Key-Authentication so that only callers with a valid API key can call the proxy.

### Description

Add API Key security to the Apigee proxy
- Create the "VerifyAPIKey" policy
- Attach it to your Proxy so that every incoming request's API key is validated. You can choose the location of the API key in the incoming request (mostly the API key is passed in a header)

### Success Criteria

- The "VerifyAPIKey" is successfully attached to the proxy
- The proxy is deployed

### Learning Resources

- API Key policy documentation (https://cloud.google.com/apigee/docs/api-platform/reference/policies/verify-api-key-policy)
- Attaching a policy to a proxy (https://cloud.google.com/apigee/docs/api-platform/develop/attaching-and-configuring-policies-management-ui)

## Challenge 4: Test the end-to-end flow

### Pre-requisites

- Completion of Challenge 1, 2 and 3.

### Introduction

In this challenge, we are going to test the end-to-end flow. We will test all functionalities like the API key verification, Apige being able to call the Application Integration flow, and the creation of a JIRA issue by Application Integration.

### Description

Test the end-to-end flow:
- Determine the target URL of the Apigee proxy. 
- Send a request to the Apigee proxy using Postman or curl.

### Success Criteria

- An issue is created in JIRA

### Learning Resources

- Test an Apigee proxy (https://cloud.google.com/apigee/docs/api-platform/get-started/test-proxy)