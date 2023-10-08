# Full Stack Authentication with Next-Auth and Next.js 13

-In this project, have 3 sessions ;

(1) Integrate Next-Auth with Next-Js 13
(2) Protect Pages with Next-Auth Middleware
(3) Protect API routes with JWT Access Token

# (1) Integrate Next-Auth with Next-Js 13 Session

## To build project

-First, install nextjs

```bash

npx create-next-app@latest

```

## To handle or catch all routes

-Have to use Next-Auth
-To use Next-Auth,have to install Next-Auth

```bash

npm install next-auth

```

## To create handler

have to create 'auth/[...nextauth]'(dynamic route handler ) folder to catch all routes

-/api/auth/signin
-/api/auth/signout

Note --> NextAuth မှာ Provider ကို CredentialsProvider သုံးထားတယ်
--> CredentialsProvider ရဲ့ အရေးကြီးဆုံးအပိုင်းက authorize() function ဖြစ်တယ်
--> authorize() function ထဲမှာ user ကို ရှာရန် logic ကို ထည့်ပေးရတယ်

## Using Prisma

Use the Prisma to set up a sqlite database

-first Install Prisma

```bash

npm install prisma --save-dev

```

-second Set up Prisma

```bash

npx prisma init --datasource-provider sqlite

```

-third run a migration to create your database tables with Prisma Migrate

```bash

npx prisma migrate dev --name init

```

-last Install Prisma client

```bash

npm install @prisma/client

```

-to view and edit the data in your database

```bash

npx prisma studio

```

## Creating login API

-create another route called login (/api/login)  
-inside it create route.ts file
-inside it create a post request for creating a login API

--> To hash the password, use bcrypt (a password-hashing function) <--

-to install bcrypt

```bash

npm i bcrypt

```

-to install types/bcrypt

```bash

npm install --save @types/bcrypt

```

Note --> login API ကို create လုပ်ပြီးသွားလျှင် authorize() function ထဲမှာ login API ကို သုံးလို့ရသွားပြီ
--> ထို့နောက် to create a post request to the login API in the authorize() function

## Creating user API

-create another route called user (/api/user)  
-inside it create route.ts file
-inside it create a post request for creating a user

## Using Next Auth

-Next.js also has an Authentication package called Next Auth that makes it easy to add authentication to a Next.js app.

-#Why use next auth?#

-Next Auth is a secure authentication system for Next.js applications.
-It is based on JSON Web Tokens (JWT) and provides authentication and authorization.

-#How to set up Next Auth?#

-need to create a file called route.js in the app/api/auth/[...nextauth] directory ( dynamic route handler for NextAuth)

Note --> to keep or store the current authenticated user data, use session
--> to share session object across components or to get the login state of our users and render user details on the frontend of our app, need to wrap around our components with SessionProvider

## Creating a Sign In Button component

-to check user login state and obtain session information, use the useSession() Hook (a React Hook)
-if a session exists, conditionally render the signin and signout links

Note --> useSession() returns an object containing two values: data and status

--data--
--> data: this can be three values: Session / undefined / null

--Session--
in case of success, data will be Session.

--undefined--
when the session hasn't been fetched yet, data will be undefined

--null--
in case it failed to retrieve the session, data will be null

--status--
--> status: enum mapping to three possible session states: "loading" | "authenticated" | "unauthenticated"

## Session | Session callback

-The session callback is called whenever a session is checked.

-It can have after the providers list.

-Inside it, have to set database or JWT.

Note --> When using database sessions, the User (user) object is passed as an argument.
--> When using JSON Web Tokens for sessions, the JWT payload (token) is provided instead.

-if you use JWT or database sessions;

--> token: the JWT token for this session
--> session: the session object from your adapter

## Fixing the next-auth no secret warning

-To fix the next-auth no secret warning, have to create NEXTAUTH_URL(eg: http://localhost:3000) and NEXTAUTH_SECRET(eg: lsdkmlskdmflksdkskmsdnkj) on .env

Note --> the URL is your domain and the secret is any hash

# (2) Protect Pages with Next-Auth Middleware Session

## Creating UserPost Page

-Create another page called UserPost (/src/app/UserPost/), inside it create page.tsx file

## Protecting Pages

-Use Middleware to protect pages that can access only authenticated user

-To use Middleware, create a middleware file side by side of the app in the src directory

-Inside it, add the URL of the pages ("matcher: ["/UserPost/:path*"]") which want to protect from the unauthenticated user

## To fix in order to secure the application

-Create another API for users, so create a dynamic route inside the user (/api/user/[id]) ,inside it create route.ts file

Note --> This API is a GET request.

# (3) Protect API routes with JWT Access Token Session

-To protect API routes, Should create Access Token

-In order to create an Access Token, use a library called JWT (JSON Web Token)

-Before using, need to install JWT

```bash

npm i jsonwebtoken

npm install --save @types/jsonwebtoken

```

-In here, do this 3 steps to protect API routes;

              --Step-1--

Create a JWT Access Token in login API

-To use JWT, create a file called jwt.ts in the components, inside it need two functions;

(1) the first one is for creating or signing a JWT token

Note --> This function takes two arguments (payload , options)
--> And it needs a secret key to create a JWT token which can define it on .env
--> In this function, use sign() method

(2) the second one is for verifying a JWT token

Note --> In this function, use verify() method

            --Step-2--

Save the Access Token inside the Next-Auth Session

-To store the access token inside the Next-Auth, define two functions

(1) A JWT function

Note --> A user object and a token object combine into one object, and returns one object as JWT

(2) A Session function

Note --> It has an object as its parameter and inside this object takes session and token

           --Step-3--

Require the Client to have Access Token in the header of its request

## Getting Started

First, run the development server:

```bash

npm run dev

```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.
