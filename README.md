# Google Cloud Functions Deployment Template

This is a GitHub template repository for automating Google Cloud Functions deployment using GitHub Actions.

## How to Use

1. **Create a New Repository**  
   Use this repository as a template to create a new GitHub repository.

2. **Set Up Environment Variables**  
   Configure the following environment variables (`Settings > Codespaces > Environment Variables` or `Settings > Secrets and variables > Actions > Variables`):

   | Variable Name           | Description                                         | Example Value            |
   |-------------------------|-----------------------------------------------------|--------------------------|
   | `FUNCTION_NAME`         | Base name of the Cloud Function                     | `my_cloud_function`      |
   | `FUNCTION_RUNTIME`      | Runtime environment for the Cloud Function          | `python311`              |
   | `FUNCTION_ENTRY_POINT`  | Entry point function name                           | `main`                   |
   | `FUNCTION_REGION`       | Deployment region                                   | `us-central1`            |
   | `FUNCTION_MAX_INSTANCES`| (Optional) Maximum number of instances to run       | `5`                      |

3. **Set Up GitHub Secrets**  
   Configure the following secrets (`Settings > Secrets and variables > Actions > Secrets`):

   | Secret Name             | Description                                         |
   |-------------------------|-----------------------------------------------------|
   | `SERVICE_ACCOUNT_KEY`   | Base64-encoded Service Account Key JSON             |

4. **Modify Code**  
   Update the `main.py` file with your Cloud Function code and adjust dependencies in `requirements.txt`.

5. **Deploy Automatically**  
   - Push changes to the `main` branch to deploy the production function (`FUNCTION_NAME`).
   - Push changes to the `develop` branch to deploy the development function (`FUNCTION_NAME + "_dev"`).
   - Any other branch will result in the workflow exiting without deployment.

---

## File Structure

The repository includes the following files:

- `.github/workflows/deploy-cloud-function.yml`: GitHub Actions workflow for deployment.
- `main.py`: Example Python code for the Cloud Function.
- `requirements.txt`: Python dependencies for the Cloud Function.

---

## Prerequisites

Before using this template, ensure the following:

1. **Google Cloud Project**  
   - A Google Cloud project with Cloud Functions enabled.

2. **Service Account**  
   - Create a Service Account with the following roles:
     - `Cloud Functions Admin`
     - `IAM Service Account User`
   - Download the Service Account key in JSON format and encode it in Base64 for use in GitHub Secrets.

3. **gcloud CLI**  
   - Install the [Google Cloud SDK](https://cloud.google.com/sdk) locally for testing if needed.

---

## Workflow Behavior

1. **Main Branch Deployment**  
   - When changes are pushed to the `main` branch, the workflow deploys the function using `FUNCTION_NAME`.

2. **Develop Branch Deployment**  
   - When changes are pushed to the `develop` branch, the workflow deploys the function using `FUNCTION_NAME + "_dev"`.

3. **Other Branches**  
   - For any other branch, the workflow exits without deploying.

---

## Example Workflow Steps

1. **Determine Function Name**  
   The workflow dynamically determines the function name based on the branch:
   - `main` → `FUNCTION_NAME`
   - `develop` → `FUNCTION_NAME + "_dev"`
   - Other branches → Exit without deployment.

2. **Authenticate with Google Cloud**  
   The workflow uses the `SERVICE_ACCOUNT_KEY` secret to authenticate with Google Cloud.

3. **Deploy Cloud Function**  
   The Cloud Function is deployed to the specified region, runtime, and entry point.

---

This setup ensures seamless deployment to Google Cloud Functions with branch-based behavior. Adjust the workflow or environment variables as needed for your specific use case.