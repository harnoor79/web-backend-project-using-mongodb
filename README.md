# Backend REST API Starter Guide

**Author:**   
**Project type:** Beginner backend REST API project

This project is a beginner-level REST API built around the concepts covered in freeCodeCamp's **Intro to Backend Web Development** tutorial. The implementation is written independently and uses Node.js, Express, MongoDB, Mongoose, bcrypt, and JSON Web Tokens (JWT).

## Key Features

- Register new users and securely hash their passwords with bcrypt
- Authenticate users through login and JWT generation
- Retrieve the profile of the currently authenticated user
- Provide a logout confirmation response
- Perform create, read, update, and delete (CRUD) operations on posts
- Restrict post editing and deletion to the user who created the post
- Connect to MongoDB using values stored in environment variables
- Organize the code into configuration, model, controller, middleware, and route folders

## Prerequisites

Before starting, make sure you have installed or created the following:

1. **Node.js LTS** — https://nodejs.org/
2. **MongoDB Atlas account** — https://www.mongodb.com/atlas/database (a locally installed MongoDB server can also be used)
3. **Postman** — https://www.postman.com/downloads/ to send and test API requests
4. **Git** — https://git-scm.com/downloads
5. **GitHub account** — https://github.com/

## Installation and Setup

1. Download the project and extract the files.
2. Open the extracted `backend-api` directory in Visual Studio Code.
3. Launch the integrated terminal from that directory.
4. Install the project packages by running:

   ```bash
   npm install
   ```

5. Duplicate the `.env.example` file and rename the duplicate to `.env`.
6. Edit `.env`: add your MongoDB Atlas connection URI and create a long, random value for `JWT_SECRET`.
7. Run the development server with:

   ```bash
   npm run dev
   ```

8. Open http://localhost:4000 in your browser. A JSON response should appear to indicate that the API is active.

> Keep your `.env` file private and do not commit it to GitHub. It stores database access details and your JWT secret; `.gitignore` is configured to exclude it.

## Configuring MongoDB Atlas

1. Sign up for MongoDB Atlas and create a free cluster.
2. Add a database user with its own username and strong password. These credentials are different from the ones used to sign in to the Atlas website.
3. Open **Network Access** and allow your current IP address. For projects containing real data, do not expose the database to every IP address by using `0.0.0.0/0`.
4. Choose **Connect → Drivers → Node.js**, then copy the connection URI provided.
5. Put the URI in your `.env` file and replace the sample username, password, and cluster details. If the password contains special characters, URL-encode them.
6. Include a database name in the URI, for example `app_database`.

## Available API Routes

Base URL: `http://localhost:4000/api`

| Method | Endpoint | Auth required | Purpose |
|---|---|---:|---|
| POST | `/users/register` | No | Register a new user |
| POST | `/users/login` | No | Authenticate and return a JWT |
| POST | `/users/logout` | Yes | Send logout confirmation; remove the token on the client |
| GET | `/users/me` | Yes | View the authenticated user's profile |
| GET | `/posts` | No | Retrieve the complete list of posts |
| GET | `/posts/:id` | No | Retrieve a post by its ID |
| POST | `/posts` | Yes | Add a new post |
| PATCH | `/posts/:id` | Yes | Edit a post owned by the logged-in user |
| DELETE | `/posts/:id` | Yes | Remove a post owned by the logged-in user |

### Sample Request Data

Register a user (`POST /api/users/register`):

```json
{
  "name": "",
  "email": "user@example.com",
  "password": "change-this-password"
}
```

Log in (`POST /api/users/login`):

```json
{
  "email": "user@example.com",
  "password": "change-this-password"
}
```

Add a post (`POST /api/posts`):

```json
{
  "title": "My first backend post",
  "content": "This post was created through my REST API."
}
```

When calling a route that requires authentication, take the token returned after registration or login and add the following header in Postman:

`Authorization: Bearer YOUR_TOKEN_HERE`

## Testing with Postman

Load `postman/backend-api.postman_collection.json` into Postman using its import option. Register a user or log in first. Copy the JWT from the response and place it in the collection's `token` variable before testing protected routes.

## Directory Layout

```text
backend-api/
├── backend/
│   └── src/
│       ├── config/
│       │   ├── constants.js
│       │   └── database.js
│       ├── controllers/
│       │   ├── post.controller.js
│       │   └── user.controller.js
│       ├── middleware/
│       │   └── auth.middleware.js
│       ├── models/
│       │   ├── post.model.js
│       │   └── user.model.js
│       ├── routes/
│       │   ├── post.route.js
│       │   └── user.route.js
│       ├── app.js
│       └── index.js
├── postman/
│   └── backend-api.postman_collection.json
├── .env.example
├── .gitignore
├── package.json
└── README.md
```

## Uploading the Project to GitHub

First, create a new repository in your GitHub account. You can use `backend-api` as its repository name. This project is not linked to another person's GitHub account. Once the empty repository has been created, open the project directory in your terminal and execute:

```bash
git init
git add .
git status
git commit -m "Initial backend project"
git branch -M main
git remote add origin https://github.com/YOUR-GITHUB-USERNAME/backend-api.git
git push -u origin main
```

Change `YOUR-GITHUB-USERNAME` to your own GitHub username. This is the only account-specific value in the commands. Before committing, review the output of `git status` and confirm that `.env` is not included.

## Important Learning Note

This codebase is intended for practice and learning; it should not be treated as a production-ready authentication system. The logout route is stateless, which means an issued JWT can still be used until it expires unless the client removes it or a server-side token revocation mechanism is implemented.

Before using a system like this in production, consider adding request rate limits, comprehensive input validation, HTTPS, secure token storage, automated tests, application logging, and a suitable token-revocation approach.
