<!--- STARTEXCLUDE --->
# 🎓 Netflix Clone using AstraDB and GraphQL

[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/from-referrer/)
[![License Apache2](https://img.shields.io/hexpm/l/plug.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Discord](https://img.shields.io/discord/685554030159593522)](https://discord.com/widget?id=685554030159593522&theme=dark)

*50 minutes, Intermediate, [Start Building](#running-astra-tik-tok)*

A simple Netflix clone running on AstraDB that leverages the GraphQL API.
<!--- ENDEXCLUDE --->

![image](./img/ui.png)

## Objectives
* Deploy a Netflix clone "locally" and to production
* Learn how to use the @astrajs GraphQL API to retrieve pre-populated data
* Leverage Netlify and DataStax AstraDB

## ℹ️ Frequently asked questions ℹ️ 

- *Can I run the workshop on my computer?*
> There is nothing preventing you from running the workshop on your own machine. 
> If you do so, you will need
> * git installed on your local system
> * [node 15 and npm 7 or later](https://www.whitesourcesoftware.com/free-developer-tools/blog/update-node-js/)
>
> You will have to adapt commands and paths based on your environment and install the dependencies by yourself. **We won't provide support** to keep on track with schedule. However, we will do our best to give you the info you need to be successful.

- *What other prerequisites are there?*
> * You will need a github account
> * You will also need Netlify and AstraDB accounts, but we'll work through that in the exercises
> * Use **Chrome** or **Firefox** for the best experience. Other browsers are great, but don't work well with the GitPod integration we use a bit later.

- *Do I need to pay for anything for this workshop?*
> * No. All tools and services we provide here are FREE.

## Materials for the Session

It doesn't matter if you join our workshop live or you prefer to do at your own pace, we have you covered. In this repository, you'll find everything you need for this workshop:

- [Slide deck](./slides/slides.pdf)
- [Discord chat](https://bit.ly/cassandra-workshop)
- [Questions and Answers](https://community.datastax.com/)

## Homework

<img src="tutorial/images/netflix-badge.png?raw=true" width="200" align="right" />

Don't forget to complete your upgrade and get your verified skill badge! Finish and submit your homework!

1. Complete the practice steps from this repository as described below. Make screenshots alongside the steps and show us your deployed production Netlfix clone up in Netlify.
2. (Optional extra credit) Watch the 2 hour Ania video [HERE](#video-tutorial-with-ania-kubow), build the app yourself, and show us the completed app.
3. Submit your homework [here](https://github.com/datastaxdevs/workshop-astra-tik-tok/issues/new?assignees=HadesArchitect&labels=homework%2Cpending&template=homework-assignment.md&title=%5BHW%5D+%3CNAME%3E) (EDIT)

That's it, you are done! Expect an email next week!
  
# Let's start

## Table of contents

### Part 1 - DB Setup & Data Ingest
1. [Create AstraDB Instance](#2-create-astra-instance)
2. [Create a security token](#3-create-a-security-token)
3. [Create table **genre** with GraphQL](#4-create-table-genre-with-graphql)
4. [Insert data in **genre**  with GraphQL](#5-insert-data-in-the-table-with-graphql)
5. [Retrieve values of **genre** table](#6-retrieving-list-of-values)
6. [Create **movie** table](#7-creating-a-movies-table)
7. [Insert values in **movie** table](#8-insert-values-in-movie-table)
8. [Retrieve values from **movie** table](#9-retrieve-values-from-movie-tables)

###  Part 2 - Deploy to Production
1. [Deploy to Netlify](#2-deploy-to-netlify)
2. [Clone your GitHub repository](#3-clone-your-github-repository)
3. [Launch GitPod](#4-launch-gitpod-ide)
4. [Install the Netlify CLI](#5-install-the-netlify-cli-command-line-interface)
5. [Retrieve application token to securely connect to the database](#6-generate-application-token-to-securely-connect-to-the-database)
6. [Configure Environment Variables and Install Dependencies](#7-configure-and-connect-database)
7. [Launch your app](#8-launch-your-app)
8. [Connect Netlify to your site](#9-connect-netlify-to-your-site)
9. [Deploy to production](#10-deploy-to-production)

### Part 3 - Working with AstraDB and the GraphQL API
11. [Connecting the Database](#11-connecting-the-database)

### Extra resources
[What is JamStack?](jamstack.md)

[Video tutorial with Ania Kubow](#video-tutorial-with-ania-kubow)

# Part 1 - DB Setup & Data Ingest

## 1. Login or Register to AstraDB and create database

> *When creating your instance use the promotion code **ANIA200** to get 200$ of free credit allowing you about 30 million writes + 30 Million reads  + 50GB a month of monthly storage!!*


**`ASTRADB`** is the simplest way to run Cassandra with zero operations at all - just push the button and get your cluster. No credit card required, $25.00 USD credit every month, roughly 5M writes, 30M reads, 40GB storage monthly - sufficient to run small production workloads.  

✅ Click the button to login or register with Datastax. You can use your `Github`, `Google` accounts or register with an `email`.

_Make sure to chose a password with minimum 8 characters, containing upper and lowercase letters, at least one number and special character_

<a href="http://dtsx.io/netflix-clone"><img src="img/create_astra_db.png?raw=true" /></a>
- <details><summary>Show me!</summary>
    <img src="https://github.com/datastaxdevs/workshop-spring-stargate/raw/main/images/tutorials/astra-create-db.gif?raw=true" />
</details>

**Use the following values when creating the database**
|Field| Value|
|---|---|
|**database name**| `netflix_workshop_db` |
|**keypace**| `netflix_keyspace` |
|**Cloud Provider**| *Use the one you like, click a cloud provider logo,  pick an Area in the list and finally pick a region.* |

_You can technically use whatever you want and update the code to reflect the keyspace. This is really to get you on a happy path for the first run._

You will see your new database `pending` in the Dashboard.

![my-pic](https://github.com/datastaxdevs/shared-assets/blob/master/astra/dashboard-pending-1000-update.png?raw=true)

The status will change to `Active` when the database is ready, this will only take 2-3 minutes. You will also receive an email when it is ready.

[🏠 Back to Table of Contents](#table-of-content)


## 2. Create a security token

✅ [Create a token for your app](https://docs.datastax.com/en/astra/docs/manage-application-tokens.html) to use in the settings screen

Copy the token value (eg `AstraCS:KDfdKeNREyWQvDpDrBqwBsUB:ec80667c....`) in your clipboard and save the CSV this value would not be provided afterward.

**👁️ Expected output**
- <details><summary>Show me!</summary>
    <img src="img/astra-create-token.gif?raw=true" />
</details>

[🏠 Back to Table of Contents](#table-of-content)



## 3. Create table **genre** with GraphQL

✅ Open **GraphQL Playground** by 
1. Click on your active database
2. Click `Connect` TAB
3. Click `GRAPHQL API`
4. Clik link to you playground.

*as show on the picture below*
![image](img/open-playground.png?raw=true)


✅ Populate HTTP HEADER variable `x-cassandra-token` on the bottom of the page with your token as shown below

![image](img/graphql-playground.png?raw=true)

✅ In GraphQL Playground, create a table with the following mutation, making sure to replace `netflix_keyspace` if you used a different name:
```yaml
mutation {
  reference_list: createTable(
    keyspaceName:"netflix_keyspace",
    tableName:"reference_list",
    ifNotExists:true
    partitionKeys: [ 
      { name: "label", type: {basic: TEXT} }
    ]
    clusteringKeys: [
      { name: "value", type: {basic: TEXT}, order: "ASC" }
    ]
  )
}
```

[🏠 Back to Table of Contents](#table-of-content)

## 4. Insert data in the Table with GraphQL

✅ In graphQL playground, change tab to now use `graphql`. Edit the end of the URl to change from `system` to the name of your keyspace: `netflix_keyspace`

✅ Populate HTTP HEADER variable `x-cassandra-token` on the bottom of the page with your token as shown below (again !! yes this is not the same tab)

![image](img/graphql-playground-2.png?raw=true)

✅ In GraphQL Playground,populate the `reference_list` table with the following values:

```yaml
mutation insertGenres {
  action: insertreference_list(value: {label:"genre", value:"Action"}) {
    value{value}
  }
  anime: insertreference_list(value: {label:"genre", value:"Anime"}) {
     value{value}
  }
  award: insertreference_list(value: {label:"genre", value:"Award-Winning"}) {
     value{value}
  }
  children: insertreference_list(value: {label:"genre", value:"Children & Family"}) {
     value{value}
  }
  comedies: insertreference_list(value: {label:"genre", value:"Comedies"}) {
     value{value}
  }
  documentaries: insertreference_list(value: {label:"genre", value:"Documentaries"}) {
     value{value}
  }
  drama: insertreference_list(value: {label:"genre", value:"Dramas"}) {
     value{value}
  }
  fantasy: insertreference_list(value: {label:"genre", value:"Fantasy"}) {
     value{value}
  }
  french: insertreference_list(value: {label:"genre", value:"French"}) {
     value{value}
  }
  horror: insertreference_list(value: {label:"genre", value:"Horror"}) {
     value{value}
  }
  independent: insertreference_list(value: {label:"genre", value:"Independent"}) {
     value{value}
  }
  music: insertreference_list(value: {label:"genre", value:"Music & Musicals"}) {
     value{value}
  }
  romance: insertreference_list(value: {label:"genre", value:"Romance"}) {
     value{value}
  }
  scifi: insertreference_list(value: {label:"genre", value:"Sci-Fi"}) {
     value{value}
  }
  thriller: insertreference_list(value: {label:"genre", value:"Thriller"}) {
     value{value}
  }  
}
```

[🏠 Back to Table of Contents](#table-of-content)

## 5. Retrieving list of values

✅ In GraphQL Playground, not changing tab (yeah) list values from the table with the following command.

```yaml
query getAllGenre {
    reference_list (value: {label:"genre"}) {
      values {
      	value
      }
    }
}
```

*👁️ Expected output*
![image](img/graphql-playground-3.png?raw=true)

[🏠 Back to Table of Contents](#table-of-content)

## 6. Creating a Movies Table

✅ Move to tab `GRAPHQL-SCHEMA`, everything should be set, use the following mutation to create a new table:

```yaml
mutation {
  movies_by_genre: createTable(
    keyspaceName:"netflix_keyspace",
    tableName:"movies_by_genre",
    ifNotExists: true,
    partitionKeys: [
      { name: "genre", type: {basic: TEXT} }
    ]
    clusteringKeys: [ 
      { name: "year", type: {basic: INT}, order: "DESC" },
      { name: "title", type: {basic: TEXT}, order: "ASC" }
    ]
    values: [
      { name: "synopsis", type: {basic: TEXT} },
      { name: "duration", type: {basic: INT} },
      { name: "thumbnail", type: {basic: TEXT} }
    ]
  )
}
```

*👁️ Expected output*
![image](img/graphql-playground-4.png?raw=true)

[🏠 Back to Table of Contents](#table-of-content)

## 7. Insert Values in Movie table

✅ Move to tab `GRAPHQL`, everything should be set, use the following mutation to populate movies table: 

```yaml
mutation insertMovies {
  inception: insertmovies_by_genre(
    value: { 
      genre:"Sci-Fi", 
      year:2010,
      title:"Inception",
      synopsis:"Cobb steals information from his targets by entering their dreams.",
      duration:121,
      thumbnail:"https://i.imgur.com/RPa4UdO.mp4"}) {
    value{title}
    }
  
  prometheus: insertmovies_by_genre(value: { 
      genre:"Sci-Fi", 
      year:2012,
      title:"Prometheus",
      synopsis:"After a clue to mankind's origins is discovered, explorers are sent to the darkest corner of the universe.",
      duration:134,
      thumbnail:"https://i.imgur.com/L8k6Bau.mp4"}) {
    value{title}
    }
  
  	aliens: insertmovies_by_genre(value: { 
      genre:"Sci-Fi", 
      year:1986,
      title:"Aliens",
      synopsis:"Ellen Ripley is sent back to the planet LV-426 to establish contact with a terraforming colony.",
      duration:134,
      thumbnail:"https://i.imgur.com/QvkrnyZ.mp4"}) {
    value{title}
    }
  
    bladeRunner: insertmovies_by_genre(value: { 
      genre:"Sci-Fi", 
      year:1982,
      title:"Blade Runner",
      synopsis:"Young Blade Runner K's discovery of a long-buried secret leads him to track down former Blade Runner Rick Deckard.",
      duration:145,
      thumbnail:"https://i.imgur.com/xhhvmj1.mp4"}) {
    value{title}
    }
  }
```
> ℹ️ To get more movie data check the files in the `data` folder.

*👁️ Expected output*
![image](img/graphql-playground-5.png?raw=true)

[🏠 Back to Table of Contents](#table-of-content)

## 8. Retrieve values from Movie tables

✅ In GraphQL Playground, not changing tab (yeah) list values from the table with the following command.

```yaml
query getMovieAction {
    movies_by_genre (
      value: {genre:"Sci-Fi"},
       orderBy: [year_DESC]) {
      values {
      	year,
        title,
        duration,
        synopsis,
        thumbnail
      }
    }
}
```

*👁️ Expected output*
![image](img/graphql-playground-6.png?raw=true)


<br/>
<br/>
<br/>


# Part 2 - Deploy to Production

## 1. Deploy to Netlify
- <details><summary> What does the netlify deploy button do?</summary>The Netlify deploy button will:<ul>
    <li>Create a new repository for you on Github</li>
    <li>Create a site on Netlify</li>
    <li>Link the two together.</li></ul>
</details>

- Click the button to deploy

  [![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/RyanWelford/workshop-graphql-netflix)
 * <details><summary>Show me!</summary>
    <img src="tutorial/images/deploy-to-netlify.gif?raw=true" />
    </details>

This will take a few minutes.

  * Click on `Site deploy in progress` within the Netlify UI, 
    <details>
    <summary>Show me! </summary>
    <img src="tutorial/images/deploy-1.png" />
    </details>

  * Click the top deploy link to see the build process.
    <details>
    <summary>Show me! </summary>
    <img src="tutorial/images/deploy-2.png" />
    </details>

  * Wait until the build complete `Netlify Build Complete`,  **When you see Pushing to repository** you're ready to move on.
    <details>
    <summary>Show me! </summary>
    <img src="tutorial/images/deploy-3.png" />
    </details>

  * Scroll up to the top and click on the site name (it'll be after {yourlogin}'s Team next to the Netlify button).
    <details>
    <summary>Show me! </summary>
    <img src="tutorial/images/deploy-4.png" />
    </details>

## 2. Clone your GitHub repository

  * Click on the `GitHub` in `Deploys from GitHub` to get back to your new repository.  Scroll to where you were in the README.
    <details>
    <summary>Show me! </summary>
    <img src="tutorial/images/deploy-5.png" />
    </details>

## 3. Launch GitPod IDE
- Click the button to launch the GitPod IDE from **YOUR** repository.

* _Supported by <img src="tutorial/images/chrome-logo.svg" height="20"/> Chrome and <img src="tutorial/images/firefox-logo.svg" height="20"/> Firefox_

### WAIT! Before moving on ensure you are working out of YOUR repository, not the datastaxdevs repository.

![correct notcorrect](tutorial/images/correct-not-correct.png?raw=true)

If you are still using the `datastaxdevs` repo please ensure to follow the previous step, [step3](#3-clone-your-github-repository) to get to your repo.

 * Ok, I've got it, just give me the button already
 * <details>
     <summary>CLICK HERE to launch GitPod</summary>

     [![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/from-referrer/)
   </details>

## 4. Install the Netlify CLI (Command Line Interface)
 * In the `workshop-graphql-netflix` directory run the following command to install the netlify-cli
 ```
 npm install -g netlify-cli
```

 * <details><summary>Show me!</summary>
    <img src="tutorial/images/netlify-install-cli.png?raw=true" />
    </details>

## 5. Retrieve application token to securely connect to the database

Use the token you previously generated. If you no longer have the token and did not download a .csv, you can generate a new token using the instructions above

## 6. Configure Environment Variables and Install Dependencies

✅ Create `.env` file

```ini
ASTRA_DB_APPLICATION_TOKEN={ your_token }
ASTRA_GRAPHQL_ENDPOINT={ your_endpoint }
```

👩‍💻  Install all the packages

```bash
npm install
```

## 7. Launch your app
  * Run the application 
  ```
  netlify dev
  ```
  * The application should automatically launch in the GitPod preview pane

## 8. Connect Netlify to your site
Execute each of the commands below to link your code to your Netlify deployment.
  * First thing, we'll need to **STOP** the `netlify dev` command we issued a moment ago. In the terminal where you executed the netlify command issue a `CTRL-C` (control key + the C key) in order to stop the process.
  * Then continue with the following commands
  * This will pop up a browser to authenticate with netlify
  ```
  netlify login
  ```
  _Note, when using GitPod the preview pane will not display this properly. You must click the "open in a new window" button in the very top right of the preview pane._

  * This will link your workspace to the associated site
  ```
  netlify link
  ```

  * This will take the .env file created by astra-setup and upload it to netlify
  ```
  netlify env:import .env
  ```

<!--
  * Will be used to allow you to execute `netlify open`
  ```
  netlify sites:list
  ```
-->

## 9. Deploy to production
Now that you've hooked everything up, time to deplpoy to production.

  * Run
  ```
  netlify build
  ```

  * Then run
  ```
  netlify deploy --prod
  ```

  * Then finally run
  ```
  netlify open:site
  ```
  
  You've deployed your app to Netlify!
  ![Netlify Setup Example](./tutorial/images/prodDeploy.png?raw=true)

# Part 3 - Working with AstraDB and the GraphQL API

### Serverless configuration

Take a look at `netlify.toml`.

```
[build]
command = "npm run build"
functions = "functions"
publish = "build"
```

This is the configuration file we include in our codebase that tells Netlify how to build our app. In our case it's really simple. First we give the `build` command to build our app: `npm run build`. Then we tell Netlify where to find our serverless functions, and finally where to find the resulting app after build.

So Netlify will create endpoints for our serverless functions based on the files it finds in our functions folder.

✅ We can see these functions in our Netlify account.
- Go to netlify.com and sign in.
- Select your site from the list.
- Select the "Functions" tab at the top.

![netlify_functions](./tutorial/images/netlify_functions_tab.gif)

From here we can see all our functions and get direct links as well as watch real time logs.

![netlify_endpoint](./tutorial/images/netlify_endpoint_nav.gif)

# Extra resources

## Video tutorial with Ania Kubow
Thank you to our wonderful friend Ania Kubow for producing the Netflix clone. If you are not aware of Ania and love learning about coding you should absolutely check out her YouTube channel listed below.

While we focused on getting you up and running to production with AstraDB and Netlify, Ania's video will dig into more details on the app itself. Check it out to dig in more.

[Ania's Netflix Video](https://www.youtube.com/watch?v=g8COh40v2jU)
