# Backend Banking Practice

A backend banking and ledger service built with Node.js, Express, and MongoDB. It handles user authentication, account management, idempotent fund transfers, and an immutable ledger system for balance calculation.

## Features

- **Authentication & Authorization**: User registration, login with bcrypt password hashing, JWT-based auth via cookies/headers, token blacklisting for logout, and system-user role checks.
- **Account Management**: Create accounts, fetch user accounts, and compute real-time balances using MongoDB aggregation on ledger records.
- **Transactions & Ledger**: Double-entry style credit/debit entries, immutable ledger entries (updates and deletes blocked at schema level), and idempotency keys to prevent duplicate transactions.
- **Email Notifications**: Transactional alerts on registration and transaction status using Nodemailer.

## Tech Stack

- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose ODM
- **Security**: JWT (`jsonwebtoken`), `bcryptjs`, `cookie-parser`
- **Email**: `nodemailer`

## Getting Started

### Prerequisites

- Node.js (v18+)
- MongoDB (local or MongoDB Atlas connection string)

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/sid-1506/Backend-Banking-Practice.git
   cd Backend-Banking-Practice
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file in the root directory:
   ```env
   MONGO_URI=mongodb://localhost:27017/banking-ledger
   JWT_SECRET=your_jwt_secret_key
   EMAIL_USER=your_email@gmail.com
   CLIENT_ID=your_oauth_client_id
   CLIENT_SECRET=your_oauth_client_secret
   REFRESH_TOKEN=your_oauth_refresh_token
   ```

4. Start the server:
   ```bash
   # Development (with nodemon)
   npm run dev

   # Production
   npm start
   ```

   The server runs on `http://localhost:3000`.

## API Endpoints

### Health Check
- `GET /` - Service status check

### Authentication (`/api/auth`)
- `POST /register` - Register a new user (`email`, `name`, `password`)
- `POST /login` - Login with credentials & receive JWT token
- `POST /logout` - Invalidate active session & blacklist token

### Accounts (`/api/accounts`) *(Protected)*
- `POST /` - Create a new account for the logged-in user
- `GET /` - List all accounts belonging to the logged-in user
- `GET /balance/:accountId` - Get calculated balance from ledger records

### Transactions (`/api/transactions`)
- `POST /` *(Protected)* - Transfer funds between accounts (requires `toAccount`, `amount`, `idempotencyKey`)
- `POST /system/initial-funds` *(System User)* - Credit initial funds from system user

## License

ISC
