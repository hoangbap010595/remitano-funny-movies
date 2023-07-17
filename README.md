### Candidate: Le Cong Hoang - lchoang1995@gmail.com - 0901.479.784

# 1. Instructions

Fullstack applicants must complete 4 features.
- User registration and login
- Sharing YouTube videos
- Viewing a list of shared videos (no need to display up/down votes)
- Real-time notifications for new video shares: When a user shares a new video, other logged-in users should receive a real-time notification about the newly shared video. This notification can be displayed as a pop-up or a banner in the application, and it should contain the video title and the name of the user who shared it. 

### Demo
> [Back-end](https://rfm-demo-be.thocbeauty.com/graphql) (GraphQL API)

>  [Front-end](https://rfm-demo-fe.thocbeauty.com/)

### Source code
> [Back-end](https://github.com/hoangbap010595/remitano-funny-movies-backend) - [Front-end](https://github.com/hoangbap010595/remitano-funny-movies-frontend)

# 2. Prerequisites

* State-of-the-art architecture with `NodeJS`, `Prisma` and domain-driven design
* Implement the real-time notifications feature using WebSockets `socket.io` and background jobs `redis`.
* Front-end using `ReactJS` with `antd` UI library
* Cross-platform: run it on Windows, Linux, or Mac
* Supports `Docker` out of the box for easy deployment
* Composable, extensible and highly flexible due to modular design

### Backend
| Branch                                                                                                       |  Nest | Prisma                                               |  Graphql                                                              |
| ------------------------------------------------------------------------------------------------------------ | ----- | ---------------------------------------------------- | --------------------------------------------------------------------- |
| main                                                                                                       | v9    | [v4](https://github.com/prisma/prisma)         | [Code-first](https://docs.nestjs.com/graphql/quick-start#code-first)  |

### Frontend
| Branch                                                                                                       |  ReactJS | Antd UI                                               |
| ------------------------------------------------------------------------------------------------------------ | ----- | ---------------------------------------------------- |
| main                                                                                                       | v18.2.0    | [v5.7.0](https://ant.design/)        |


## Overview

- [Instructions](#1-instructions)
- [Prerequisites](#2-prerequisites)
  - [Overview](#overview)
- [Installation & Configuration](#installation-&-configuration)
  - [Backend Setup](#backend-setup)
    - [1. Clone Repository and Install Dependencies](#1-clone-repository-and-install-dependencies)
    - [2. Install MySQL and Redis with Docker](#2-install-mysql-and-redis-with-docker)
    - [3. Prisma: Prisma Migrate](#3-prisma-prisma-migrate)
    - [4. Prisma: Prisma Client JS](#4-prisma-client-js)
    - [5. Seed the database data with this script](#5-seed-the-database-data-with-this-script)
    - [6. Start Backend Server](#6-start-backend-server)
  - [Frontend Setup](#backend-setup)
    - [1. Clone Repository and Install Dependencies](#1-clone-repository-and-install-dependencies)
    - [2. Available Scripts](#2-available-scripts)
  - [GraphQL Playground](#graphql-playground)
- [Docker Deployment](#docker-deployment)
  - [Backend](#backend)
  - [Frontend](#backend)
  - [Schema Development](#schema-development)
- [Usage](#4-usage)
- [Troubleshooting](#5-troubleshooting)

# 3. Installation & Configuration
## Backend Setup

### 1. Clone Repository and Install Dependencies

Clone repository from Github:

```bash
# git
git clone https://github.com/hoangbap010595/remitano-funny-movies-backend.git
cd remitano-funny-movies-backend
```

Install the dependencies for the Backend application:

```bash
# npm
npm install
# yarn
yarn install
```

### 2. Install MySQL and Redis with Docker

Setup a development MySQL with Docker. Open file `.env` - which sets the required environments for MySQL such as `MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_DB`, `MYSQL_PORT`, and `MYSQL_HOST`. Update the variables as you wish and select a strong password. For Redis, sets the required environments `REDIS_HOST` and `REDIS_PORT`

Start the MySQL database

```bash
docker run --name mysql -e MYSQL_ROOT_PASSWORD=znyhCTVg8tneMdAu -e MYSQL_DATABASE=remitano_funny_videos -e MYSQL_USER=remitano_funny_videos -e MYSQL_PASSWORD=znyhCTVg8tneMdAu -p 3306:3306  -d mysql
```

Start the Redis

```bash
docker run --name redis -p 6379:6379  -d redis
```

### 3. Prisma Migrate

[Prisma Migrate](https://github.com/prisma/prisma2/tree/master/docs/prisma-migrate) is used to manage the schema and migration of the database. Prisma datasource requires an environment variable `DATABASE_URL` for the connection to the PostgreSQL database. Prisma reads the `DATABASE_URL` from the root [.env](./.env) file.

Use Prisma Migrate in your [development environment](https://www.prisma.io/blog/prisma-migrate-preview-b5eno5g08d0b#evolving-the-schema-in-development) to

1. Creates `migration.sql` file
2. Updates Database Schema
3. Generates Prisma Client

```bash
npx prisma migrate dev
# or
npm run migrate:dev
```

If you like to customize your `migration.sql` file run the following command. After making your customizations run `npx prisma migrate dev` to apply it.

```bash
npx prisma migrate dev --create-only
# or
npm run migrate:dev:create
```

If you are happy with your database changes you want to deploy those changes to your [production database](https://www.prisma.io/blog/prisma-migrate-preview-b5eno5g08d0b#applying-migrations-in-production-and-other-environments). Use `prisma migrate deploy` to apply all pending migrations, can also be used in CI/CD pipelines as it works without prompts.

```bash
npx prisma migrate deploy
# or
npm run migrate:deploy
```

### 4. Prisma: Prisma Client JS

[Prisma Client JS](https://www.prisma.io/docs/reference/tools-and-interfaces/prisma-client/api) is a type-safe database client auto-generated based on the data model.

Generate Prisma Client JS by running

> **Note**: Every time you update [schema.prisma](prisma/schema.prisma) re-generate Prisma Client JS

```bash
npx prisma generate
# or
npm run prisma:generate
```

### 5. Seed the database data with this script

Execute the script with this command:

```bash
npm run seed
```

### 6. Start Backend Server

Run Backend Server in Development mode:

```bash
npm run start

# watch mode
npm run start:dev
```

Run Backend Server in Production mode:

```bash
npm run start:prod
```

GraphQL Playground for the Backend Server is available here: http://localhost:3000/graphql

**[⬆ back to top](#overview)**

### GraphQL Playground

Open up the [example GraphQL queries](graphql/auth.graphql) and copy them to the GraphQL Playground. Some queries and mutations are secured by an auth guard. You have to acquire a JWT token from `signup` or `login`. Add the `accessToken`as followed to **HTTP HEADERS** in the playground and replace `YOURTOKEN` here:

```json
{
  "Authorization": "Bearer YOURTOKEN"
}
```

## Frontend Setup

### 1. Clone Repository and Install Dependencies

Clone repository from Github:

```bash
# git
git clone https://github.com/hoangbap010595/remitano-funny-movies-frontend.git
cd remitano-funny-movies-frontend
```

Install the dependencies for the Backend application:

```bash
# npm
npm install
# yarn
yarn install
```

### 2. Available Scripts

In the project directory, you can run:

```bash
# npm
npm start
# yarn
yarn start
```

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.\
You will also see any lint errors in the console.

```bash
# npm
npm run test
# yarn
yarn test
```

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

```bash
# npm
npm run build
# yarn
yarn build
```

# 4. Docker Deployment
## Backend
Backend server is a Node.js application and it is easily [dockerized](https://nodejs.org/de/docs/guides/nodejs-docker-webapp/).

See the [Dockerfile](./Dockerfile) on how to build a Docker image of your Backend server.

See the folder [.docker](./docker), open the file `backend-evn.sh` and sets the required environments for MySQL such as `MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_DB`, `MYSQL_PORT`, and `MYSQL_HOST`. Update the variables as you wish and select a strong password. For Redis, sets the required environments `REDIS_HOST` and `REDIS_PORT`

Now to build a Docker image and start the container, run:

```bash
chmod +x backend-evn.sh
chmod +x backend-start-docker.sh
./backend-start-docker.sh
```

Now open up [localhost:3000](http://localhost:3000) to verify that your backend server is running.

When you run your Backend application in a Docker container update your [.env](.env) file

```diff
- REDIS_HOST=localhost
# replace with name of the database container
+ REDIS_HOST=redis

- MYSQL_HOST=localhost
# replace with name of the database container
+ MYSQL_HOST=mysql

# Prisma database connection
+ DATABASE_URL=mysql://${MYSQL_USER}:${MYSQL_PASSWORD}@${MYSQL_HOST}:${MYSQL_PORT}/${MYSQL_DB}
```

If `DATABASE_URL` is missing in the root `.env` file, which is loaded into the Docker container, the Backend application will exit with the following error:

```bash
(node:19) UnhandledPromiseRejectionWarning: Error: error: Environment variable not found: DATABASE_URL.
  -->  schema.prisma:3
   |
 2 |   provider = "mysql"
 3 |   url      = env("DATABASE_URL")
```

## Frontend
See the [Dockerfile](./Dockerfile) on how to build a Docker image of your Frontend server.

See the folder [.docker](./docker), open the file `front-evn.sh` and sets the required environments `REACT_APP_API_URL` and `REACT_APP_WEBSOCKET_URL`

```diff
- REACT_APP_API_URL="http://localhost:3000/graphql"
- REACT_APP_WEBSOCKET_URL="http://localhost:3003"

# replace with name of the application container
+ REACT_APP_API_URL="https://rfm-demo-be.thocbeauty.com/graphql"
+ REACT_APP_WEBSOCKET_URL="https://rfm-demo-ws.thocbeauty.com"
```

Now to build a Docker image and start the container, run:

```bash
chmod +x front-evn.sh
chmod +x front-start-docker.sh
./front-start-docker.sh
```
Now open up [localhost:3000](http://localhost:3000) to verify that your application is running.

## Schema Development

Update the Prisma schema `prisma/schema.prisma` and after that run the following two commands:

```bash
npx prisma generate
# or in watch mode
npx prisma generate --watch
# or
npm run prisma:generate
npm run prisma:generate:watch
```

**[⬆ back to top](#overview)**

# 4. Usage

### 1. User Registration:
1. Visit the registration page.
2. Fill out the required information such as username, email, and password.
3. Click on the "Register" button to create a new user account.

### 2. User Login:
1. Go to the login page.
2. Enter your registered email and password.
3. Click on the "Login" button to access your account.

### 3. Sharing YouTube Videos:
1. Once logged in, Click button "Share a movie" on top-right and go to the video sharing section.
2. Find the option to share a [YouTube](https://www.youtube.com/) video.
3. Enter the URL of the YouTube video you want to share.
4. Click on the "Share" button to share the video.

### 4. Real-Time Notifications for New Video Shares:
1. When a user shares a new video, logged-in users will receive real-time notifications
2. Notifications will appear as pop-ups on the top-right within the application
3. The notification will include the video title and the name of the user who shared it. And you can click on button `Like` or `Dislike` to react video.

### 5. Viewing Shared Videos:
1. Go to the home page.
2. Explore the shared video list shared by other users.
3. Click on a video to watch it directly on the application.

# 5. Troubleshooting
### 1. Database Connection Issues:
1. Ensure that the `database server` and `redis` is running and accessible.
2. Verify the database connection settings in the application's configuration file.
3. Double-check the credentials (username and password) used to connect to the database.

### 2. Check & sets the required environments, variable
1. Frontend: `REACT_APP_API_URL`, `REACT_APP_WEBSOCKET_URL`
2. Backend: `REDIS_HOST`, `MYSQL_HOST`, `ALLOW_CORS`, `DATABASE_URL`
