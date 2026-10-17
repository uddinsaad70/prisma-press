# 🚀 Prisma Press — Production-Grade TypeScript, Express 5 & Prisma 7 Backend Guide

![TypeScript](https://img.shields.io/badge/TypeScript-7.0-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Express.js](https://img.shields.io/badge/Express-5.2-000000?style=for-the-badge&logo=express&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-7.9-2D3748?style=for-the-badge&logo=prisma&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-Auth-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)

A comprehensive, **step-by-step blueprint and developer handbook** for building a scalable, type-safe RESTful API from scratch using **TypeScript**, **Express 5**, and **Prisma ORM v7 (PostgreSQL)**[cite: 4, 14, 15].

To make this guide maximally useful for both future projects and portfolio review, it is structured into **two complete parts**:

- **🛠️ Part 1: Universal Setup Blueprint (Steps 1–15)** — A reusable, project-agnostic foundation to initialize _any_ TypeScript + Express 5 + Prisma 7 backend from an empty folder to a running server[cite: 14, 15].
- **🚀 Part 2: Prisma Press Implementation & Deep Dive (Steps 16–24)** — The complete, production-ready code for **Multi-File Prisma Schemas**, **Reusable Utility Helpers**, **Transactional Nested Queries**, **JWT HTTP-Only Cookie Authentication**, **Role-Based Access Control (RBAC)**, and **Middleware Interview Notes**[cite: 2, 8, 9, 10, 11, 12, 13, 14, 15].

---

## 🛠️ Tech Stack

| Technology               | Version             | Purpose                                                                          |
| :----------------------- | :------------------ | :------------------------------------------------------------------------------- |
| **TypeScript**           | `^7.0.2`            | Static typing and compile-time safety[cite: 4, 14, 15]                           |
| **Express 5**            | `^5.2.1`            | Fast, unopinionated HTTP server & modular routing[cite: 4, 14, 15]               |
| **Prisma ORM**           | `^7.9.1`            | Next-generation type-safe ORM with `@prisma/adapter-pg`[cite: 4, 14, 15]         |
| **PostgreSQL**           | Latest              | Relational database (Prisma Postgres / Any PostgreSQL)[cite: 14, 15]             |
| **bcryptjs**             | `^3.0.3`            | Password hashing and verification[cite: 2, 4, 14, 15]                            |
| **jsonwebtoken**         | `^9.0.3`            | Stateless Access & Refresh Token generation and verification[cite: 2, 4, 14, 15] |
| **cors & cookie-parser** | `^2.8.6` / `^1.4.7` | Cross-Origin Resource Sharing & HTTP-Only Cookie parsing[cite: 2, 4, 14, 15]     |
| **http-status**          | `^2.1.0`            | Standardized, readable HTTP status code constants[cite: 2, 4, 14, 15]            |
| **dotenv**               | `^17.4.2`           | Environment variable management from `.env`[cite: 2, 4, 14, 15]                  |
| **tsx**                  | `^4.23.12`          | Direct TypeScript execution with live hot-reloading (`watch`)[cite: 4, 14, 15]   |

---

## 📑 Table of Contents

### 🛠️ Part 1: Universal Project Setup (Reusable Boilerplate)

1. [Prerequisites](#1-prerequisites)
2. [Git Initialization & `.gitignore`](#2-git-initialization--gitignore)
3. [Prisma 7 + TypeScript Setup](#3-prisma-7--typescript-setup)
4. [TypeScript Configuration (`tsconfig.json`)](#4-typescript-configuration-tsconfigjson)
5. [Database Setup & `.env`](#5-database-setup--env)
6. [Installing Dependencies](#6-installing-dependencies)
7. [Express Server Setup (`app.ts` & `server.ts`)](#7-express-server-setup-appts--serverts)
8. [`package.json` Scripts](#8-packagejson-scripts)
9. [Centralized Environment Configuration (`src/config`)](#9-centralized-environment-configuration-srcconfig)
10. [Global Middleware Setup](#10-global-middleware-setup)
11. [Multi-File Prisma Schema Setup](#11-multi-file-prisma-schema-prismaschema)
12. [Migrations, DB Push & Client Generation](#12-migrations-db-push--client-generation)
13. [Prisma Client Instance (`src/lib/prisma.ts`)](#13-prisma-client-instance-srclibprismats)
14. [Database Connection in `server.ts`](#14-database-connection-in-serverts)
15. [Running the Server](#15-running-the-server)

### 🚀 Part 2: Prisma Press Implementation & Architecture Reference

16. [Core Utility Helpers (`catchAsync`, `sendResponse`, `jwtUtils`)](#16-core-utility-helpers-catchasync-sendresponse-jwtutils)
17. [User & Profile Module (Nested Writes, Omit & Include)](#17-user--profile-module-nested-writes-omit--include)
18. [Authentication Module (Login, HTTP-Only Cookies & Refresh Token)](#18-authentication-module-login-http-only-cookies--refresh-token)
19. [Role-Based Access Control Middleware (`src/middlewares/auth.ts`)](#19-role-based-access-control-middleware-srcmiddlewaresauthts)
20. [Complete Project Structure](#20-complete-project-structure)
21. [API Endpoints Reference](#21-api-endpoints-reference)
22. [Deep Dive: How Express Middlewares Work (Interview Notes)](#22-deep-dive-how-express-middlewares-work-interview-notes)
23. [Troubleshooting & Common Pitfalls](#23-troubleshooting--common-pitfalls)
24. [Quick Setup Checklist](#24-quick-setup-checklist)

---

---

# 🛠️ PART 1: Universal Project Setup (Reusable Boilerplate)

> **Use Case:** Follow Steps **1 through 15** whenever you want to spin up _any_ new TypeScript + Express + Prisma backend project from scratch[cite: 14, 15].

---

## 1. Prerequisites

Ensure the following tools are installed on your system before starting[cite: 14, 15]:

- **Node.js** (LTS v20+ recommended)[cite: 14, 15]
- **npm** (comes bundled with Node.js)[cite: 14, 15]
- **Git**[cite: 14, 15]
- **PostgreSQL Database** (Prisma Postgres or local/cloud PostgreSQL instance)[cite: 14, 15]

Verify your installations[cite: 14, 15]:

```bash
node -v
npm -v
git --version
```

---

## 2. Git Initialization & `.gitignore`

Create your project directory and initialize Git[cite: 14, 15]:

```bash
mkdir prisma-press && cd prisma-press
git init
```

Create a `.gitignore` file in the root directory to prevent sensitive secrets and heavy build artifacts from being tracked[cite: 14, 15]:

```gitignore
node_modules/
dist/
.env
generated/
```

> ⚠️ **Security Note:** Never commit your `.env` file to version control[cite: 14, 15]. Always commit a `.env.example` template with empty keys so collaborators know which variables are required[cite: 14, 15].

---

## 3. Prisma 7 + TypeScript Setup

Follow steps **1 through 4** of the official Prisma Postgres Quickstart guide[cite: 14, 15]:
👉 [Prisma ORM Quickstart Documentation](https://www.prisma.io/docs/prisma-orm/quickstart/prisma-postgres)[cite: 14, 15]

Run the following initialization commands[cite: 4, 14, 15]:

```bash
npm init -y
npm install typescript tsx @types/node --save-dev
npm install prisma --save-dev
npm install @prisma/client @prisma/adapter-pg pg dotenv
npm install @types/pg --save-dev
npx prisma init --datasource-provider postgresql --output ../generated/prisma
```

This automatically generates[cite: 14, 15]:

- `prisma/schema.prisma`[cite: 14, 15]
- `prisma.config.ts`[cite: 5, 14, 15]
- `.env` (containing a placeholder `DATABASE_URL`)[cite: 14, 15]

**Two Customizations We Will Make[cite: 14, 15]:**

1. We will configure a custom `tsconfig.json` → [See Step 4](#4-typescript-configuration-tsconfigjson)[cite: 14, 15].
2. Instead of running `npx create-db@latest` from the CLI, we will provision our database directly via [Prisma Console](https://www.prisma.io/postgres) → [See Step 5](#5-database-setup--env)[cite: 14, 15].

---

## 4. TypeScript Configuration (`tsconfig.json`)

Create a `tsconfig.json` file in the root directory with the following configuration[cite: 14, 15]:

```json
{
  "compilerOptions": {
    "outDir": "./dist",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "target": "ES2023",
    "types": ["node"],
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true,
    "noUncheckedIndexedAccess": true,
    "strict": true,
    "isolatedModules": true,
    "noUncheckedSideEffectImports": true,
    "moduleDetection": "force",
    "skipLibCheck": true
  },
  // "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

### ⚠️ Key Configuration Details[cite: 14, 15]:

- **Do NOT add `generated/prisma` to `exclude`:** Our `src/lib/prisma.ts` and route files directly import `PrismaClient` and `Enums` from `generated/prisma`[cite: 2, 14, 15]. Excluding it will cause TypeScript resolution errors[cite: 14, 15].
- **Use `"./dist"` for `outDir`:** Always prefix with `./` so TypeScript resolves the folder relative to the project root rather than the OS system root[cite: 14, 15].

---

## 5. Database Setup & `.env`

1. Navigate to [Prisma Postgres](https://www.prisma.io/postgres) (or your preferred PostgreSQL provider) and create a new database[cite: 14, 15].
2. Copy the **Connection String**[cite: 14, 15].
3. Paste it into your root `.env` file[cite: 14, 15]:

```env
DATABASE_URL="postgresql://USER:PASSWORD@HOST:5432/DB_NAME?sslmode=require"
```

---

## 6. Installing Dependencies

> 💡 **Rule of Thumb:** Some packages ship with built-in TypeScript declarations (e.g., `bcryptjs`, `http-status`, `dotenv`), while others require installing `@types/<package>` as devDependencies[cite: 4, 14, 15].

### Step-by-Step Installation[cite: 14, 15]:

```bash
# Express 5
npm i express
npm i -D @types/express

# Password Hashing (Built-in types included)
npm i bcryptjs

# Cross-Origin Resource Sharing (CORS)
npm i cors
npm i -D @types/cors

# Cookie Parsing
npm i cookie-parser
npm i -D @types/cookie-parser

# HTTP Status Constants (Built-in types included)
npm i http-status

# Environment Variables
npm i dotenv

# JSON Web Tokens (JWT)
npm i jsonwebtoken
npm i -D @types/jsonwebtoken
```

### ⚡ One-Line Installation Command[cite: 4, 14, 15]:

```bash
npm i express bcryptjs cors cookie-parser http-status dotenv jsonwebtoken
npm i -D @types/express @types/cors @types/cookie-parser @types/jsonwebtoken @types/pg
```

---

## 7. Express Server Setup (`app.ts` & `server.ts`)

Reference: [Express 5 Hello World Guide](https://expressjs.com/en/5x/starter/hello-world/)[cite: 14, 15]

Create a `src/` directory with two entry files[cite: 2, 14, 15]:

### `src/server.ts` (Initial Skeleton)[cite: 14, 15]

```ts
async function main() {
  try {
    // Database connection and server listener will be added in Step 14
  } catch (error) {
    console.error(error);
  }
}

main();
```

### `src/app.ts` (Initial Version)[cite: 2, 14, 15]

```ts
import express, { Application, Request, Response } from "express";

const app: Application = express();

app.get("/", (req: Request, res: Response) => {
  res.send("Hello f");
});

export default app;
```

> 💡 **Why separate `app.ts` and `server.ts`?**[cite: 14, 15]
>
> - `app.ts` handles the Express application configuration (middlewares, route mounting, error handlers)[cite: 2, 14, 15].
> - `server.ts` manages infrastructure concerns (connecting/disconnecting the database, listening on a network port, handling process signals)[cite: 2, 14, 15].
> - This separation keeps the codebase modular and allows testing frameworks (like Supertest/Jest) to import `app` without binding to a live port[cite: 14, 15].

---

## 8. `package.json` Scripts

Ensure `"type": "module"` is set in your `package.json` along with the following scripts[cite: 4, 14, 15]:

```json
{
  "name": "prisma-press-backend",
  "version": "0.0.1",
  "type": "module",
  "main": "server.js",
  "scripts": {
    "dev": "tsx watch src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  }
}
```

| Command         | Description                                                                                    |
| :-------------- | :--------------------------------------------------------------------------------------------- |
| `npm run dev`   | Runs `src/server.ts` directly via `tsx` with live file watching (Development)[cite: 4, 14, 15] |
| `npm run build` | Compiles TypeScript files into JavaScript inside `./dist`[cite: 4, 14, 15]                     |
| `npm start`     | Executes the compiled production build from `dist/server.js`[cite: 4, 14, 15]                  |

---

## 9. Centralized Environment Configuration (`src/config`)

Instead of scattering `process.env.VARIABLE` calls throughout the codebase, we centralize all environment variables in `src/config/index.ts`[cite: 2, 14, 15].

### `.env`[cite: 15]

```env
PORT=5000
DATABASE_URL="postgresql://USER:PASSWORD@HOST:5432/DB_NAME?sslmode=require"
APP_URL="http://localhost:3000"
BCRYPT_SALT_ROUNDS=10
JWT_ACCESS_SECRET="your-ultra-secure-access-secret-key"
JWT_REFRESH_SECRET="your-ultra-secure-refresh-secret-key"
JWT_ACCESS_EXPIRES_IN="1d"
JWT_REFRESH_EXPIRES_IN="7d"
```

### `.env.example` (Safe to commit to Git)[cite: 14, 15]

```env
PORT=5000
DATABASE_URL=
APP_URL=
BCRYPT_SALT_ROUNDS=
JWT_ACCESS_SECRET=
JWT_REFRESH_SECRET=
JWT_ACCESS_EXPIRES_IN=
JWT_REFRESH_EXPIRES_IN=
```

### `src/config/index.ts`[cite: 2, 14, 15]

```ts
import dotenv from "dotenv";
import path from "path";

dotenv.config({ path: path.join(process.cwd(), ".env") });

export default {
  PORT: process.env.PORT || 5000,
  database_url: process.env.DATABASE_URL,
  app_url: process.env.APP_URL,
  bcrypt_salt_rounds: process.env.BCRYPT_SALT_ROUNDS!,
  jwt_access_secret: process.env.JWT_ACCESS_SECRET!,
  jwt_refresh_secret: process.env.JWT_REFRESH_SECRET!,
  jwt_access_expires_in: process.env.JWT_ACCESS_EXPIRES_IN!,
  jwt_refresh_expires_in: process.env.JWT_REFRESH_EXPIRES_IN!,
};
```

---

## 10. Global Middleware Setup

Update `src/app.ts` to register essential global middlewares **before** mounting any routes[cite: 2, 14, 15]:

```ts
import cookieParser from "cookie-parser";
import express, { Application, Request, Response } from "express";
import cors from "cors";
import config from "./config";
import { userRoutes } from "./modules/user/user.route";
import { authRoute } from "./modules/auth/auth.route";

const app: Application = express();

// 1. Enable CORS with credentials support for HTTP-only cookies
app.use(
  cors({
    origin: config.app_url,
    credentials: true,
  }),
);

// 2. Request body & cookie parsers
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(cookieParser());

// 3. Health-check route
app.get("/", (req: Request, res: Response) => {
  res.send("Hello f");
});

// 4. Application API Routes
app.use("/api/users", userRoutes);
app.use("/api/auth", authRoute);

export default app;
```

> 📖 For a detailed breakdown of each middleware and why order matters, jump to [Step 22: Deep Dive into Middlewares](#22-deep-dive-how-express-middlewares-work-interview-notes)[cite: 14, 15].

---

## 11. Multi-File Prisma Schema (`prisma/schema/`)

As an application grows, maintaining dozens of models inside a single `schema.prisma` file becomes unwieldy[cite: 14, 15]. Prisma supports splitting your schema across multiple `.prisma` files inside a dedicated directory[cite: 8, 9, 10, 11, 12, 13, 14, 15].
👉 [Official Multi-File Prisma Schema Docs](https://www.prisma.io/docs/orm/prisma-schema/data-model/multi-schema)[cite: 14, 15]

### Step 1: Point `prisma.config.ts` to the Schema Directory[cite: 5, 14, 15]

Update `prisma.config.ts` so Prisma scans the entire `prisma/schema` directory instead of a single file[cite: 5, 14, 15]:

```ts
import "dotenv/config";
import { defineConfig } from "prisma/config";

export default defineConfig({
  schema: "prisma/schema", // 👈 Points to the folder containing all .prisma files
  migrations: {
    path: "prisma/migrations",
  },
  datasource: {
    url: process.env["DATABASE_URL"],
  },
});
```

> ⚠️ **Crucial:** If you forget to update `schema: "prisma/schema"`, Prisma will ignore your separate model files during migrations and client generation[cite: 14, 15].

### Step 2: Schema Directory Structure[cite: 8, 9, 10, 11, 12, 13, 15]

```text
prisma/
├── schema/
│   ├── schema.prisma      # Generator & Datasource configuration
│   ├── enums.prisma       # Shared Enums (ActiveStatus, Role, PostStatus, CommentStatus)
│   ├── user.prisma        # User Model
│   ├── profile.prisma     # Profile Model (1:1 with User)
│   ├── post.prisma        # Post Model (1:N with User)
│   └── comment.prisma     # Comment Model (N:1 with User & Post)
└── migrations/
```

### Step 3: Define the Schema Files[cite: 8, 9, 10, 11, 12, 13, 15]

#### 1. `prisma/schema/schema.prisma`[cite: 12, 15]

```prisma
generator client {
  provider = "prisma-client"
  output   = "../../generated/prisma"
}

datasource db {
  provider = "postgresql"
}
```

> 📌 **Understanding the `output` path:** Because `schema.prisma` resides inside `prisma/schema/` (two levels deep from the project root), we use `"../../generated/prisma"` so the generated client is placed cleanly at `<project-root>/generated/prisma`[cite: 3, 12, 14, 15].

#### 2. `prisma/schema/enums.prisma`[cite: 9, 15]

```prisma
enum ActiveStatus {
  ACTIVE
  INACTIVE
  BLOCKED
}

enum Role {
  USER
  ADMIN
  AUTHOR
}

enum PostStatus {
  DRAFT
  PUBLISHED
  ARCHIVED
}

enum CommentStatus {
  APPROVED
  REJECTED
}
```

#### 3. `prisma/schema/user.prisma`[cite: 13, 15]

```prisma
model User {
  id           String       @id @default(uuid())
  name         String       @db.VarChar(255)
  email        String       @unique
  password     String
  activeStatus ActiveStatus @default(ACTIVE)
  role         Role         @default(USER)
  createdAt    DateTime     @default(now())
  updatedAt    DateTime     @updatedAt

  profile      Profile?
  posts        Post[]
  comments     Comment[]

  @@map("users")
}
```

#### 4. `prisma/schema/profile.prisma`[cite: 11, 15]

```prisma
model Profile {
  id           String   @id @default(uuid())
  profilePhoto String?
  bio          String?
  userId       String   @unique
  user         User     @relation(fields: [userId], references: [id])

  createdAt    DateTime @default(now())
  updatedAt    DateTime @updatedAt

  @@map("profiles")
}
```

#### 5. `prisma/schema/post.prisma`[cite: 10, 15]

```prisma
model Post {
  id         String     @id @default(uuid())
  title      String     @db.VarChar(255)
  content    String
  thumbnail  String?
  isFeatured Boolean    @default(false)
  status     PostStatus @default(PUBLISHED)
  tags       String[]
  views      Int        @default(0)
  createdAt  DateTime   @default(now())
  updatedAt  DateTime   @updatedAt

  authorId   String
  author     User       @relation(fields: [authorId], references: [id], onDelete: Cascade)

  comments   Comment[]

  @@index([authorId])
  @@map("posts")
}
```

#### 6. `prisma/schema/comment.prisma`[cite: 8, 15]

```prisma
model Comment {
  id       String        @id @default(uuid())
  content  String        @db.Text
  authorId String
  author   User          @relation(fields: [authorId], references: [id], onDelete: Cascade)

  postId   String
  post     Post          @relation(fields: [postId], references: [id], onDelete: Cascade)
  status   CommentStatus @default(APPROVED)

  @@index([postId])
  @@index([authorId])
  @@map("comments")
}
```

---

## 12. Migrations, DB Push & Client Generation

Run the following commands whenever you create or modify your Prisma schema[cite: 14, 15]:

```bash
# 1. Create a SQL migration file and apply it to the database
npx prisma migrate dev --name init

# 2. (Alternative) Push schema changes directly without creating migration files
npx prisma db push

# 3. Generate the TypeScript Prisma Client inside ./generated/prisma
npx prisma generate

# 4. Open Prisma Studio GUI to inspect & edit database records in your browser
npx prisma studio
```

> 💡 **Pro Tip — Schema Recovery:**
> If your local `prisma/schema/` folder is ever accidentally deleted:
>
> 1. Check `generated/prisma/internal/class.ts` under the `"inlineSchema"` property—Prisma stores a full verbatim copy of your entire schema there[cite: 3, 15]!
> 2. Alternatively, run `npx prisma db pull` to introspect your live PostgreSQL database and reconstruct your schema automatically[cite: 15].

---

## 13. Prisma Client Instance (`src/lib/prisma.ts`)

Complete Step 7 of the Prisma Quickstart by initializing `PrismaClient` with the `@prisma/adapter-pg` driver adapter[cite: 2, 14, 15]:

```ts
import "dotenv/config";
import { PrismaPg } from "@prisma/adapter-pg";
import { PrismaClient } from "../../generated/prisma/client";

const connectionString = `${process.env.DATABASE_URL}`;

const adapter = new PrismaPg({ connectionString });
const prisma = new PrismaClient({ adapter });

export { prisma };
```

### ⚠️ Important Reminders[cite: 14, 15]:

- Always import `PrismaClient` from `"../../generated/prisma/client"` (not from `@prisma/client/extension`, which VS Code might auto-suggest)[cite: 2, 14, 15].
- Verify the relative path from `src/lib/prisma.ts` to the root `generated/` folder is `../../generated/prisma/client`[cite: 2, 14, 15].

---

## 14. Database Connection in `server.ts`

Update `src/server.ts` so the application verifies the database connection before accepting HTTP traffic[cite: 2, 14, 15]:

```ts
import app from "./app";
import config from "./config";
import { prisma } from "./lib/prisma";

const PORT = config.PORT;

async function main() {
  try {
    await prisma.$connect();
    console.log("Connected to the database successfully.");

    app.listen(PORT, () => {
      console.log(`Server is running on port ${PORT}`);
    });
  } catch (error) {
    console.error("Error starting the server:", error);
    await prisma.$disconnect();
    process.exit(1);
  }
}

main();
```

---

## 15. Running the Server

Start the development server[cite: 4, 14, 15]:

```bash
npm run dev
```

Expected console output[cite: 2, 14, 15]:

```text
Connected to the database successfully.
Server is running on port 5000
```

Visit `http://localhost:5000/` in your browser or Postman to see `Hello f`[cite: 2, 14, 15]. ✅

---

---

# 🚀 PART 2: Prisma Press Implementation & Architecture Reference

> **Use Case:** Steps **16 through 24** document the complete feature implementation of **Prisma Press** (Reusable Utilities, User & Profile Nested Operations, JWT Cookie Authentication, RBAC Middleware, and Interview Notes)[cite: 2, 14, 15]. You can also copy these modules directly as a starter template for Authentication & RBAC in future projects!

---

## 16. Core Utility Helpers (`catchAsync`, `sendResponse`, `jwtUtils`)

To keep our controllers clean and DRY (Don't Repeat Yourself), we create three reusable utilities in `src/utils/`[cite: 2, 15]:

### 1. `src/utils/catchAsync.ts` (Higher-Order Async Error Wrapper)[cite: 2, 15]

Eliminates repetitive `try-catch` blocks in Express controllers[cite: 2, 15]:

```ts
import { NextFunction, Request, RequestHandler, Response } from "express";
import httpStatus from "http-status";

export const catchAsync = (fn: RequestHandler) => {
  return async (req: Request, res: Response, next: NextFunction) => {
    try {
      await fn(req, res, next);
    } catch (error) {
      console.log(error);
      res.status(httpStatus.INTERNAL_SERVER_ERROR).json({
        success: false,
        statusCode: httpStatus.INTERNAL_SERVER_ERROR,
        message: "Failed to register user",
        error: (error as Error).message,
      });
    }
  };
};
```

### 2. `src/utils/sendResponse.ts` (Unified API Response Formatter)[cite: 2, 15]

Ensures every API endpoint returns a consistent JSON structure (`success`, `statusCode`, `message`, `data`, `meta`)[cite: 2, 15]:

```ts
import { Response } from "express";

type TMeta = {
  page: number;
  limit: number;
  total: number;
};

type TResponseData<T> = {
  success: boolean;
  statusCode: number;
  message: string;
  data: T;
  meta?: TMeta;
};

export const sendResponse = <T>(res: Response, data: TResponseData<T>) => {
  res.status(data.statusCode).json({
    success: data.success,
    statusCode: data.statusCode,
    message: data.message,
    data: data.data,
    meta: data.meta,
  });
};
```

### 3. `src/utils/jwt.ts` (JWT Sign & Verify Utility)[cite: 2, 15]

```ts
import jwt, { JwtPayload, SignOptions } from "jsonwebtoken";

const createToken = (
  payload: JwtPayload,
  secret: string,
  expiresIn: SignOptions,
) => {
  const token = jwt.sign(payload, secret, { expiresIn } as SignOptions);
  return token;
};

const verifyToken = (token: string, secret: string) => {
  try {
    return {
      success: true,
      data: jwt.verify(token, secret),
    };
  } catch (error: any) {
    console.log("Token verification failed", error);
    return {
      success: false,
      error: error.message,
    };
  }
};

export const jwtUtils = {
  createToken,
  verifyToken,
};
```

---

## 17. User & Profile Module (Nested Writes, Omit & Include)

Key Prisma features demonstrated in `src/modules/user/`[cite: 2, 15]:

- **Transactional Nested Writes (`profile: { create: ... }` & `profile: { update: ... }`):** Creates or updates both the `User` and `Profile` records in a single atomic operation[cite: 2, 15].
- **Field Omission (`omit: { password: true }`):** Strips the hashed password before returning the user object[cite: 2, 15].
- **Eager Loading (`include: { profile: true }`):** Joins and returns the associated `Profile` record[cite: 2, 15].

### `src/modules/user/user.interface.ts`[cite: 2, 15]

```ts
export interface RegisterUserPayload {
  name: string;
  email: string;
  password: string;
  profilePhoto?: string;
}
```

### `src/modules/user/user.service.ts`[cite: 2, 15]

```ts
import bcrypt from "bcryptjs";
import { prisma } from "../../lib/prisma";
import config from "../../config";
import { RegisterUserPayload } from "./user.interface";

const registerUserIntoDB = async (payload: RegisterUserPayload) => {
  const { name, email, password, profilePhoto } = payload;

  const isUserExists = await prisma.user.findUnique({
    where: { email },
  });

  if (isUserExists) {
    throw new Error("User with this email already exists");
  }

  const hashedPassword = await bcrypt.hash(
    password,
    Number(config.bcrypt_salt_rounds),
  );

  // Nested Creation: Creates User and associated Profile simultaneously
  const createdUser = await prisma.user.create({
    data: {
      name,
      email,
      password: hashedPassword,
      profile: {
        create: {
          profilePhoto,
        },
      },
    },
  });

  const user = await prisma.user.findUnique({
    where: {
      id: createdUser.id,
      email: createdUser.email || email,
    },
    omit: { password: true },
    include: {
      profile: true,
    },
  });

  return user;
};

const getMyProfileFromDB = async (userId: string) => {
  const user = await prisma.user.findUniqueOrThrow({
    where: { id: userId },
    omit: { password: true },
    include: { profile: true },
  });
  return user;
};

const updateMyProfileInDB = async (userID: string, payload: any) => {
  const { name, email, profilePhoto, bio } = payload;

  const updatedUser = await prisma.user.update({
    where: { id: userID },
    data: {
      name,
      email,
      profile: {
        update: {
          profilePhoto,
          bio,
        },
      },
    },
    omit: { password: true },
    include: { profile: true },
  });

  return updatedUser;
};

export const userService = {
  registerUserIntoDB,
  getMyProfileFromDB,
  updateMyProfileInDB,
};
```

### `src/modules/user/user.controller.ts`[cite: 2, 15]

```ts
import { NextFunction, Request, Response } from "express";
import httpStatus from "http-status";
import { userService } from "./user.service";
import { catchAsync } from "../../utils/catchAsync";
import { sendResponse } from "../../utils/sendResponse";

const registerUser = catchAsync(
  async (req: Request, res: Response, next: NextFunction) => {
    const payload = req.body;
    const user = await userService.registerUserIntoDB(payload);

    sendResponse(res, {
      success: true,
      statusCode: httpStatus.CREATED,
      message: "User registered successfully",
      data: { user },
    });
  },
);

const getMyProfile = catchAsync(
  async (req: Request, res: Response, next: NextFunction) => {
    const profile = await userService.getMyProfileFromDB(
      req.user?.id as string,
    );

    sendResponse(res, {
      success: true,
      statusCode: httpStatus.OK,
      message: "User profile fetched successfully",
      data: { profile },
    });
  },
);

const updateMyProfile = catchAsync(
  async (req: Request, res: Response, next: NextFunction) => {
    const userId = req.user?.id as string;
    const payload = req.body;
    const updatedUser = await userService.updateMyProfileInDB(userId, payload);

    sendResponse(res, {
      success: true,
      statusCode: httpStatus.OK,
      message: "User profile updated successfully",
      data: { updatedUser },
    });
  },
);

export const userController = {
  registerUser,
  getMyProfile,
  updateMyProfile,
};
```

### `src/modules/user/user.route.ts`[cite: 2, 15]

```ts
import { Router } from "express";
import { userController } from "./user.controller";
import { Role } from "../../../generated/prisma/enums";
import { auth } from "../../middlewares/auth";

const router = Router();

router.post("/register", userController.registerUser);

router.get("/me", auth(Role.ADMIN, Role.USER), userController.getMyProfile);

router.put(
  "/my-profile",
  auth(Role.ADMIN, Role.USER, Role.AUTHOR),
  userController.updateMyProfile,
);

export const userRoutes = router;
```

---

## 18. Authentication Module (Login, HTTP-Only Cookies & Refresh Token)

Located in `src/modules/auth/`, this module handles user login, status verification (`BLOCKED` check), token issuance, and refresh token rotation via `httpOnly` cookies[cite: 2, 15].

### `src/modules/auth/auth.interface.ts`[cite: 2, 15]

```ts
export interface ILogInUser {
  email: string;
  password: string;
}
```

### `src/modules/auth/auth.service.ts`[cite: 2, 15]

```ts
import bcrypt from "bcryptjs";
import { prisma } from "../../lib/prisma";
import { ILogInUser } from "./auth.interface";
import jwt, { SignOptions } from "jsonwebtoken";
import config from "../../config";
import { jwtUtils } from "../../utils/jwt";

const loginUser = async (payload: ILogInUser) => {
  const { email, password } = payload;

  const user = await prisma.user.findUniqueOrThrow({
    where: { email },
  });

  if (user.activeStatus === "BLOCKED") {
    throw new Error("Your account has been blocked. Please contact support.");
  }

  const isPasswordMatched = await bcrypt.compare(password, user.password);
  if (!isPasswordMatched) {
    throw new Error("password is incorrect");
  }

  const jwtPayload = {
    id: user.id,
    name: user.name,
    email: user.email,
    role: user.role,
  };

  const accessToken = jwtUtils.createToken(
    jwtPayload,
    config.jwt_access_secret,
    config.jwt_access_expires_in as SignOptions,
  );

  const refreshToken = jwtUtils.createToken(
    jwtPayload,
    config.jwt_refresh_secret,
    config.jwt_refresh_expires_in as SignOptions,
  );

  return {
    accessToken,
    refreshToken,
  };
};

const refreshToken = async (refreshToken: string) => {
  const verifiedRefreshToken = jwtUtils.verifyToken(
    refreshToken,
    config.jwt_refresh_secret,
  );

  if (!verifiedRefreshToken.success) {
    throw new Error(verifiedRefreshToken.error);
  }

  const { id } = verifiedRefreshToken.data as jwt.JwtPayload;

  const user = await prisma.user.findUniqueOrThrow({
    where: { id },
  });

  if (user.activeStatus === "BLOCKED") {
    throw new Error("Your account has been blocked. Please contact support.");
  }

  const jwtPayload = {
    id: user.id,
    name: user.name,
    email: user.email,
    role: user.role,
  };

  const newAccessToken = jwtUtils.createToken(
    jwtPayload,
    config.jwt_access_secret,
    config.jwt_access_expires_in as SignOptions,
  );

  return { accessToken: newAccessToken };
};

export const authService = {
  loginUser,
  refreshToken,
};
```

### `src/modules/auth/auth.controller.ts`[cite: 2, 15]

```ts
import { NextFunction, Request, Response } from "express";
import { catchAsync } from "../../utils/catchAsync";
import { authService } from "./auth.service";
import httpStatus from "http-status";
import { sendResponse } from "../../utils/sendResponse";

const loginUser = catchAsync(
  async (req: Request, res: Response, next: NextFunction) => {
    const payload = req.body;
    const { accessToken, refreshToken } = await authService.loginUser(payload);

    res.cookie("accessToken", accessToken, {
      httpOnly: true,
      secure: false,
      sameSite: "none",
      maxAge: 1000 * 60 * 60 * 24, // 24 hours
    });

    res.cookie("refreshToken", refreshToken, {
      httpOnly: true,
      secure: false,
      sameSite: "none",
      maxAge: 1000 * 60 * 60 * 24 * 7, // 7 days
    });

    sendResponse(res, {
      success: true,
      statusCode: httpStatus.OK,
      message: "User logged in successfully",
      data: { accessToken, refreshToken },
    });
  },
);

const refreshToken = catchAsync(
  async (req: Request, res: Response, next: NextFunction) => {
    const refreshToken = req.cookies.refreshToken;
    const { accessToken } = await authService.refreshToken(refreshToken);

    res.cookie("accessToken", accessToken, {
      httpOnly: true,
      secure: false,
      sameSite: "none",
      maxAge: 1000 * 60 * 60 * 24, // 24 hours
    });

    sendResponse(res, {
      success: true,
      statusCode: httpStatus.OK,
      message: "Token refreshed successfully",
      data: { accessToken },
    });
  },
);

export const authController = {
  loginUser,
  refreshToken,
};
```

### `src/modules/auth/auth.route.ts`[cite: 2, 15]

```ts
import { Router } from "express";
import { authController } from "./auth.controller";

const router = Router();

router.post("/login", authController.loginUser);
router.post("/refresh-token", authController.refreshToken);

export const authRoute = router;
```

---

## 19. Role-Based Access Control Middleware (`src/middlewares/auth.ts`)

This custom middleware extracts the JWT from either `req.cookies.accessToken` or the `Authorization: Bearer <token>` header, verifies its signature, checks if the user's role matches the allowed roles, confirms the user is not `BLOCKED` in the database, and attaches the decoded user to `req.user`[cite: 2, 15]:

```ts
import { NextFunction, Request, Response } from "express";
import { Role } from "../../generated/prisma/enums";
import { catchAsync } from "../utils/catchAsync";
import { jwtUtils } from "../utils/jwt";
import { JwtPayload } from "jsonwebtoken";
import { prisma } from "../lib/prisma";
import config from "../config";

// Extend Express Request type globally to support req.user
declare global {
  namespace Express {
    interface Request {
      user?: {
        email: string;
        name: string;
        id: string;
        role: Role;
      };
    }
  }
}

export const auth = (...requiredRoles: Role[]) => {
  return catchAsync(async (req: Request, res: Response, next: NextFunction) => {
    const token = req.cookies.accessToken
      ? req.cookies.accessToken
      : req.headers.authorization?.startsWith("Bearer ")
        ? req.headers.authorization?.split(" ")[1]
        : req.headers.authorization;

    if (!token) {
      throw new Error("You are not logged in! Please log in to get access.");
    }

    const verifiedToken = jwtUtils.verifyToken(token, config.jwt_access_secret);
    if (!verifiedToken.success) {
      throw new Error(verifiedToken.error);
    }

    const { name, email, id, role } = verifiedToken.data as JwtPayload;

    if (requiredRoles.length && !requiredRoles.includes(role)) {
      throw new Error("You do not have permission to access this resource");
    }

    const user = await prisma.user.findUnique({
      where: { id, email, name, role },
    });

    if (!user) {
      throw new Error("User not found. Please log in again.");
    }

    if (user.activeStatus === "BLOCKED") {
      throw new Error("Your account has been blocked. Please contact support.");
    }

    req.user = { name, email, id, role };
    next();
  });
};
```

---

## 20. Complete Project Structure

```text
prisma-press/
├── generated/
│   └── prisma/                      # Auto-generated Prisma Client (v7.9.1)
├── prisma/
│   ├── schema/
│   │   ├── schema.prisma            # Generator & PostgreSQL Datasource
│   │   ├── enums.prisma             # ActiveStatus, Role, PostStatus, CommentStatus
│   │   ├── user.prisma              # User Model
│   │   ├── profile.prisma           # Profile Model
│   │   ├── post.prisma              # Post Model
│   │   └── comment.prisma           # Comment Model
│   └── migrations/                  # SQL Migration History
├── src/
│   ├── config/
│   │   └── index.ts                 # Centralized .env configuration
│   ├── lib/
│   │   └── prisma.ts                # PrismaClient instance with @prisma/adapter-pg
│   ├── middlewares/
│   │   └── auth.ts                  # JWT verification & RBAC middleware
│   ├── modules/
│   │   ├── auth/
│   │   │   ├── auth.controller.ts
│   │   │   ├── auth.interface.ts
│   │   │   ├── auth.route.ts
│   │   │   └── auth.service.ts
│   │   └── user/
│   │       ├── user.controller.ts
│   │       ├── user.interface.ts
│   │       ├── user.route.ts
│   │       └── user.service.ts
│   ├── utils/
│   │   ├── catchAsync.ts            # Higher-order async error handler
│   │   ├── jwt.ts                   # JWT creation & verification helper
│   │   └── sendResponse.ts          # Standardized JSON response utility
│   ├── app.ts                       # Express app setup, middlewares & routes
│   └── server.ts                    # Database connection & server bootstrap
├── .env                             # Local environment secrets (gitignored)
├── .env.example                     # Environment variable template
├── .gitignore
├── package.json
├── prisma.config.ts                 # Prisma 7 CLI configuration
├── tsconfig.json                    # TypeScript compiler configuration
└── README.md
```

---

## 21. API Endpoints Reference

| HTTP Method | Endpoint                  | Allowed Roles             | Description                                                                     |
| :---------- | :------------------------ | :------------------------ | :------------------------------------------------------------------------------ |
| `GET`       | `/`                       | Public                    | Server health check (`Hello f`)[cite: 2, 14, 15]                                |
| `POST`      | `/api/users/register`     | Public                    | Registers a new user and creates a nested profile[cite: 2, 15]                  |
| `GET`       | `/api/users/me`           | `ADMIN`, `USER`           | Fetches the authenticated user's profile[cite: 2, 15]                           |
| `PUT`       | `/api/users/my-profile`   | `ADMIN`, `USER`, `AUTHOR` | Updates user details and nested profile (`bio`, `profilePhoto`)[cite: 2, 15]    |
| `POST`      | `/api/auth/login`         | Public                    | Authenticates user and sets `accessToken` & `refreshToken` cookies[cite: 2, 15] |
| `POST`      | `/api/auth/refresh-token` | Cookie (`refreshToken`)   | Issues a new `accessToken` using a valid `refreshToken`[cite: 2, 15]            |

---

## 22. Deep Dive: How Express Middlewares Work (Interview Notes)

### What is a Middleware?[cite: 14, 15]

A middleware is a function that sits in the request-response pipeline and executes **before** the final route controller sends a response[cite: 14, 15]. Each `app.use()` call adds a processing stage to that pipeline[cite: 14, 15]:

```text
Incoming Request ──► cors() ──► express.json() ──► urlencoded() ──► cookieParser() ──► auth() ──► Controller ──► Response
```

### 1. `cors({ origin, credentials })`[cite: 14, 15]

- **CORS** = Cross-Origin Resource Sharing[cite: 14, 15].
- By default, web browsers block frontend applications hosted on one origin (e.g., `http://localhost:3000`) from calling an API on another origin (e.g., `http://localhost:5000`)[cite: 14, 15].
- `origin: config.app_url` restricts API access strictly to your trusted frontend URL[cite: 14, 15].
- `credentials: true` allows the browser to send and receive cookies and authorization headers across origins[cite: 14, 15].
- **In one line:** Controls which frontend domain can communicate with your API and exchange authentication cookies[cite: 14, 15].

### 2. `express.json()`[cite: 14, 15]

- Parses incoming JSON payloads from `POST`/`PUT`/`PATCH` requests and populates `req.body` as a JavaScript object[cite: 14, 15].
- ⚠️ Without this middleware, `req.body` will be `undefined`[cite: 14, 15].
- **In one line:** Raw JSON string → `req.body` object[cite: 14, 15].

### 3. `express.urlencoded({ extended: true })`[cite: 14, 15]

- Parses URL-encoded form data (`application/x-www-form-urlencoded`, e.g., `email=abc&password=123`) into `req.body`[cite: 14, 15].
- `extended: true` uses the `qs` library to support rich nested objects and arrays, whereas `extended: false` only parses flat key-value pairs[cite: 14, 15].
- **In one line:** Form data → `req.body` object[cite: 14, 15].

### 4. `cookieParser()`[cite: 14, 15]

- Parses the raw `Cookie` HTTP header (`Cookie: accessToken=xyz; refreshToken=abc`) and populates `req.cookies` as a convenient key-value object[cite: 14, 15].
- Essential for reading JWT tokens stored in HTTP-Only cookies[cite: 2, 14, 15].
- **In one line:** Raw cookie header → `req.cookies` object[cite: 14, 15].

### ⚠️ Why Middleware Order Matters[cite: 14, 15]

Express executes middlewares strictly in the order they are declared[cite: 14, 15]:

1. `cors()` should run first to handle preflight (`OPTIONS`) requests immediately[cite: 14, 15].
2. `express.json()` and `cookieParser()` **must** be registered before your route handlers; otherwise, `req.body` and `req.cookies` will be `undefined` inside your controllers[cite: 14, 15].

### 💡 Key Interview Questions[cite: 14, 15]

- **Why set `credentials: true` in CORS?** → Required for sending/receiving cookies (`httpOnly` JWTs) and `Authorization` headers in cross-origin requests[cite: 14, 15].
- **Why use a specific `origin` instead of `"*"`?** → Security: prevents unauthorized third-party websites from abusing your API[cite: 14, 15]. Additionally, browsers forbid combining `origin: "*"` with `credentials: true`[cite: 14, 15].
- **What is the difference between `extended: true` vs `false`?** → `true` allows parsing nested objects and arrays in URL-encoded payloads; `false` only supports flat key-value strings[cite: 14, 15].

---

## 23. Troubleshooting & Common Pitfalls

| Issue / Error                                          | Root Cause                                                                                                | Solution                                                                                                                          |
| :----------------------------------------------------- | :-------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------- |
| `Cannot find module '../../generated/prisma/client'`   | `prisma generate` has not been run, or the relative path is inaccurate[cite: 14, 15].                     | Run `npx prisma generate` and verify the `output` path in `schema.prisma` matches your import[cite: 14, 15].                      |
| TypeScript cannot resolve types in `generated/prisma`  | `generated/prisma` is listed inside `exclude` in `tsconfig.json`[cite: 14, 15].                           | Remove `generated/prisma` from `tsconfig.json` `exclude`[cite: 14, 15].                                                           |
| New `.prisma` model files are ignored during migration | `prisma.config.ts` still points to a single file (`prisma/schema.prisma`)[cite: 14, 15].                  | Update `prisma.config.ts` to point to the folder: `schema: "prisma/schema"`[cite: 5, 14, 15].                                     |
| `req.body` is `undefined` inside controllers           | `express.json()` is missing or placed after route mounting[cite: 14, 15].                                 | Place `app.use(express.json())` **above** all route declarations in `app.ts`[cite: 14, 15].                                       |
| Cookies are not saved / CORS error in browser          | `APP_URL` mismatch, or frontend fetch/axios is missing `credentials: "include"`[cite: 14, 15].            | Match `APP_URL` in `.env` to your frontend origin and enable `withCredentials: true` on the client[cite: 14, 15].                 |
| Database connection fails on startup                   | Invalid `DATABASE_URL` or `.env` not loaded properly[cite: 14, 15].                                       | Verify `.env` is in the project root and your PostgreSQL instance is active[cite: 14, 15].                                        |
| `npm start` fails with `ERR_MODULE_NOT_FOUND`          | `"moduleResolution": "bundler"` allows extensionless imports, which plain Node ESM rejects[cite: 14, 15]. | Use `tsx` in development, or bundle production builds using `tsup` (or switch to `NodeNext` with `.js` extensions)[cite: 14, 15]. |

---

## 24. Quick Setup Checklist

- [ ] Initialize Git (`git init`) and configure `.gitignore`[cite: 14, 15]
- [ ] Run Prisma 7 + TypeScript setup commands and add custom `tsconfig.json`[cite: 14, 15]
- [ ] Provision PostgreSQL database and add `DATABASE_URL` to `.env`[cite: 14, 15]
- [ ] Install Express, Auth, and TypeScript `@types/*` packages[cite: 4, 14, 15]
- [ ] Set up `src/app.ts`, `src/server.ts`, and `package.json` scripts[cite: 2, 4, 14, 15]
- [ ] Configure centralized environment variables in `src/config/index.ts`[cite: 2, 14, 15]
- [ ] Register global middlewares (`cors`, `json`, `urlencoded`, `cookieParser`)[cite: 2, 14, 15]
- [ ] Set up Multi-File Prisma Schema in `prisma/schema/` and update `prisma.config.ts`[cite: 5, 8, 9, 10, 11, 12, 13, 14, 15]
- [ ] Run `npx prisma db push` (or `migrate dev`) followed by `npx prisma generate`[cite: 14, 15]
- [ ] Configure `src/lib/prisma.ts` using `@prisma/adapter-pg`[cite: 2, 14, 15]
- [ ] Implement utilities (`catchAsync`, `sendResponse`, `jwtUtils`), modules (`user`, `auth`), and RBAC middleware (`auth.ts`)[cite: 2, 15]
- [ ] Run `npm run dev` and test endpoints in Postman ✅[cite: 14, 15]
