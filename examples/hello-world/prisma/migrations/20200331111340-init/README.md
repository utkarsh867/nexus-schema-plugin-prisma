# Migration `20200331111340-init`

This migration has been generated by Flavian DESVERNE at 3/31/2020, 11:13:40 AM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "quaint"."User" (
    "birthDate" DATE NOT NULL  ,
    "email" TEXT NOT NULL  ,
    "id" TEXT NOT NULL  ,
    PRIMARY KEY ("id")
) 

CREATE TABLE "quaint"."Post" (
    "id" TEXT NOT NULL  ,
    PRIMARY KEY ("id")
) 

CREATE TABLE "quaint"."_PostToUser" (
    "A" TEXT NOT NULL  ,
    "B" TEXT NOT NULL  ,FOREIGN KEY ("A") REFERENCES "Post"("id") ON DELETE CASCADE ON UPDATE CASCADE,
FOREIGN KEY ("B") REFERENCES "User"("id") ON DELETE CASCADE ON UPDATE CASCADE
) 

CREATE UNIQUE INDEX "quaint"."User.email" ON "User"("email")

CREATE UNIQUE INDEX "quaint"."_PostToUser_AB_unique" ON "_PostToUser"("A","B")

CREATE  INDEX "quaint"."_PostToUser_B_index" ON "_PostToUser"("B")
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20200331111340-init
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,20 @@
+datasource db {
+  provider = "sqlite"
+  url      = "file:./dev.db"
+}
+
+generator prisma_client {
+  provider = "prisma-client-js"
+}
+
+model User {
+  id        String   @id @default(cuid())
+  email     String   @unique
+  birthDate DateTime
+  posts     Post[]
+}
+
+model Post {
+  id       String @id @default(cuid())
+  authors   User[] @relation(references: [id])
+}
```

