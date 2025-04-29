# Choogle Project

A web application, likely a chatbot or API service, built with Flask and integrating AI capabilities from both OpenAI and Google Generative AI. It appears designed to potentially interact with the LINE messaging platform.

![Choogle Demo Screenshot](https://raw.githubusercontent.com/johnnyhng/choogle/main/choogle.jpeg)

## Core Technologies

*   **Web Framework:** [Flask](https://flask.palletsprojects.com/)
*   **AI Services:**
    *   [OpenAI](https://openai.com/) (via `openai` library)
    *   Google Generative AI (via `google-genai` library)
*   **Messaging Platform Integration:** LINE (via `line-bot-sdk`)
*   **Configuration:**
    *   Environment variables, typically managed via `app.yaml` for deployment (e.g., Google App Engine).
    *   `.env` file for local development (using `python-dotenv`).
*   **Deployment:** WSGI server like Gunicorn, often configured via `app.yaml` entrypoint.

## Features (Inferred)

*   Provides a web endpoint using Flask.
*   Handles interactions or requests, potentially from LINE webhooks.
*   Leverages AI models from OpenAI for tasks like text generation, analysis, etc.
*   Leverages AI models from Google (e.g., Gemini) for similar or complementary AI tasks.
*   Securely manages API keys and other configuration using environment variables.

## Prerequisites

*   Python (e.g., 3.8+ recommended)
*   `pip` (Python package installer)
*   Git (for cloning the repository)
*   Access Keys/Credentials:
    *   LINE Channel Access Token and Channel Secret (if using the LINE bot features)
    *   OpenAI API Key
    *   Google Generative AI API Key
*   (Optional, for deployment) Google Cloud SDK or similar platform CLI tool if using `app.yaml` for deployment.

## Setup and Installation

1.  **Clone the repository:**
    ```bash
    git clone <your-repository-url>
    cd choogle
    ```

2.  **Create and activate a virtual environment (recommended):**
    ```bash
    python -m venv venv
    # On Windows
    # venv\Scripts\activate
    # On macOS/Linux
    source venv/bin/activate
    ```

3.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Configure Environment Variables:**

    *   **For Local Development (using `.env`):**
        Create a `.env` file in the root directory (`/home/hhs/workspace/choogle/`). If your application uses the `python-dotenv` library (often included indirectly or directly), it will load these variables automatically when running locally.

        **.env example:**
        ```dotenv
        # Flask specific (optional, depends on your app structure)
        FLASK_APP=your_app_module.py # e.g., main:app or app:app
        FLASK_ENV=development

        # LINE Bot SDK
        LINE_CHANNEL_ACCESS_TOKEN="YOUR_LOCAL_LINE_CHANNEL_ACCESS_TOKEN"
        LINE_CHANNEL_SECRET="YOUR_LOCAL_LINE_CHANNEL_SECRET"

        # OpenAI
        OPENAI_API_KEY="YOUR_LOCAL_OPENAI_API_KEY"

        # Google Generative AI
        GOOGLE_API_KEY="YOUR_LOCAL_GOOGLE_API_KEY"

        # Add any other environment variables your application needs
        ```
        *   **Important:** Add `.env` to your `.gitignore` file to avoid committing secrets.

    *   **For Deployment (using `app.yaml`):**
        Create or modify the `app.yaml` file in the root directory. This file is commonly used by platforms like Google App Engine to configure the runtime environment, scaling, and environment variables.

        **app.yaml template:**
        ```yaml
        runtime: python39 # Specify your desired Python runtime (e.g., python38, python310, python311)
        entrypoint: gunicorn -b :$PORT your_wsgi_app:app # Adjust 'your_wsgi_app:app' to your Flask app instance (e.g., main:app)

        env_variables:
          # --- Sensitive Keys - Consider using Secret Manager ---
          # LINE Bot SDK
          LINE_CHANNEL_ACCESS_TOKEN: "YOUR_DEPLOYMENT_LINE_CHANNEL_ACCESS_TOKEN"
          LINE_CHANNEL_SECRET: "YOUR_DEPLOYMENT_LINE_CHANNEL_SECRET"

          # OpenAI
          OPENAI_API_KEY: "YOUR_DEPLOYMENT_OPENAI_API_KEY"

          # Google Generative AI
          GOOGLE_API_KEY: "YOUR_DEPLOYMENT_GOOGLE_API_KEY"

          # --- Other Configuration ---
          # Example: Set Flask environment for production
          # FLASK_ENV: "production"

          # Add any other environment variables your application needs for deployment
        ```
        *   **Security Warning:** Avoid committing `app.yaml` directly to your repository if it contains sensitive API keys. For production environments, it's highly recommended to use a secret management service (like Google Secret Manager, AWS Secrets Manager, Azure Key Vault) and reference the secrets in `app.yaml` rather than hardcoding them. Consult your deployment platform's documentation for best practices.
        *   Adjust `runtime` and `entrypoint` according to your specific application structure and deployment platform requirements. The `entrypoint` should match how you would run your application using a production WSGI server like Gunicorn. `$PORT` is typically injected by the platform.

## Running the Application

*   **Development Server (using Flask's built-in server & `.env`):**
    Ensure `python-dotenv` is installed (`pip install python-dotenv`) if your app relies on it to load `.env`.
    ```bash
    # FLASK_APP might be loaded from .env or needs export
    # export FLASK_APP=app.py # (Replace app.py if needed)
    flask run
    ```
    The application should now be running locally (e.g., `http://127.0.0.1:5000/`), using variables defined in your `.env` file.

*   **Production Server (using Gunicorn locally):**
    This command runs Gunicorn directly, often using variables from the shell environment or potentially loaded via `dotenv` if configured in your app's entry point.
    ```bash
    # Replace 'your_wsgi_app:app' with the actual path to your Flask app instance
    # Example: If your Flask app object 'app' is in 'main.py', use 'main:app'
    # Variables might need to be exported manually if not using .env loading in app
    # export LINE_CHANNEL_ACCESS_TOKEN="YOUR_..."
    gunicorn --workers 4 --bind 0.0.0.0:8080 your_wsgi_app:app
    ```

*   **Deployment (using `app.yaml`):**
    When deploying to a platform like Google App Engine, you typically use the platform's CLI tool (e.g., `gcloud app deploy`). The platform reads `app.yaml`, sets up the environment (including `env_variables`), and runs the specified `entrypoint`. Environment variables defined in `app.yaml` will take precedence during deployment.
