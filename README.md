WXC Social API
<b>WXC Social</b> is a robust, secure, and scalable RESTful backend service designed to power a modern social media application. Built with <b>Python</b> and <b>Flask</b>, the API provides a complete set of endpoints for core social networking features, including user authentication, content management, social interactions, and a unique currency exchange module.

The architecture emphasizes a clean separation of concerns, ensuring the application is maintainable, testable, and ready for deployment in a cloud-native environment like <b>AWS ECS</b>.

<hr>

<details>
<summary><b>Key Features</b></summary>

<b>Secure Authentication System:</b>

JWT-based sessions with HttpOnly cookies and CSRF protection.

Secure password handling using the Argon2 algorithm.

Complete user lifecycle management: registration, login, logout, and secure password reset via email.

Redis-based token blocklist for immediate session invalidation upon logout.

<b>Core Social Features:</b>

Full CRUD (Create, Read, Update, Delete) functionality for user posts.

A robust follower system to build a social graph.

A personalized /feeds endpoint that constructs a timeline for the logged-in user.

Content interactions including "likes" and threaded comments with nested replies.

A notification system for social interactions like follows, likes, and comments.

<b>Currency Exchange Module:</b>

Allows users to create and manage their own exchange with a base currency.

Users can set and update a list of buy/sell rates for various currencies.

The API provides all necessary data for the frontend to perform real-time conversion calculations.

<b>Production-Ready Architecture:</b>

Designed with a clean, three-tier architecture (API, Service, Data layers).

Containerized with Docker using a minimal and secure Alpine base image.

Optimized for deployment on AWS ECS with RDS for PostgreSQL and ElastiCache for Redis.

Comprehensive test suite built with Pytest for high code coverage and reliability.

</details>

<hr>

<details>
<summary><b>Architecture Overview</b></summary>

The API is built on a modern, service-oriented architecture designed for clarity and scalability. The codebase is organized into three distinct layers:

<b>API (Resource) Layer:</b> Built with <b>Flask-Restful</b>, this layer handles all incoming HTTP requests and outgoing responses. It is responsible for parsing request data, calling the appropriate service function, and formatting the final JSON response. It contains no business logic.

<b>Service Layer:</b> Plain Python modules that act as the "brain" of the application. This layer contains all business logic, validates input, enforces application rules, and orchestrates data operations across multiple models.

<b>Data (Model) Layer:</b> Built with <b>SQLAlchemy</b> and managed with <b>Alembic</b> migrations, this layer defines the database schema and provides the fundamental, lean methods for direct database interaction (CRUD).

Deployment Flow
User's Browser  -->  AWS Amplify (Frontend)  -->  API Gateway & ALB  -->  ECS Task (Docker Container)  -->  (RDS PostgreSQL & ElastiCache Redis)

</details>

<hr>

<details>
<summary><b>Tech Stack</b></summary>

Programming Language: Python

Frameworks and Libraries: Flask, Flask-Restful, Flask-JWT-Extended, SQLAlchemy, Alembic

Databases: PostgreSQL with PostGIS for geospatial data

Caching / Blocklisting: Redis

Testing: Pytest, unittest.mock, fakeredis

Cloud Platforms: Docker, AWS ECS

</details>

<hr>

<details>
<summary><b>API Endpoints</b></summary>

Endpoint

Method

Description

/register

POST

Create a new user account.

/login

POST

Authenticate a user and receive JWTs.

/logout

POST

Log out a user and blocklist their token.

/refresh-token

POST

Get a new access token using a refresh token.

/request-password-reset

POST

Send a password reset link to the user's email.

/reset-password/<token>

POST

Set a new password using a valid reset token.

/auth-check

GET

Verify the current user's authentication status.

/settings

GET/PUT/DELETE

Manage the current user's account settings.

/feeds

GET

Fetch the personalized post feed for the user.

/posts/<uuid>

GET/PUT/DELETE

Fetch, update, or delete a single post.

/users/<username>

GET

Fetch a user's public profile.

/users/<username>/posts

GET

Fetch a paginated list of a user's posts.

/users/<username>/follow

POST/DELETE

Follow or unfollow a user.

/posts/<uuid>/like

POST/DELETE

Like or unlike a post.

/posts/<uuid>/comments

POST

Add a new comment to a post.

/comments/<uuid>

PUT/DELETE

Update or delete a single comment.

/comments/<uuid>/like

POST/DELETE

Like or unlike a comment.

/notifications

GET

Fetch the user's notifications.

/exchange

GET/PUT

Fetch or update the user's currency exchange data.

</details>

<hr>

<details>
<summary><b>Setup and Installation</b></summary>

Clone the repository:

git clone <your-repo-url>
cd <your-repo-name>

Create and activate a virtual environment:

python -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`

Install dependencies:

pip install -r requirements.txt

Set up environment variables:

Copy the .env.example file to a new file named .env.

Fill in the necessary values for your DATABASE_URL, SECRET_KEY, JWT_SECRET_KEY, and mail server configuration.

Run database migrations:

flask db upgrade

Run the application:

flask run

</details>

<hr>

<details>
<summary><b>Running Tests</b></summary>

To run the complete test suite, use the following command from the project's root directory:

pytest

</details>
