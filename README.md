# GitHub Exercise
 - Pleas NOTE: These instructions maybe updated, please check the README on GitHub for the most up-to-date instructions.

 The GitHub API is documented here: https://docs.github.com/en/rest

 We will be connecting to this API to create a version of 'GitHub'.
 You should clone this repo (ssh link: git@github.com:robynHM/githubTutorial.git).
 We want to track changes to the project using git. Since we cloned this repo we are going to change the remote origin.

 * Go to your GitHub account → Your repositories → Create new repository 
   * Give it the same name as your local project 
   * Ensure it is public 
   * Ensure 'Initialise with README' is unticked 
 * Go to the command line/terminal:
   * We need to tell git where the remote GitHub repository is, so that we can push up changes. Run the following, substituting with your GitHub username and newly created project name 
   * `git remote rename origin upstream` (The old origin is now renamed to upstream)
   * `git remote add origin git@github.com:<username>/<repo-name>.git` (A new origin is added, your github!)
   * e.g. `git remote add origin git@github.com:vinniebrice/play-template.git`
 
 * Run `git remote -v` to check the remote is correct

 * Run `git status`, and you can now see there are various files git has picked up but is not yet tracking any changes and only recognises a whole bunch of new folders and files.

 * Run `git add .` to add all files in red to a 'staging area' in preparation for a final commit

 * To make a commit, run `git commit -m "example message"` (Think of an appropriate message for the changes you've made)

 * Push your new commit up to main `git push origin main` (Note: there are two places we 'can' push to, the old location, upstream, and your new one, origin.)

 * Go to your new project on GitHub to check everything pushed up okay.
 
 * In terminal type `sbt run`, in your browser on localhost:9000 you should see a page with the title "Welcome to your version of GitHub".
 
  For package structure use standard: 

* views
  * This contains the html templates which are the pages that are viewable in the browser.
* controller 
    * API endpoint entry
    * Responsible for displaying views files
    * Interacts with the service layer

  Then make folders:
* service 
  * Orchestration, should be lightweight seeing as we're only calling one endpoint
  * Service should contain all the logic and checks we want to happen. e.g. is the data that is submitted to be added into database the correct format
* connector 
  * To send requests to external services
* model 
  * Domain model data representation
* repository
    * This contains the methods to interact with your database


 Using the GitHub API you come across rate limits, if this occurs it can be easier if upfront you use your own git credentials for authorisation - https://developer.github.com/v3/auth/#basic-authentication (you may not need to worry about authorisation yet and can come back to this once you get to task 9 as authorisation is needed when you start manipulating repositories on the GitHub API.)

 When writing your code only have working code on your main branch and for each new task create a branch to create this. 

 This is great practice as if we push directly to main we could potentially be breaking the code or cause merge conflicts. 
 We can make commits in an isolated branch that will not affect the code in main which should only ever be working code.

 When the task is completed, you have written the corresponding tests, and they are passing create a pull request to merge your branch into main.

 Make sure you are practicing test driven development by writing integration and unit tests for all your methods after you have written them.
 
## Task 1.
 Create your package structure with the corresponding files (e.g. controller class...).
 Your mongodb should contain users which will have the following information: username, date account created, location, number of followers, number following.
    Look at the return of a user from the GitHub API to help you make your model of a user. Try inputting your GitHub username into the url to see: https://api.github.com/users/{yourUsername}
 
 Set up your mongodb database and a model of the user it will accept.
 When adding users to the database make sure there cannot be duplicate usernames inside the database.
 
 ## Task 2.
 Create a route that will return a user from the GitHub API with the route:
 {url}/github/users/{username}

 When somebody accesses your app at the above url, you should display the user on a views page.
 Use this connector to do this:
 https://api.github.com/users/{username}

 ## Task 3.
 Make routes for all the CRUD commands (Create Read Update Delete)
 
 ## Task 4.
  Make a route and add other methods to add a user from the GitHub API to your repository.
 
 ## Task 5.
  Make a  new route: /github/users/{userName}/repositories That will list the names of all public repositories for the provided username
  Make use of the GitHub API: https://api.github.com/users/{username}/repos
  Once this page exists, add a link “Repositories” to the page created in Task 2, so that as a website user you can navigate from somebodies profile to their list of repositories.
 
## Task 6.
 Add a new route /github/users/:username/repos/:repoName
 On this page display the names of files & folders (as determined by an API call) for the selected usernames’ repository.
 
 ## Task 7.
  Make a new route GET /github/users/:username/repos/:repoName/*path
  The path will be path to file I.E repoName/folderName/fileName.scala, you can base64 encode path for ease.
  If the path given resolves to a file, display the plaintext of the file on the page.
  If the path given is a folder, display the files & folder names (In the same way it's displayed in the first part of Task 5)
  
## Task 8.
 Link up the pages. I.E the first step of task 5 doesn't only list the files/folders, but they are hyperlinks that will direct to the correct page in the  stretch exercise.
 For example, /github/users/jxr227/repos/ROScratch, will show various files and folders including the folder "exercises". If you click "exercises" you  will be directed to:
 /github/users/jxr227/repos/ROScratch/exercises
 
 Do the same on the /*path pages, so that you can, from the top level of a repository, browse all folders and files.

## Task 9.
 When it comes to unit testing your connector (the file using ws client that connects to the API) you can use wiremock to mock the API responses. 
 
 To use wiremock in your library dependencies in the build.sbt file add:
    "com.github.tomakehurst" % "wiremock-jre8" % "2.33.2" % Test

 If you run test if you get an error about jackson databind after adding the wiremock try adding this to your build.sbt file:
 dependencyOverrides +="com.fasterxml.jackson.core" % "jackson-databind" % "2.11.0"

## Task 10.
 Now try and add methods to get, create, update and delete files in a repository on the GitHub API (not on your database data).
 Follow this link for instructions and work your way through each of the steps:
 https://docs.github.com/en/rest/repos/contents?apiVersion=2022-11-28
 You will need a personal access token to make changes in the API. This token should NOT be hardcoded into your code, as it should be for your eyes only. Try storing it as a permanent environment variable called AuthPassword, e.g. export AuthPassword= yourToken.
 For details on how to create an access token see: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token
 Made curl requests in terminal for these routes and write tests to check they are all working.

## Task 11.
 Now that you have accomplished step 10 instead of using curl requests in terminal try creating a form, so you can complete these routes from your browser.

## Task 12.
 Add some css styling to the layout of your pages. 
 For example change the font, change the layout of the text, add some colour, add a back button. How it looks is up to you.

