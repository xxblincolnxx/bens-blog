+++
date = '2025-03-06T14:09:56-05:00'
draft = false
author = 'Ben Prothe'
title = 'New Project: Picking my Backend'
series = "Final Project Rebuild"
tags = ["Reference", "Project Update", "Reflection"]
+++

Today I’ll talk about the first step I’ve taken towards my new pet project: a total rebuild of my bootcamp final project. My team of 3 completed this project in the span of 2 weeks at the very beginning of the 2020 pandemic… as in, we had been going to classes in person, then we entered quarantine just as we got our team assignments and completed everything remotely.

At the time we had only been writing code for 10 weeks. In our toolkit we had only vanilla JS + HTML + CSS for frontend, and Django and Python (notably no SQL) for backend. Our team bravely took on learning parts of Vue.js to help us build our UI. The backend was also a departure from what we knew. We used the Django REST framework so we could fetch our data instead of using the out of box Model-View-Template structure to pass data to our views.

I’m so incredibly proud of our team and our accomplishments, especially considering our challenges; both self imposed and the literal force majeure that was the 2020 pandemic. Today I have the luxury of having no 2-week deadline and 5 years of writing code under my belt. I’m looking forward to seeing what I can build now.

## Roadmap

This is the part of a project that I really, really love: the **research phase.** I get to pick the technologies that I’m going to use to start. Here’s where I’m at for backend (FrontEnd is still TBD):

1. Creating a stand-alone json API instead of using Next.js API features
2. Primary Tech: NestJS + Drizzle + Postgresql
3. Auth: Passport.js (has a NestJS recipe) with Local/JWT Strategy

### Where’s Django?

I can read Python and I can write it pretty well after some refreshers. But at this point I know JavaScript and TypeScript so much better and I am just more comfortable developing in a Node environment. I also grew to enjoy the style and organization of Java:Spring APIs with DTOs, services, controllers, etc. NestJS follows extremely similar patterns so I’m right at home. I like Django’s template system far less.

### Why I chose Drizzle?

{{< rawhtml >}}

<iframe style="width:100%; height: 350px;overflow:auto;" src="https://www.youtube.com/embed/bpGvVI7NM_k?si=E-hOyqN59_cVDmR5" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
{{< /rawhtml >}}

