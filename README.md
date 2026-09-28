# Interview Practice Assistant

A beginner-friendly local web app for practicing placement interview questions.

## What it does

- Lets students create an account, log in, and log out.
- Lets you practice Python, DBMS, and Operating Systems questions at beginner, intermediate, and advanced difficulty.
- Runs a three-question mock interview with feedback after each answer.
- Generates a question for a target job role using an optional pasted job description.
- Generates a practice question from an uploaded text-based PDF resume. The PDF is processed in memory and is not saved.
- Saves submitted answers and difficulty in a local SQLite database, separated by account.
- Provides a personal dashboard with answer totals, topic activity, and recent feedback.
- Uses Gemini for interview feedback and a practice score when `GEMINI_API_KEY` is configured. Without a key or when the service is unavailable, it shows local basic feedback without a score.

Passwords are stored as secure password hashes. The app creates a local `.secret_key` file for signed login sessions; that file is ignored by Git. Set the `SECRET_KEY` environment variable to a long random value when deploying.

POST forms use Flask-WTF CSRF protection. Session cookies are HTTP-only and use SameSite=Lax. Set `COOKIE_SECURE=1` only when the site is served over HTTPS.

To enable Gemini feedback, install requirements and put `GEMINI_API_KEY=your_key` in a local `.env` file in the project root. `.env` is ignored by Git. Never paste the key into `app.py` or commit it to GitHub.

## Run on Windows

1. Install Python 3.10 or newer and Visual Studio Code.
2. Open this folder in VS Code.
3. In VS Code, open **Terminal → New Terminal**.
4. Create and activate a virtual environment:

   ```powershell
   py -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

   If PowerShell blocks activation, use Command Prompt in VS Code and run:

   ```cmd
   .venv\Scripts\activate.bat
   ```

5. Install the project packages:

   ```powershell
   python -m pip install -r requirements.txt
   ```

6. Start the app:

   ```powershell
   python app.py
   ```

7. Open `http://127.0.0.1:5000` in your browser. Keep the terminal open while using the app. Press `Ctrl+C` in the terminal to stop it.

The database file `practice.db` is created automatically the first time the app starts.

## Deployment preparation

- `Procfile` starts the app with Gunicorn on a Linux hosting service.
- `GET /health` returns a small status response for a host health check.
- Configure `GEMINI_API_KEY` and `SECRET_KEY` as host environment variables. Never upload `.env` or `.secret_key`.
- The app uses the local SQLite file by default. Set `DATABASE_URL` to a PostgreSQL connection string on the host to keep hosted accounts and practice history in a managed database.
- Set `COOKIE_SECURE=1` only when the deployed site uses HTTPS.
