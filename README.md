WXC Social API
<b>WXC Social</b> is a robust, secure, and scalable RESTful backend service designed to power a modern social media application. Built with <b>Python</b> and <b>Flask</b>, the API provides a complete set of endpoints for core social networking features, including user authentication, content management, social interactions, and a unique currency exchange module.

The architecture emphasizes a clean separation of concerns, ensuring the application is maintainable, testable, and ready for deployment in a cloud-native environment like <b>AWS ECS</b>.

<hr>

<details>
<summary><b>Key Features</b></summary>
<ul>
<li><b>Secure Authentication System:</b>
<ul>
<li>JWT-based sessions with <code>HttpOnly</code> cookies and CSRF protection.</li>
<li>Secure password handling using the Argon2 algorithm.</li>
<li>Complete user lifecycle management: registration, login, logout, and secure password reset via email.</li>
<li>Redis-based token blocklist for immediate session invalidation upon logout.</li>
</ul>
</li>
<li><b>Core Social Features:</b>
<ul>
<li>Full CRUD (Create, Read, Update, Delete) functionality for user posts.</li>
<li>A robust follower system to build a social graph.</li>
<li>A personalized <code>/feeds</code> endpoint that constructs a timeline for the logged-in user.</li>
<li>Content interactions including "likes" and threaded comments with nested replies.</li>
<li>A notification system for social interactions like follows, likes, and comments.</li>
</ul>
</li>
<li><b>Currency Exchange Module:</b>
<ul>
<li>Allows users to create and manage their own exchange with a base currency.</li>
<li>Users can set and update a list of buy/sell rates for various currencies.</li>
<li>The API provides all necessary data for the frontend to perform real-time conversion calculations.</li>
</ul>
</li>
<li><b>Production-Ready Architecture:</b>
<ul>
<li>Designed with a clean, three-tier architecture (API, Service, Data layers).</li>
<li>Containerized with Docker using a minimal and secure Alpine base image.</li>
<li>Optimized for deployment on AWS ECS with RDS for PostgreSQL and ElastiCache for Redis.</li>
<li>Comprehensive test suite built with Pytest for high code coverage and reliability.</li>
</ul>
</li>
</ul>
</details>

<hr>

<details>
<summary><b>Architecture Overview</b></summary>

The API is built on a modern, service-oriented architecture designed for clarity and scalability. The codebase is organized into three distinct layers:

API (Resource) Layer: Built with <b>Flask-Restful</b>, this layer handles all incoming HTTP requests and outgoing responses. It is responsible for parsing request data, calling the appropriate service function, and formatting the final JSON response. It contains no business logic.

Service Layer: Plain Python modules that act as the "brain" of the application. This layer contains all business logic, validates input, enforces application rules, and orchestrates data operations across multiple models.

Data (Model) Layer: Built with <b>SQLAlchemy</b> and managed with <b>Alembic</b> migrations, this layer defines the database schema and provides the fundamental, lean methods for direct database interaction (CRUD).

Deployment Flow
<code>
User's Browser  -->  AWS Amplify (Frontend)  -->  API Gateway & ALB  -->  ECS Task (Docker Container)  -->  (RDS PostgreSQL & ElastiCache Redis)
</code>

</details>

<hr>

<details>
<summary><b>Tech Stack</b></summary>
<ul>
<li><b>Programming Language:</b> Python</li>
<li><b>Frameworks and Libraries:</b> Flask, Flask-Restful, Flask-JWT-Extended, SQLAlchemy, Alembic</li>
<li><b>Databases:</b> PostgreSQL with PostGIS for geospatial data</li>
<li><b>Caching / Blocklisting:</b> Redis</li>
<li><b>Testing:</b> Pytest, unittest.mock, fakeredis</li>
<li><b>Cloud Platforms:</b> Docker, AWS ECS</li>
</ul>
</details>

<hr>

<details>
<summary><b>API Endpoints</b></summary>

