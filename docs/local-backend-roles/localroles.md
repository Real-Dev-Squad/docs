# Setting roles in your local backend

Please make sure that you have setup the backend locally. Watch these videos if you have not done that: 
- <a href="https://www.youtube.com/watch?v=haqPaPRrhPU">Real Dev Squad Backend Setup Part 1: GitHub OAuth and Downloading Firestore credentials json file</a>

- <a href="https://www.youtube.com/watch?v=2Ja5hH2nH1o&feature=youtu.be">Real Dev Squad Backend Setup Part 2: Firestore</a>

## Why we need to step up roles in our local backend?
- There might be a certain feature that you are building which requires admin or super_user role in order to perform a particular action.
- Let's take an example: suppose you are working on a feature which allows the super_user to promote a user of Real Dev Squad to member, for that you need to hit `/moveToMembers/:username` endpoint, it uses a authorizeUser middleware to check if the user has a super_user role. For that you need to use your local backend and set up the required roles in firebase to provide super_user role to yourself.

## Steps: 

1. Make sure that your backend server is running. To run the server run `yarn dev` in your project directory. This will run your server on localhost:3000

2. The RDS backend hits `/users/self` endpoint to get the data of the user.

3. So let's try to acces our details by going to http://localhost:3000/users/self. This will give a 401 error, to solve this follow the step below.

4. Copy this URL `https://github.com/login/oauth/authorize?client_id=<your-client-id>` and paste it somewhere, now go to your  `local.js` file and copy the clientId that is present there, and paste it in place of `<your-client-id>` in the URL provided above.

5. This will redirect you to `http://localhost:3000/healthcheck`, if you go to your firebase console, you will notice that a `doc` has been created with your userId in the `users` collection in your firebase, now try going to http://localhost:3000/users/self. you can see those details.

6. You can see that there is no roles property present in the object that is returned. we will update the doc that contains our details with the roles. Click on add fields and set the properties as shown below: 

![Firebase Image](/public/assets/firebase-image.jpg)

7. Now click on add, and now if you go to http://localhost:3000/users/self, you will be able to see the roles.

8. Now you will be able to perform actions which require admin or super_user roles in your local backend.