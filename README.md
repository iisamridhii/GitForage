
# GitForage

### A GitHub-Inspired Code Hosting and Version Control Platform

GitForage is a full-stack code hosting and collaboration platform inspired by GitHub. It combines repository management, user authentication, developer profiles, issue tracking, and a custom command-line version control system.

Built using the MERN stack, GitForage integrates AWS S3 for remote commit storage.

## Features

### 1. User Authentication and Profiles
- User registration and login.
- Password hashing using bcrypt.
- JWT-based authentication.
- User profile retrieval and updating.
- Followed users and starred repository data models.

### 2. Repository Management
- Create and manage repositories.
- Fetch available repositories.
- Retrieve repositories by ID or name.
- Fetch repositories belonging to a particular user.
- Update repository details.
- Delete repositories.
- Toggle repository visibility between public and private.

### 3. Issue Tracking
- Create issues associated with repositories.
- Retrieve all issues.
- Fetch individual issues.
- Update issue details.
- Delete issues.
- Track issue status (Open/Closed).

### 4. Custom Version Control System

GitForage includes a command-line version control implementation supporting:

| Command | Description |
|---|---|
| `init` | Initializes a local repository |
| `add` | Adds files to the staging area |
| `commit` | Creates a commit containing staged files |
| `push` | Uploads commits to AWS S3 |
| `pull` | Retrieves commits from AWS S3 |
| `revert` | Restores files from a specified commit |

The version control implementation uses:
- Local filesystem-based repository storage.
- A staging directory for selected files.
- Separate directories for individual commits.
- UUID-based commit identification.
- JSON files for commit metadata.
- AWS S3 for remote commit storage.

### 5. Additional Integrations
- AWS S3 for remote storage.
- Socket.IO for client-server communication.
- MongoDB for application data persistence.

## Technology Stack

| Category | Technologies |
|---|---|
| Frontend | React.js, Vite |
| UI | Primer CSS, Primer React |
| Backend | Node.js, Express.js |
| Database | MongoDB, Mongoose |
| Authentication | JWT, bcrypt |
| Cloud Storage | AWS S3 |
| Real-time Communication | Socket.IO |
| Routing | React Router |
| HTTP Requests | Axios |
| CLI | Yargs |
| Commit Identification | UUID |

## Project Structure

```text
GitForage/
│
├── backend-main/
│   ├── config/
│   ├── controllers/
│   ├── middleware/
│   ├── models/
│   ├── routes/
│   ├── index.js
│   ├── package.json
│   └── commit.json
│
├── frontend-main/
│   ├── public/
│   ├── src/
│   │   ├── assets/
│   │   ├── components/
│   │   │   ├── auth/
│   │   │   ├── dashboard/
│   │   │   └── user/
│   │   ├── App.jsx
│   │   ├── Routes.jsx
│   │   ├── authContext.jsx
│   │   └── main.jsx
│   │
│   ├── package.json
│   └── index.html
│
└── README.md
```

## Getting Started

### Prerequisites

Make sure you have the following installed:

- Node.js
- npm
- MongoDB database
- AWS account with an S3 bucket

### 1. Clone the Repository

```bash
git clone <YOUR_REPOSITORY_URL>
cd GitForage
```

### 2. Backend Setup

Navigate to the backend directory:

```bash
cd backend-main
```

Install dependencies:

```bash
npm install
```

Create a `.env` file inside `backend-main`:

```env
PORT=3000
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET_KEY=your_jwt_secret
S3_BUCKET=your_s3_bucket_name
```

### 3. AWS S3 Configuration

GitForage uses AWS S3 to store and retrieve commit files.

Configure your AWS credentials using the AWS SDK's supported credentials configuration.

Open:

```text
backend-main/config/aws-config.js
```

Set your S3 bucket name in the configuration:

```javascript
const S3_BUCKET = "your_s3_bucket_name";
```

The current configuration uses the `ap-south-1` AWS region.

Ensure that your AWS credentials have the necessary permissions to access the configured bucket.

### 4. Start the Backend Server

