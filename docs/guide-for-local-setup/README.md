# This is a document that describes how to run the RDS app locally with a local backend

## 1. Setup the Backend

- Go to the RDS Backend setup guide [here](https://github.com/Real-Dev-Squad/website-backend/#readme)
- Add the `cors` config in `config/local.js` file:

```javascript
 cors: {
     allowedOrigins: /(http:\/\/localhost:[0-9]{4}$)/,
 },
```

## 2. Setup the Frontend -

- Go to the README of the respective frontend repo.
- Change all the API endpoints to `http://<your local api>:<port>/`
  - RDS API endpoint: `https://api.realdevsquad.com/`
  - Default API endpoint[local]: `http://localhost:3000/`
- Change all the URL with respective frontend URL
  - Like you want to run WWW and My site then you have to set `http://localhost:5500` for WWW(main Site) then change all the URL from `https://www.realdevsquad.com` to `http://localhost:5500` in both the repo and if you set `http://localhost:3443` for My site then you have to change all the URL from `https://my.realdevsquad.com` to `http://localhost:3443` in both the repo and so on.

## 3. To skip the signup process

- After authenticated, add some data to the database
      - `username` - `<your user name>` of the user
      - `first_name` - `your first name` of the user
- Also change the `incompleteUserDetails` to `false` in the database

## 4. Add roles to the user like Super User, Admin, Member

Some of the features are only available to users with specific roles. For adding roles to the user, you can add the roles to the `roles` map in the database.

- For Super User, add `super_user` with the boolean value `true` in the `roles` map.
- For Admin, add `admin` with the boolean value `true` in the `roles` map.
- For Member, add `member` with the boolean value `true` in the `roles` map.

**Note:** For register your app for OAuth, go to URL `http://<app url>/auth/github/callback` and authorize the app.
