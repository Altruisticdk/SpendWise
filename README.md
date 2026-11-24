# SpendWise – Smart Personal Finance Manager

SpendWise is a web-based personal finance management application for tracking income and expenses, organizing transactions, setting monthly category budgets, and reviewing financial activity.

## Features

- **User accounts:** Register and sign in using an email address and password.
- **Dashboard:** View an overview of recorded transactions and financial activity.
- **Transactions:** Add, view, and delete income and expense records.
- **Budgets:** Create or update monthly spending limits by category, view spending and remaining amounts, and delete budgets.
- **Reports:** Review transaction activity through the reports section.
- **Profile:** Access the profile section of the application.
- **Protected API routes:** Transaction and budget endpoints require a valid JSON Web Token.

## Technology Stack

### Frontend
- React 19
- Vite
- React Router
- Axios
- Tailwind CSS
- Recharts
- Lucide React
- React Hot Toast

### Backend
- Node.js
- Express 5
- MongoDB with Mongoose
- JSON Web Token (JWT) authentication
- bcryptjs password hashing
- Helmet, CORS, Morgan, and dotenv

## Project Structure

```text
SpendWise/
├── backend/
│   ├── src/
│   │   ├── config/          # MongoDB connection
│   │   ├── controllers/     # Authentication, budget, and transaction logic
│   │   ├── middlewares/     # JWT authentication middleware
│   │   ├── models/          # User, Budget, and Transaction schemas
│   │   └── routes/          # API route definitions
│   ├── server.js            # Express application entry point
│   └── package.json
└── frontend/
    ├── public/
    ├── src/
    │   ├── assets/
    │   ├── components/
    │   └── pages/
    └── package.json
```

## Prerequisites

Install the following before running the project:

- Node.js (a current LTS release is recommended)
- npm (included with Node.js)
- MongoDB database (local MongoDB or a MongoDB Atlas cluster)

## Getting Started

### 1. Clone the repository

```bash
git clone <YOUR_REPOSITORY_URL>
cd SpendWise
```

Replace `<YOUR_REPOSITORY_URL>` with the URL of your Git repository.

### 2. Configure the backend environment

Open a terminal in the backend folder:

```bash
cd backend
```

Create a file named `.env` in the `backend` directory:

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=replace_with_a_long_random_secret
```

- `PORT` is the port used by the Express server. The application defaults to `5000` if this variable is omitted.
- `MONGO_URI` is your MongoDB connection string.
- `JWT_SECRET` is the secret used to sign and verify authentication tokens.

**Security:** Never commit `.env` or real credentials to GitHub. Add `backend/.env` to `.gitignore`. If a real credential was included in an uploaded archive or has been exposed, rotate it before publishing the repository. Use a placeholder-only `.env.example` if you want to document the required variables.

### 3. Install backend dependencies and start the API

From the `backend` directory:

```bash
npm install
npm run dev
```

The backend runs on:

```text
http://localhost:5000
```

For a normal start without nodemon, use:

```bash
npm start
```

The server connects to MongoDB before it begins listening for requests.

### 4. Install frontend dependencies and start the web app

Open a second terminal:

```bash
cd SpendWise/frontend
npm install
npm run dev
```

Vite normally serves the frontend at:

```text
http://localhost:5173
```

Open that address in your browser. The frontend currently sends API requests to `http://localhost:5000`, and the backend CORS configuration currently allows `http://localhost:5173`.

### 5. Create an account

Use the registration page to create an account, then sign in to access the finance management pages.

## API Overview

The backend exposes the following routes. Transaction and budget routes are protected and require this request header:

```http
Authorization: Bearer <JWT_TOKEN>
```

| Method | Endpoint | Purpose | Authentication |
|---|---|---|---|
| GET | `/` | API welcome response | No |
| GET | `/api/health` | Server health check | No |
| POST | `/api/auth/register` | Create an account | No |
| POST | `/api/auth/login` | Sign in and receive a token | No |
| GET | `/api/auth/me` | Get the current authenticated user | Yes |
| GET | `/api/transactions` | List the signed-in user's transactions | Yes |
| POST | `/api/transactions` | Create a transaction | Yes |
| DELETE | `/api/transactions/:id` | Delete a transaction | Yes |
| GET | `/api/budgets` | Get monthly budgets and calculated spending | Yes |
| POST | `/api/budgets` | Create or update a category budget | Yes |
| DELETE | `/api/budgets/:id` | Delete a budget | Yes |

### Transaction fields

The transaction API accepts the following fields:

- `type`: `income` or `expense`
- `amount`: positive number
- `category`: transaction category
- `description`: optional description
- `date`: optional transaction date
- `paymentMethod`: `cash`, `card`, `upi`, `bank`, or `other`
- `notes`: optional notes

### Budget fields

The budget API accepts:

- `category`: budget category
- `amount`: positive budget amount
- `month`: month number from 1 to 12 (optional; defaults to the current month)
- `year`: four-digit year (optional; defaults to the current year)

The budget response calculates category spending from the signed-in user's expense transactions for the selected month.

## Available Scripts

### Backend (`backend/`)

| Command | Description |
|---|---|
| `npm run dev` | Start the backend with nodemon |
| `npm start` | Start the backend with Node.js |
| `npm test` | Placeholder test script; automated tests are not configured |

### Frontend (`frontend/`)

| Command | Description |
|---|---|
| `npm run dev` | Start the Vite development server |
| `npm run build` | Create a production build |
| `npm run preview` | Preview the production build locally |
| `npm run lint` | Run ESLint |

## Important Notes

- The backend uses MongoDB. A valid `MONGO_URI` is required for the server to start.
- Passwords are hashed before storage using bcryptjs.
- JWTs are configured to expire after seven days.
- The current frontend API URLs and backend CORS origin are configured for local development. Update them for a production deployment.
- No automated backend test suite is configured in the supplied project.
- Do not upload `node_modules`; dependencies can be restored with `npm install`.

## Future Improvements

Possible enhancements include automated tests, environment-based frontend API configuration, improved deployment documentation, transaction editing, and additional report filters.

## License

No project-specific license file was included in the supplied archive. Add a `LICENSE` file before distributing the project if you want to specify reuse permissions.
