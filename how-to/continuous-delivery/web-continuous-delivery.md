# Setting Up On-Prem Continuous Integration and Delivery for a Web Project

#### Summary
* This guide explains how to set up a website for continuous integration and delivery to Turner's on-prem servers
* When code is checked in to the repository, VSTS will automatically run the continuous integration build. When completed, Octopus Deploy will release the build to the test environment.

#### Pre-reqs
* This guide assumes you have a Visual Studio Team Services project created already

---

## Step 1: Set Up Continuous Integration Using Visual Studio Team Services

### Setup Service Endpoint for Octopus Deploy
1. Navigate to the settings for your VSTS project
2. Go to "Services"
3. Add a "New Service Endpoint" => "Generic"
4. Name the connection "Octopus Server"
5. The server URL is `http://msft-build-prod/`
6. The username can be blank
7. Copy the "Octopus API Key" from Turner's shared LastPass folder

    ![Connect Octopus Server][octopus-connection]

### Create a New Build Definition
1. Navigate to your VSTS project
2. Click on "Build & Release"
3. Click "New Definition"

    ![Navigate to Project][navigate-to-project]
4. Choose "Empty" from the template list
5. Select a repository (from VSTS, GitHub, etc.)
6. Select the "Continuous Integration" checkbox
7. Make sure the "Default agent queue" is "Default"

    ![New Build Definition][new-from-definition]

### Add Your NPM Commands
1. Add as many npm commands as you need to get your modules installed, tested, and packed.

    ![Add NPM Steps][npm-packages]

### Add Copy Files Step
1. Add the "Copy Files" step from the task catalogue under the "Utility" tab

    ![Add Copy Files Step][copy-files]
2. Under the step's settings, set the "Source Folder" to the directory that your web project resides in.
3. Set the "Contents" text box to the files you'd like to include (or exclude). Here's a short example:

    ```
    **\**
    !**\.vscode\**
    !**\coverage\**
    !**\node_modules\**
    !**\semantic\**
    !**\src\**
    !**\styles\**
    !**\typings\**
    ```
4. Set the "Target Folder" to "`$(build.artifactstagingdirectory)`"

    ![Set Copy Files Settings][copy-files-settings]

### Add Package Application Step
1. Add the "Package Application" step from the task catalogue under the "Package" tab

    ![Add Package Application Step][octopus-package]
2. Under the step's settings, set the "Package ID" to `$(Build.DefinitionName)`
3. Set "Package Format" to `NuPkg`
4. Set "Package Version" to `$(Build.BuildNumber)`
5. Set "Source Path" to `$(build.artifactstagingdirectory)\dist`, or whatever your output directory is
6. Set "Output Path" to `$(build.artifactstagingdirectory)\packed`

    ![Set Package Application Settings][octopus-package-settings]

### Add Push Package to Octopus Step
1. Add the "Push Package(s) to Octopus" step from the task catalogue under the "Package" tab

    ![Add Push to Octopus Step][push-octopus]
2. Under the step's settings, set the "Octopus Deploy Server" to the generic connection we set up at the beginning of this step.
3. Set "Package" to `$(build.artifactstagingdirectory)\packed\$(Build.DefinitionName).$(Build.BuildNumber).nupkg`

    ![Set Push to Octopus Settings][push-octopus-settings]

### Add Create Octopus Release Step
1. Add the "Create Octopus Release" step from the task catalogue under the "Deploy" tab

    ![Add Create Octopus Release Step][release-octopus]
2. Under the step's settings, set the "Octopus Deploy Server" to the generic connection we set up at the beginning of this step.
3. Set "Project Name" to `$(Build.DefinitionName)`. This assumes that you name the project in Octopus the same thing that this build definition will be named.
4. Set "Release Number" to `$(Build.BuildNumber)`

    ![Set Create Octopus Release Settings][release-octopus-settings]

### Set Build Number Format
1. Go to the "General" tab and set the "Build number format" to `1.0$(rev:.r)`. This will have to be incremented manually when you want to bump the version.

    ![Set Build Versioning][build-versioning]

### Save the Build Definition
1. Click "Save" and **name the project the same thing that your ASP.NET project is named**. In my example, the project is named "TestProject.Web".

---

## Step 2: Set Up Continuous Delivery Using Octopus Deploy

### Create a New Octopus Deploy project
1. Log in to Octopus Deploy (http://msft-build-prod/) by selecting "sign in with your Microsoft Windows domain account" on the sign in screen

    ![Log in to Octopus Deploy][octopus-login]
2. Head to the All Projects page and click "Add project" at the top

    ![Add a Project to Octopus Deploy][octopus-add-project]
3. Create a project with the same name that you used for your build definition in VSTS. The default settings are fine.
4. Head to the "Process" tab and click "Add step"
5. Select "IIS Application - Publish" from the list

    ![Add the IIS Application - Publish Step][octopus-app-publish-step]
6. Select "Web" from "Runs on targets in roles"
7. Select the type of authentication you'd like to use
8. If you'd like to substitute any Octopus variables in particular files, you can specify a file pattern to use in the "Variable Substitution Targets"
9. Specify the Hostname that you'd like the website to bind to in the "Hostname" field. This will default to <the Octopus project name>.<environment> -- "TestProject.Web.Test", for example.
10. Specify the environments you'd like the step to run in - probably "Test" and "Staging"

    ![IIS Application Publish - Settings][octopus-app-publish-settings]

### Optionally, Add Slack Notifications
1. Use the "Slack - Notify Deployment" step template and configure the integration

    ![Add Slack Step][octopus-slack-step]

### Done!
* Congrats! Your web project is all set up with CI & CD!

<!-- Image Resources -->
[octopus-connection]:../resources/ContinuousDeliveryImages/continuous-delivery_octopus-connection.png
[navigate-to-project]:../resources/ContinuousDeliveryImages/continuous-delivery_navigate-to-project.png
[new-from-definition]:../resources/ContinuousDeliveryImages/continuous-delivery_new-from-definition.png
[npm-packages]:../resources/ContinuousDeliveryImages/continuous-delivery_npm-packages.png
[copy-files]:../resources/ContinuousDeliveryImages/continuous-delivery_copy-files.png
[copy-files-settings]:../resources/ContinuousDeliveryImages/continuous-delivery_copy-files-settings.png
[octopus-package]:../resources/ContinuousDeliveryImages/continuous-delivery_octopus-package.png
[octopus-package-settings]:../resources/ContinuousDeliveryImages/continuous-delivery_octopus-package-settings.png
[push-octopus]:../resources/ContinuousDeliveryImages/continuous-delivery_push-octopus.png
[push-octopus-settings]:../resources/ContinuousDeliveryImages/continuous-delivery_push-octopus-settings.png
[release-octopus]:../resources/ContinuousDeliveryImages/continuous-delivery_release-octopus.png
[release-octopus-settings]:../resources/ContinuousDeliveryImages/continuous-delivery_release-octopus-settings.png
[build-versioning]:../resources/ContinuousDeliveryImages/continuous-delivery_build-versioning.png
[octopus-login]:../resources/ContinuousDeliveryImages/continuous-delivery_octopus-login.png
[octopus-add-project]:../resources/ContinuousDeliveryImages/continuous-delivery_octopus-add-project.png
[octopus-app-publish-step]:../resources/ContinuousDeliveryImages/continuous-delivery_octopus-app-publish-step.png
[octopus-app-publish-settings]:../resources/ContinuousDeliveryImages/continuous-delivery_octopus-app-publish-settings.png
[octopus-slack-step]:../resources/ContinuousDeliveryImages/continuous-delivery_octopus-slack-step.png
