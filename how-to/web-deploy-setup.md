# Setting Up IIS and Visual Studio Team Services for Web Deploy

## Step 1: Create IIS Web Site

  ![][1.1]

  ![][1.2]

Port 80
Ignore Warning about Port 80
Set your folder path

## Step 2: Setup Bindings on Port 80

  ![][1.3]

Ensure the site is started

## Step 3: Set up Application Pool Identity to run as a service account

  ![][1.4]

  ![][1.6]

  ![][1.5]

## Step 4: Set up IIS Site for Web Deploy

  ![][1.7]

## Set up Web Deploy User under IIS Manager Permissions

  ![][1.11]

Allow Publish of Web Deploy

## Step 5: Set folder permissions  

* Ensure Web Deployer has full access

  ![][1.10]

* Ensure Service account has Read Access

  ![][1.12]

# Setting Up Your Web Project for Tokenization

## Step 1: Download pre-reqs
* Download Richard Fennell's Parameters.Xml Generator extension from Visual Studio's Extensions and Updates.

  ![][2.1]

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

  ![][2.2]

  ![][2.3]

  ![][2.4]
you need that will be tokenized on release builds with double underscores.  

# Setting up Team Services Build Definitions

* Create Build Definition - name it CI (for now - naming may change)
  * Ensure nuget restore step is there
  * Configure the Visual Studio build step like the below picture

  * Argument for MS Build:


      /p:DeployOnBuild=true /p:PublishProfile=tokenized /p:PackageLocation=$(build.stagingDirectory)
  * Configure repository

  ![][2.5]

  * Configure triggers for CI build

  ![][2.6]

  * Queue a build manually and confirm the artifacts downloaded: (picture below)
    NOTE: The main things to look for are:
    * ```<solution_name>.deploy.cmd```
    * ```<solution_name>.SetParameters.xml```
    * ```<solution_name>.<web_project_name>.zip```

    ![][2.7]

# Setting up Team Services Release Definitions

## Step 1: Create your environment(s)

## Step 2: Add Release Tasks
* Add the Replace Tokens task
  * Set "Root Directory" to ```$(System.DefaultWorkingDirectory)/<your linked artifact name>/drop```
  * Set Target Files to ```**\*.SetParameters.xml```

    IMPORTANT: Ensure in the advanced tab of the Replace Tokens task is
    using double underscores for prefix AND postfix.

    ![][2.8]

* Add the Batch Script Task
  * Set the Path to:

  ```$(System.DefaultWorkingDirectory)/<your linked artifact alias>/drop/<your_web_project>.deploy.cmd```

  * Set Arguments to: ```/Y -allowUntrusted $(DEPLOY_URL) $(AUTH)```.  
  DEPLOY_URL and AUTH are variables we will configure in the next step.  

## Step 3: Configure Environment Variables

Set the AUTH variable to ```/U:<Web_Deploy_UserName> /P:<Password> /A:Basic```

    IMPORTANT: Make sure you click the padlock on AUTH variable

The rest should look like (picture below):

All done!

<!-- Image References -->
[1.1]: resources/WebDeploySetupImages/IISSiteCreation1.png
[1.2]: resources/WebDeploySetupImages/IISSiteCreation2.png
[1.3]: resources/WebDeploySetupImages/IISSiteCreation3.png
[1.4]: resources/WebDeploySetupImages/Img4.png
[1.5]: resources/WebDeploySetupImages/Img5.png
[1.6]: resources/WebDeploySetupImages/Img6.png
[1.7]: resources/WebDeploySetupImages/Img7.png
[1.8]: resources/WebDeploySetupImages/Img8.png
[1.9]: resources/WebDeploySetupImages/Img9.png
[1.10]: resources/WebDeploySetupImages/Img10.png
[1.11]: resources/WebDeploySetupImages/Img11.png
[1.12]: resources/WebDeploySetupImages/Img12.png
[2.1]: resources/WebDeploySetupImages/Img13.png
[2.2]: resources/WebDeploySetupImages/Img14.png
[2.3]: resources/WebDeploySetupImages/Img15.png
[2.4]: resources/WebDeploySetupImages/Img17.png
[2.5]: resources/WebDeploySetupImages/Img16.png
[2.6]: resources/WebDeploySetupImages/Img18.png
[2.7]: resources/WebDeploySetupImages/Img19.png
[2.8]: resources/WebDeploySetupImages/Img20.png
[2.9]: resources/WebDeploySetupImages/Img21.png
[2.10]: resources/WebDeploySetupImages/Img22.png
[2.11]: resources/WebDeploySetupImages/Img23.png
[2.12]: resources/WebDeploySetupImages/Img24.png
