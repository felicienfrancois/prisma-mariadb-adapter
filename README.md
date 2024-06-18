# @felicienfrancois/prisma-mariadb-adapter

This package contains the driver adapter for Prisma ORM that enables usage of the [`mariadb-connector-nodejs`](https://github.com/mariadb-corporation/mariadb-connector-nodejs) (`mariadb`) database driver for MySQL and MariaDB.

`mariadb` is one of the most popular drivers in the JavaScript ecosystem for MySQL and MariaDB databases. It can be used with any MySQL or MariaDB database that's accessed via TCP.

> **Note:** Support for the `mariadb` driver is available from Prisma versions [5.4.0](https://github.com/prisma/prisma/releases/tag/5.4.0) and later.

## Usage

This section explains how you can use it with Prisma ORM and the `@felicienfrancois/prisma-mariadb-adapter` driver adapter. Be sure that the `DATABASE_URL` environment variable is set to your MySQL connection string (e.g. in a `.env` file).

### 1. Enable the `driverAdapters` Preview feature flag

Since driver adapters are currently in [Preview](https://www.prisma.io/docs/orm/more/releases#preview), you need to enable its feature flag on the `datasource` block in your Prisma schema:

```prisma
// schema.prisma
generator client {
  provider        = "prisma-client-js"
  previewFeatures = ["driverAdapters"]
}

datasource db {
  provider = "mariadb"
  url      = env("DATABASE_URL")
}
```

Once you have added the feature flag to your schema, re-generate Prisma Client:

```
npx prisma generate
```

### 2. Install the dependencies

Next, install the `mariadb` package and our driver adapter:

```
npm install mariadb
npm install @felicienfrancois/prisma-mariadb-adapter
```

### 3. Instantiate Prisma Client using the driver adapter

Finally, when you instantiate Prisma Client, you need to pass an instance of our driver adapter to the `PrismaClient` constructor:

```ts
import { createPool } from "mariadb/promise";
import { PrismaMariaDB } from "@felicienfrancois/prisma-mariadb-adapter";
import { PrismaClient } from "@prisma/client";

const connectionString = `${process.env.DATABASE_URL}`;

const pool = createPool(connectionString);
const adapter = new PrismaMariaDB(pool);
const prisma = new PrismaClient({ adapter });
```

## Feedback

This is an **unofficial** driver adapter until Prisma makes an official driver adapter for MySQL and MariaDB.

If you find something missing or run into a bug, please create an issue.
