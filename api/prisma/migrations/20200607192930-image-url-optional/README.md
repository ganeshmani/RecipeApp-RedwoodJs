# Migration `20200607192930-image-url-optional`

This migration has been generated by karthick3018 at 6/7/2020, 7:29:30 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
PRAGMA foreign_keys=OFF;

CREATE TABLE "quaint"."new_Recipe" (
"description" TEXT NOT NULL  ,"id" INTEGER NOT NULL  PRIMARY KEY AUTOINCREMENT,"image_url" TEXT   ,"name" TEXT NOT NULL  )

INSERT INTO "quaint"."new_Recipe" ("description", "id", "image_url", "name") SELECT "description", "id", "image_url", "name" FROM "quaint"."Recipe"

PRAGMA foreign_keys=off;
DROP TABLE "quaint"."Recipe";;
PRAGMA foreign_keys=on

ALTER TABLE "quaint"."new_Recipe" RENAME TO "Recipe";

CREATE UNIQUE INDEX "quaint"."Recipe.name" ON "Recipe"("name")

PRAGMA "quaint".foreign_key_check;

PRAGMA foreign_keys=ON;
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20200607181742-create-table..20200607192930-image-url-optional
--- datamodel.dml
+++ datamodel.dml
@@ -1,7 +1,7 @@
 datasource DS {
   provider = "sqlite"
-  url = "***"
+  url      = env("DATABASE_URL")
 }
 generator client {
   provider      = "prisma-client-js"
@@ -9,11 +9,10 @@
 }
 // Define your own datamodels here and run `yarn redwood db save` to create
 // migrations for them.
-// TODO: Please remove the following example:
 model Recipe {
-  id          Int    @id @default(autoincrement())
-  name        String @unique
+  id          Int     @id @default(autoincrement())
+  name        String  @unique
   description String
-  image_url   String
+  image_url   String?
 }
```


