# Choogle Project

A web application, likely a chatbot or API service, built with Flask and integrating AI capabilities from both OpenAI and Google Generative AI. It appears designed to potentially interact with the LINE messaging platform.

## Core Technologies

*   **Web Framework:** [Flask](https://flask.palletsprojects.com/)
*   **AI Services:**
    *   [OpenAI](https://openai.com/) (via `openai` library)
    *   Google Generative AI (via `google-genai` library)
*   **Messaging Platform Integration:** LINE (via `line-bot-sdk`)
*   **Configuration:** Environment variables (via `app.yaml`)
*   **Deployment:** WSGI server like Gunicorn

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
    Create a `.env` file in the root directory (`/home/hhs/workspace/choogle/`) and add your credentials. The `dotenv` library will load these automatically.

    **.env example:**
    ```dotenv
    # Flask specific (optional, depends on your app structure)
    # FLASK_APP=your_app_module.py
    # FLASK_ENV=development

    # LINE Bot SDK
    LINE_CHANNEL_ACCESS_TOKEN="YOUR_LINE_CHANNEL_ACCESS_TOKEN"
    LINE_CHANNEL_SECRET="YOUR_LINE_CHANNEL_SECRET"

    # OpenAI
    OPENAI_API_KEY="YOUR_OPENAI_API_KEY"

    # Google Generative AI
    GOOGLE_API_KEY="YOUR_GOOGLE_API_KEY"

    # Add any other environment variables your application needs
    ```
    *   **Important:** Add `.env` to your `.gitignore` file to avoid committing secrets.

## Running the Application

*   **Development Server (using Flask's built-in server):**
    ```bash
    # Make sure FLASK_APP is set in .env or exported
    # export FLASK_APP=app.py  (Replace app.py with your main Flask file if different)
    flask run
    ```
    The application should now be running, typically on `http://127.0.0.1:5000/`.

*   **Production Server (using Gunicorn):**
    ```bash
    # Replace 'your_wsgi_app:app' with the actual path to your Flask app instance
    # Example: If your Flask app object 'app' is in 'main.py', use 'main:app'
    gunicorn --workers 4 --bind 0.0.0.0:8080 your_wsgi_app:app
    ```
    Refer to the Gunicorn documentation for more advanced configuration options.

## Project Structure (Example)

While not provided, a typical structure might look like this:


