# Lab Submission: Course 10, Module 2 - Authorizing Requests

This repository contains my solution for the "Authorizing Requests" lab. The goal is to implement a complete authorization workflow (session checking) on top of the existing authentication system. The work here follows the patterns and rubric from the Flatiron lesson.

## Features

* **Authorization Guard Clause:** Implemented authorization for the `MemberOnlyIndex` (`GET /members_only_articles`) and `MemberOnlyArticle` (`GET /members_only_articles/<int:id>`) resources.
* **Session Check:** Both routes use `session.get('user_id')` to check for an active session.
* **401 Unauthorized Response:** If no user is authenticated, both routes correctly return a `{'error': 'Unauthorized'}` payload and a `401` status code.
* **Content Filtering:** The `MemberOnlyIndex` resource correctly queries and returns *only* articles where the `is_member_only` attribute is `True`.

## Environment

* **Backend:** Python 3.8.13 (via `pipenv`), Flask, Flask-SQLAlchemy, Flask-Migrate, Flask-RESTful, Marshmallow
* **Frontend:** React (via `npm`)
* **Testing:** `pytest`

## Setup

Please follow these steps to ensure the environment is built correctly. The database commands must be run from within the `server/` directory due to the project's specific import structure.

1.  Clone and enter the project directory:
    ```bash
    git clone https://github.com/walbeck85/flask-authorizing-requests-lab
    cd flask-authorizing-requests-lab
    ```

2.  Install backend and frontend dependencies:
    ```bash
    pipenv install
    npm install --prefix client
    ```

3.  Activate the virtual environment:
    ```bash
    pipenv shell
    ```

4.  Build and seed the database:
    ```bash
    # From the project root, navigate into the server directory
    cd server
    
    # Set the Flask app context for this directory
    export FLASK_APP=app.py
    
    # Run migrations and seed the database
    flask db upgrade
    python seed.py
    
    # Return to the project root
    cd ..
    ```

## How to Run the Application

The application requires two terminals to run the backend and frontend concurrently.

**Terminal 1: Run the Backend (Flask)**

```bash
# Activate the virtual environment if not already active
pipenv shell

# Navigate into the server directory
cd server

# Set the Flask app context
export FLASK_APP=app.py

# Run the application
python app.py
````

**Terminal 2: Run the Frontend (React)**

```bash
# From the project root
npm start --prefix client
```

The React app will open at `http://localhost:3000` and is proxied to the Flask server at `http://localhost:5555`.

## Tests

Run the test suite from the project **root** with the virtual environment active.

```bash
pytest -x -v
```

## Rubric Alignment

  * **Authorization: Member Articles Index:** The `MemberOnlyIndex` resource checks for `session.get('user_id')` and returns `401` if it is missing.
  * **Member Articles Index (Content):** If authorized, the route queries `Article.query.filter_by(is_member_only=True).all()` to ensure only member articles are returned.
  * **Authorization: Member Articles Show:** The `MemberOnlyArticle` resource also checks for `session.get('user_id')` and returns `401` if it is missing.

## Branch and PR Workflow

All logic was completed on the `feature/authorization` branch and merged into `main` via a pull request. This documentation update was prepared on the `feature/update-readme` branch.

## Instructor Checklist

To verify the submission:

1.  `pipenv install && npm install --prefix client`
2.  `pipenv shell`
3.  `cd server`
4.  `export FLASK_APP=app.py`
5.  `flask db upgrade && python seed.py`
6.  `cd ..`
7.  `pytest -x -v` (from project root) to verify all 3 tests pass.

<!-- end list -->