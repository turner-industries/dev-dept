# SETTING UP IIS FOR WEB Deploy

## Step 1: Create IIS Web Site

Port 80
Ignore Warning about Port 80
Set your folder path

## Step 2: Setup Bindings on Port 80

Ensure the site is started

## Step 3: Set up Application Pool Identity to run as a service account

## Step 4: Set up IIS Site for Web Deploy

## Set up Web Deploy User under IIS Manager Permissions

   Allow Publish of Web Deploy

## Step 5: Set folder permissions  

* Ensure Web Deployer has full access
* Ensure Service account has Read Acess

# Setting Up Your Web Project for Tokenization

## Step 1: Download pre-reqs
* Download Richard Fennell's Parameters.Xml Generator extension from Visual Studio's Extensions and Updates.

## Step 2: Tokenize the areas of your web.config

* Right-click the Web.config file and select the GenerateParameters.xml file
option.  This will create a Parameters.xml file in your project

* Add a Web.Base.config file at the same level as your Web.config file.  Copy
contents of Web.config into Web.Base.config

* Ensure the following:

```xml
    <Target Name="BeforeBuild" Condition="'$(NCrunch)' != '1'">
      <TransformXml Source="Web.Base.config" Transform="Web.$(Configuration).config" Destination="Web.config" />
    </Target>
  ```
* Create Web Deploy Package

you need that will be tokenized on release builds with double underscores (__).  

# Setting up Team Services Build Definitions

* Create Build Definition - name it CI (for now - naming may change)
  * Ensure nuget restore step is there
  * Configure the Visual Studio build step like the below picture

  * Argument for MS Build:


      /p:DeployOnBuild=true /p:PublishProfile=tokenized /p:PackageLocation=$(build.stagingDirectory)
  * Configure repository

  * Configure triggers for CI build

  * Queue a build manually and confirm the artifacts downloaded: (picture below)
    NOTE: The main things to look for are:
    * <solution_name>.deploy.cmd
    * <solution_name>.SetParameters.xml
    * <solution_name>.<web_project_name>.zip

# Setting up Team Services Release Definitions

## Step 1: Create your environment(s)

## Step 2: Add Release Tasks
* Add the Replace Tokens task
  * Set "Root Directory" to ```$(System.DefaultWorkingDirectory)/<your linked artifact name>/drop```
  * Set Target Files to ```**\*.SetParameters.xml```


    IMPORTANT: Ensure in the advanced tab of the Replace Tokens task is
    using double underscores (__) for prefix AND postfix.

* Add the Batch Script Task
  * Set the Path to:

  ```$(System.DefaultWorkingDirectory)/<your linked artifact alias>/drop/<your_web_project>.deploy.cmd```

  * Set Arguments to: ```/Y -allowUntrusted $(DEPLOY_URL) $(AUTH)```.  
  DEPLOY_URL and AUTH are variables we will configure in the next step.

## Step 3: Configure Environment Variables

Set the AUTH variable to ```/U:<Web_Deploy_UserName> /P:<Password> /A:Basic```

    IMPORTANT: Make sure you click the padlock on AUTH variable

The rest should look like (picture below):
