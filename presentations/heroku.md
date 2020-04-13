# Deploy APIs (Heroku) 

## Notes

* [Getting Started on Heroku with Java](https://devcenter.heroku.com/articles/getting-started-with-java)
* [Deploying Java Apps on Heroku](https://devcenter.heroku.com/articles/deploying-java)

### 1. Introduction 
This tutorial will have you deploying a Java app in minutes, so that it will be globally accessible instead of just on localhost.

-----------------------------------------------------------------------------------------------


## Lab 1 Heroku Deployment

### step 1 - Create an Account
Create  a [Free Heroku App Account](https://signup.heroku.com/login)

### step 2 - Create an new Heroku App
1. Navigate to the [Heroku Dashboard](https://dashboard.heroku.com/apps) and create a new App.
![Create a New App]( /presentations/images/new-heroku-app.png)

2. Provide a unique app name and leave all othe things default and click on `create app`
![Create a New App Form]( /presentations/images/new-app_1.png)

### step 3 - Deploy with Github
1. Heroku provides three method for deployment but we'll just stick to GitHub Deployment for now.
![Deployment Methods]( /presentations/images/deployment-methods.png)

2. Select GitHub, Search for repository and click the `connect` buttom beside it.
![Connect to repo]( /presentations/images/search-repo.png)

3. In the Manual Deployment section, select the branch you want to deploy and click on the  `Deploy Branch`
![Manual Deployment]( /presentations/images/manual-deployment.png)

4. If the deployment is successful it will be display on the page together with a link to visit the app.
![Sucessful Deployment]( /presentations/images/success-deploy.png)

5. New app url. Mine is [javarestfulapi.herokuapp.com]( https://javarestfulapi.herokuapp.com/swagger-ui.html)
![App url]( /presentations/images/deploy-url.png)

### step 4 - CI/CD
Instead of manually pushing the Deploy button each time we push our code to GitHub, We can set it such that whenever we push to a particular GitHub branch it should trigger a build for us automatically.
![CI/CD ]( /presentations/images/ci.png)