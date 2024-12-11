# Pact Workflow with Swagger Mock Validator

## Overview
Pact is a consumer-driven contract testing tool used for ensuring that services (consumers and providers) can communicate with each other. Swagger Mock Validator, on the other hand, validates contracts against an OpenAPI/Swagger specification. Combining these two tools provides a robust mechanism to ensure API interactions are consistent with both the consumer’s expectations and the provider’s API documentation.

This document outlines the workflow for implementing Pact with Swagger Mock Validator.

---

## Prerequisites
1. **Pact CLI**: Install Pact CLI to create and publish contracts.
2. **Swagger/OpenAPI Specification**: Ensure your API is well-documented using Swagger or OpenAPI specifications.
3. **Swagger Mock Validator**: Install the Swagger Mock Validator tool to validate Pact contracts against the API documentation.
4. **CI/CD Pipeline**: Ensure your CI/CD pipeline supports script execution.

---

## Workflow

### 1. **Contract Creation**
#### Consumer Side
1. Identify the interactions between your consumer and the provider.
2. Write tests in your consumer application to generate Pact files:
   - Use Pact libraries for your language (e.g., Pact JS, Pact Ruby).
   - Define the expected request and response for each interaction.
3. Run the tests to create a Pact contract file (e.g., `consumer-provider.json`).


### 2. **Contract Validation Against API Documentation**
#### Steps:
1. Install the Swagger Mock Validator:
   ```bash
   npm install -g swagger-mock-validator
   ```
2. Validate the Pact file against the Swagger/OpenAPI specification:
   ```bash
   swagger-mock-validator path/to/swagger.json path/to/pact.json
   ```
3. Analyze the validation output:
   - **Success**: Pact file aligns with the API specification.
   - **Failure**: Errors are reported detailing mismatches (e.g., missing endpoints, schema mismatches).

### 3. **Provider Verification**
#### Steps:
1. Publish the Pact file to a Pact broker (optional but recommended).
   ```bash
   pact-broker publish path/to/pacts --consumer-app-version <version> --broker-base-url <http://pact-broker>
   ```
2. Set up the provider to verify the Pact file:
   - Use the appropriate Pact library for the provider's language.
   - Fetch the Pact file from the broker or local directory.
   - Write tests to verify the provider meets the consumer's expectations.
3. Run the provider verification tests.



### 4. **Integration in CI/CD**
#### Consumer Workflow:
- Run contract tests to generate the Pact file.
- Validate the Pact file against the Swagger specification.
- Upload the validated Pact file to the Pact broker.

#### Provider Workflow:
- Retrieve Pact files from the Pact broker.
- Run provider verification tests to ensure compliance with the Pact file.
- Fail the pipeline if verification fails.

---

## Things to be taken care of 
1. **Keep API Documentation Updated**: Always update the Swagger/OpenAPI specification when API changes are made.
2. **Automate Validation**: Include Pact validation and Swagger Mock Validator in the CI/CD pipeline to catch issues early.
3. **Use Pact Broker**: Centralize contract storage for better collaboration and versioning.
4. **Granular Contracts**: Create separate contracts for different use cases to keep them manageable and focused.

---

## Tools and Resources
1. **Pact Documentation**: [https://docs.pact.io](https://docs.pact.io)
2. **Swagger Mock Validator**: [https://github.com/atlassian/swagger-mock-validator](https://github.com/atlassian/swagger-mock-validator)
3. **OpenAPI Specification**: [https://swagger.io/specification/](https://swagger.io/specification/)
4. **Pact Broker**: [https://github.com/pact-foundation/pact_broker](https://github.com/pact-foundation/pact_broker)

---

## Conclusion
Integrating Pact with Swagger Mock Validator provides a robust framework for API contract testing and validation. It ensures that consumer-provider interactions are reliable and conform to documented API standards, fostering better collaboration and reducing integration issues.