<table>
<thead>
<tr>
<th>Endpoint</th>
<th>Method</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>/register</code></td>
<td><code>POST</code></td>
<td>Create a new user account.</td>
</tr>
<tr>
<td><code>/login</code></td>
<td><code>POST</code></td>
<td>Authenticate a user and receive JWTs.</td>
</tr>
<tr>
<td><code>/logout</code></td>
<td><code>POST</code></td>
<td>Log out a user and blocklist their token.</td>
</tr>
<tr>
<td><code>/refresh-token</code></td>
<td><code>POST</code></td>
<td>Get a new access token using a refresh token.</td>
</tr>
<tr>
<td><code>/request-password-reset</code></td>
<td><code>POST</code></td>
<td>Send a password reset link to the user's email.</td>
</tr>
<tr>
<td><code>/reset-password/&lt;token&gt;</code></td>
<td><code>POST</code></td>
<td>Set a new password using a valid reset token.</td>
</tr>
<tr>
<td><code>/auth-check</code></td>
<td><code>GET</code></td>
<td>Verify the current user's authentication status.</td>
</tr>
<tr>
<td><code>/settings</code></td>
<td><code>GET/PUT/DELETE</code></td>
<td>Manage the current user's account settings.</td>
</tr>
<tr>
<td><code>/feeds</code></td>
<td><code>GET</code></td>
<td>Fetch the personalized post feed for the user.</td>
</tr>
<tr>
<td><code>/posts/&lt;uuid&gt;</code></td>
<td><code>GET/PUT/DELETE</code></td>
<td>Fetch, update, or delete a single post.</td>
</tr>
<tr>
<td><code>/users/&lt;username&gt;</code></td>
<td><code>GET</code></td>
<td>Fetch a user's public profile.</td>
</tr>
<tr>
<td><code>/users/&lt;username&gt;/posts</code></td>
<td><code>GET</code></td>
<td>Fetch a paginated list of a user's posts.</td>
</tr>
<tr>
<td><code>/users/&lt;username&gt;/follow</code></td>
<td><code>POST/DELETE</code></td>
<td>Follow or unfollow a user.</td>
</tr>
<tr>
<td><code>/posts/&lt;uuid&gt;/like</code></td>
<td><code>POST/DELETE</code></td>
<td>Like or unlike a post.</td>
</tr>
<tr>
<td><code>/posts/&lt;uuid&gt;/comments</code></td>
<td><code>POST</code></td>
<td>Add a new comment to a post.</td>
</tr>
<tr>
<td><code>/comments/&lt;uuid&gt;</code></td>
<td><code>PUT/DELETE</code></td>
<td>Update or delete a single comment.</td>
</tr>
<tr>
<td><code>/comments/&lt;uuid&gt;/like</code></td>
<td><code>POST/DELETE</code></td>
<td>Like or unlike a comment.</td>
</tr>
<tr>
<td><code>/notifications</code></td>
<td><code>GET</code></td>
<td>Fetch the user's notifications.</td>
</tr>
<tr>
<td><code>/exchange</code></td>
<td><code>GET/PUT</code></td>
<td>Fetch or update the user's currency exchange data.</td>
</tr>
</tbody>
</table>

</details>

<hr>

<details>
<summary><b>Setup and Installation</b></summary>

Clone the repository:
<code>git clone <your-repo-url></code>
<code>cd <your-repo-name></code>

Create and activate a virtual environment:
<code>python -m venv venv</code>
<code>source venv/bin/activate</code>  <!-- On Windows, use venv\Scripts\activate -->

Install dependencies:
<code>pip install -r requirements.txt</code>

Set up environment variables:

<ul>
<li>Copy the <code>.env.example</code> file to a new file named <code>.env</code>.</li>
<li>Fill in the necessary values for: <code>DATABASE_URL</code>, <code>SECRET_KEY</code>, <code>JWT_SECRET_KEY</code>, <code>MAIL_SERVER</code>, <code>MAIL_PORT</code>, <code>MAIL_USERNAME</code>, <code>MAIL_PASSWORD</code>, and <code>CLIENT_DOMAIN</code>.</li>
</ul>

Run database migrations:
<code>flask db upgrade</code>

Run the application:
<code>flask run</code>

</details>

<hr>

<details>
<summary><b>Running Tests</b></summary>

To run the complete test suite, use the following command from the project's root directory:

<code>pytest</code>

</details>