From the `backend-main` directory:

```bash
npm start
```

The backend server starts on port `3000` by default.

### 5. Frontend Setup

Open another terminal and navigate to the frontend directory:

```bash
cd frontend-main
```

Install dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

Open the local URL displayed by Vite in your browser.

**Note:** The current frontend API requests use `http://localhost:3002`, whereas the backend defaults to port `3000`. Configure the backend port or frontend API URLs to match before running the application.

## Custom Version Control CLI

GitForage provides a command-line interface for local version control operations.

Run these commands from the `backend-main` directory.

### Initialize a Repository

```bash
node index.js init
```

Initializes a local repository by creating a `.apnaGit` directory.

### Add Files to Staging

```bash
node index.js add file.txt
```

Copies the specified file into the staging directory.

### Create a Commit

```bash
node index.js commit "Initial commit"
```

Creates a uniquely identified commit containing staged files and commit metadata.

### Push Commits

```bash
node index.js push
```

Uploads locally stored commit files to the configured AWS S3 bucket.

### Pull Commits

```bash
node index.js pull
```

Retrieves commit files stored in AWS S3.

### Revert to a Previous Commit

```bash
node index.js revert <commitID>
```

Restores files from the specified local commit directory.

## API Routes

The backend exposes REST API endpoints for user, repository, and issue operations.

### User Routes

| Method | Endpoint | Description |
|---|---|---|
| GET | `/allUsers` | Retrieve all users |
| POST | `/signup` | Register a user |
| POST | `/login` | Authenticate a user |
| GET | `/userProfile/:id` | Retrieve a user profile |
| PUT | `/updateProfile/:id` | Update a user profile |
| DELETE | `/deleteProfile/:id` | Delete a user profile |

### Repository Routes

| Method | Endpoint | Description |
|---|---|---|
| POST | `/repo/create` | Create a repository |
| GET | `/repo/all` | Retrieve all repositories |
| GET | `/repo/:id` | Retrieve a repository by ID |
| GET | `/repo/name/:name` | Retrieve a repository by name |
| GET | `/repo/user/:userID` | Retrieve repositories by user |
| PUT | `/repo/update/:id` | Update a repository |
| DELETE | `/repo/delete/:id` | Delete a repository |
| PATCH | `/repo/toggle/:id` | Toggle repository visibility |

### Issue Routes

| Method | Endpoint | Description |
|---|---|---|
| POST | `/issue/create` | Create an issue |
| GET | `/issue/all` | Retrieve all issues |
| GET | `/issue/:id` | Retrieve an issue |
| PUT | `/issue/update/:id` | Update an issue |
| DELETE | `/issue/delete/:id` | Delete an issue |

## Environment Variables

| Variable | Description |
|---|---|
| `PORT` | Backend server port |
| `MONGODB_URI` | MongoDB connection string |
| `JWT_SECRET_KEY` | Secret used for JWT signing |
| `S3_BUCKET` | Configured S3 bucket name |

**Security:** Never commit your `.env` file or AWS credentials to version control.

## Troubleshooting

### 1. MongoDB Connection Error
- Verify your MongoDB connection string.
- Ensure the database is accessible.
- Check network and database permissions.

### 2. AWS S3 Access Error
- Verify the configured bucket name.
- Check your AWS credentials.
- Ensure the bucket is available in the configured region.
- Verify the required S3 permissions.

### 3. Backend Connection Error
- Ensure the backend server is running.
- Check the configured port.
- Verify that frontend API URLs match the backend port.

### 4. Dependency Installation Issues

Try reinstalling dependencies from the relevant frontend or backend directory:

```bash
rm -rf node_modules package-lock.json
npm install
```

## Current Implementation Scope

GitForage currently provides repository management, authentication, profile functionality, issue-management APIs, and a basic filesystem-based version control CLI with AWS S3 integration.

The current version control implementation uses file snapshots and UUID-based commits. It is not a complete implementation of Git's internal object format or Git-compatible protocol.

## License

No license has currently been specified for this project.
