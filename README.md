# MERN Finance Tracker

A full-stack personal finance tracker built with MongoDB, Express, React, and Node.js (MERN). Users sign in with Clerk, then add, edit, and delete financial records (income/expenses) and see their total monthly balance.

## Features

- 🔐 User authentication via [Clerk](https://clerk.com/) (sign up / sign in / user session)
- ➕ Add financial records (description, amount, category, payment method, date)
- ✏️ Edit existing records inline
- 🗑️ Delete records
- 📊 Auto-calculated total monthly balance
- 🔒 Records are scoped per-user (`userId`)

## Tech Stack

**Client**
- React 18 + TypeScript
- Vite
- React Router
- React Table
- Clerk (`@clerk/clerk-react`)

**Server**
- Node.js + Express
- TypeScript
- MongoDB + Mongoose
- CORS

## Project Structure

```
Mern--finance-tracking-main/
├── client/                          # React frontend (Vite)
│   └── src/
│       ├── contexts/
│       │   └── financial-record-context.tsx   # Global state + API calls
│       └── pages/
│           ├── auth/                           # Clerk sign in/up page
│           └── dashboard/                      # Record form, list, dashboard
└── server/                          # Express backend
    └── src/
        ├── index.ts                            # App entry, DB connection
        ├── routes/
        │   └── financial-records.ts            # CRUD REST routes
        └── schema/
            └── financial-record.ts             # Mongoose schema
```

## Prerequisites

- Node.js (v18+ recommended)
- Yarn (project ships with `yarn.lock`)
- A MongoDB connection string (MongoDB Atlas or local)
- A Clerk account and publishable key (for auth)

## Getting Started

### 1. Clone and install dependencies

```bash
git clone <repo-url>
cd Mern--finance-tracking-main

# install server deps
cd server
yarn install

# install client deps
cd ../client
yarn install
```

### 2. Configure environment variables

**Server** — the MongoDB connection string is currently hardcoded in `server/src/index.ts`. Before running, replace it with your own connection string, ideally loaded from an environment variable:

```ts
// server/src/index.ts
const mongoURI: string = process.env.MONGO_URI as string;
```

Create `server/.env`:
```
MONGO_URI=your_mongodb_connection_string
PORT=3001
```

> ⚠️ **Security note:** the current version has a live MongoDB URI (with credentials) committed in `server/src/index.ts`. Rotate those credentials and move the connection string to an environment variable before deploying or making the repo public.

**Client** — Clerk needs a publishable key. Create `client/.env`:
```
VITE_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
```

And wire it up in `client/src/main.tsx` via `<ClerkProvider publishableKey={...}>` if not already present.

### 3. Run the app

In one terminal, start the server:
```bash
cd server
yarn dev
```
The API runs on `http://localhost:3001`.

In another terminal, start the client:
```bash
cd client
yarn dev
```
The app runs on `http://localhost:5173` (default Vite port).

## API Reference

Base URL: `http://localhost:3001/financial-records`

| Method | Endpoint                     | Description                          |
|--------|-------------------------------|---------------------------------------|
| GET    | `/getAllByUserID/:userId`     | Get all records for a user            |
| POST   | `/`                            | Create a new record                   |
| PUT    | `/:id`                         | Update a record by ID                 |
| DELETE | `/:id`                         | Delete a record by ID                 |

**Record shape:**
```ts
{
  userId: string;
  date: Date;
  description: string;
  amount: number;
  category: string;
  paymentMethod: string;
}
```

## Building for Production

```bash
# server
cd server
yarn build   # compiles TypeScript to /build
yarn start

# client
cd client
yarn build   # outputs static assets to /dist
```

## Roadmap Ideas

- Move MongoDB URI and Clerk key to environment variables (see security note above)
- Add pagination/filtering to the records list
- Add category-based spending charts
- Add input validation on the server (e.g. with Zod or Joi)

## License

MIT