[DONT USE AN ORM | Prime Reacts](https://youtu.be/bpGvVI7NM_k?si=r3dvR0ViLxCA6E72)

I’ve spent quite a bit of time ruminating on this decision. This video and corresponding article perfectly explain the kind of guidance I’ve encountered. “Write your own solution using SQL” seems to be the _best_ option, according to people who sound the most competent (great measure, I know). Lots of people say Drizzle is the most flexible/closest to option 1 while still doing quite a bit for you under the hood. Prism seems to be most commonly used and has great startup time compared with the other two. But I’ve heard rumblings of Prism’s package size being huge and using inefficient queries behind the curtains. So here’s how I’ve come to think of my 3 main options:

| Tech       | Ease of Development?                                           | Scales Nicely?                                                                                                                                                               | Confidence in End Result?                                                                                                                   | Endorsements from Respected Devs |
| :--------- | :------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------ | :------------------------------- |
| BYO w/ SQL | 1/10 - Will take significant time and research to do right.    | 10/10 - Biggest Strength                                                                                                                                                     | 5/10 - I’m not a backend expert. So it would be a big lift to make sure I’m using scalable patterns.                                        | +5                               |
| Drizzle    | 8/10 - More setup than Prisma but still gives you quite a bit. | 8/10 ???? Big question mark. It’s meant to be lean and provide enough control over queries to avoid those performance problems. I’ll have to check back when I find a hitch. | 9/10 - I have gaps in my SQL playbook so I might not be able to take the greatest advantage of Drizzle’s granularity… but that’s about it.. | +2                               |
| Prisma     | 10/10 - Touted for its DX and development speed                | 5/10 - It seems this is the drawback of this ORM                                                                                                                             | 9/10 - Not counting the inherent issues with Prisma, I think it would be pretty easy to spin something up that covers my bases.             | -2                               |

So, I picked Drizzle. This is partially going to be a bit of an experiment to see if I run into some of the issues described in the video while building out my new project with Drizzle. It will be my personal stress test. I’ll be sure to keep this blog up to date as I encounter challenges. So anyways, here’s how you setup Nest with Drizzle and Postgres:

## Setup

### Before you start

1. Nest CLI installed globally then create a new project like so:

   - `nest new <app-name>`

2. Get Postgres container running w/docker-compose.yaml (see example in code)

   - Run with: `docker-compose up`
   - PRO TIP: If you have any typos in the database url or compose file, you’ll have to delete the container and run the compose command again to get a new db with the proper creds configured.

3. Install the following to get Drizzle module on board:

   - `npm install drizzle-orm pg`
   - `npm install -D drizzle-kit @types/pg`
   - `npm install @nestjs/config`

### Development Loop

#### Do the following once at setup:

- Create drizzle.config.ts
- Add `ConfigModule.forRoot({ isGlobal: true })` to the “imports” array in `app.module.ts` - This allows us to access environment variables.
- Create and configure new module for the database:
  - Run: `nest g module database`
  - Inside database folder add a constants file and define/export “DATABASE_CONNECTION” = “database_connection” (just a convention)
  - The database module file needs to look like the following:

```ts
import { Module } from '@nestjs/common';
import { drizzle } from 'drizzle-orm/node-postgres';
import { DATABASE_CONNECTION } from './database-connection';
import { ConfigService } from '@nestjs/config';
import { Pool } from 'pg';
import * as usersSchema from '../users/schema';

@Module({
  providers: [
    {
      provide: DATABASE_CONNECTION,
      useFactory: (configService: ConfigService) => {
        const pool = new Pool({
          connectionString: configService.getOrThrow<string>('DATABASE_URL'),
        });
        return drizzle(pool, {
          schema: {
            ...usersSchema,
          },
        });
      },
      inject: [ConfigService],
    },
  ],
  exports: [DATABASE_CONNECTION],
})
export class DatabaseModule {}
```

#### For each database model:

1. Create or generate base files and folders for each model:
   - `nest g module <schema>` – This is the confluence file where the pieces are combined and consumed. Need to pass `imports: [DatabaseModule]` to `@Module({})` decorator but other args will vary from one model to next.
   - `nest g controller <schema>` – This is where we define our HTTP methods and handle params/subroutes.
   - `nest g service <schema>` – This is where we define methods that actually result in database queries using Drizzle methods.
2. Create and configure `schema.ts` \- This is where we define the Drizzle schema and any relations within the table.

   - Define table columns in pgTable()
   - Define relations with relations like so (one or many)(example has users and menu tables):

     ```ts
     export const menuRelations = relations(menus, ({ one }) => ({
       user: one(users, {
         fields: [menus.userId],
         references: [users.id],
       }),
     }));
     ```

3. Generate and apply migrations:
   - `npx drizzle-kit generate`
   - `npx drizzle-kit migrate`
4. Add the schema object to the database.module.ts drizzle schema.
5. Create dto folder and define types for each request body. Naming convention: `<crud-action>-<entity>.request.ts` (eg: **create-user.**request.ts)
6. Inside `<model>.service.ts`: Inject Database connection as an argument of the constructor as follows:

   ```ts
   constructor(
   @Inject(DATABASE_CONNECTION)
      private readonly database: NodePgDatabase<typeof schema>,
   ) {}
   ```

7. Implement supporting services first, then add endpoints for HTTP methods
8. Look at official documentation for the following:
   - Many-to-many relationships (you need to explicitly define a join table) (already noted)
   - Implementing filters
   - Transactions

## Up Next

Next I'll dive into my auth solution which is currently looking like it will be Passport.js.

## Sources

- **[What ORMs have taught me: just learn SQL](https://wozniak.ca/blog/2014/08/03/1/index.html?utm_source=tuicool&amp%3Butm_medium=referral)**
- **[Setting Up Drizzle ORM in a Nextjs App](https://refine.dev/blog/drizzle-react/#setting-up-drizzle-orm-in-a-nextjs-app)**
- **[NestJS + Drizzle Deep Dive | Schemas, Relations, Migrations & More](https://youtu.be/4xMDAqcwzp8?si=-hKcezzouR091DOH)**
- **[Drizzle Docs - PostgreSQL](https://orm.drizzle.team/docs/get-started-postgresql)**
- **[NestJS Docs](https://docs.nestjs.com/)**
