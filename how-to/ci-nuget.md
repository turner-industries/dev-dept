# How to Implement Nuget Continuous Integration(CI) via Visual Studio Team Services(VSTS) and Github

## 1. Configuring the CI Build
  * Using Visual Studio Team Services(VSTS), create a new build definition in your project. Select the Visual Studio template and the default Repository source (it is named the same as your solution). Check the "Continuous integration" box. Create the build definition.

  * #### Adding a Github build definition
    * In Github, select "Settings". Under "Personal access tokens", generate a new access token. Enter your password and continue. Name your token, select "repo", "admin:repo_hook", and "user", and create the personal access token. Copy the generated token.
    * Select the Repository tab. Select "Github" for the Repository type, and click "Manage" next to the Connection field to open the Services tab of your VSTS project's control panel. Add a new service endpoint for Github. Choose "Personal access token" for authorization, and enter the personal access token you generated in Github. Add a connection name and continue. On the repository tab, select the new Github connection in the "Connection" field. Select the correct repository and continue.

## 2. Configuring the .nuspec File
  * Open your solution in Visual Studio. Using the Nuget Package Manager, install NuGet.CommandLine, restart Visual Studio and open your solution. In the Package Manager Console, enter "nuget spec" to generate a .nuspec file. Fill in the appropriate information for authors, owners, licenseUrl(if available), etc. Enter $(buildNumber)$ in the version tag and save, commit, and push the project to Github (Nuget.CommandLine is not necessary one the .nuspec file has been instantiated, so you can optionally delete this package before pushing your changes to the repository).

## 3. Configuring the Build Steps for Nuget
  * Under the Variables tab in VSTS, select "Add variable". Add two new variables: Major and Minor. Enter the starting major and minor version numbers that you would like for the package.
  * Under the General tab in VSTS, enter set the Build number format field to $(Major).$(Minor).$(Year:yy)$(DayOfYear).$(Rev:rr).
  * In the Build tab in VSTS, select "add build step..." Under the "Packages" tab, add "NuGet Packager" and "NuGet Publisher" to the build.

    * #### Configuring the NuGet Packager
      * Select the NuGet Packager build definition. Under the Advanced section, set the Additional Build Properties to: buildOutputPath="$(Build.SourcesDirectory)\SOLUTION_NAME\PROJECT_NAME\bin\$(BuildConfiguration)";buildNumber=$(Build.BuildNumber). This path leads to the .nuspec file in your project.

    * #### Configuring the NuGet Packager
      * Select the NuGet Publisher build definition. Verify that "External NuGet Feed" is selected. Select "Manage" next to the Nuget Server Endpoint field. This will open the services endpoint manager in your project's VSTS control manager. Select "Generic" under "New Service Endpoint". Name the connection, set the Server URL to "nuget.org", and enter the user name and token key for the Nuget client that manages the Nuget Packages for the solution.

## 4. Finishing Up
  * Queue and build the project to verify that it is successful. The package in Nuget should be updated with the new version number.

## References and Helpful Links
*  [Automating building and publishing a NuGet package using VSTS](https://technologies.live/2016/04/01/automatically-create-and-publish-a-nuget-package/)
*  [Creating a new VSTS solution build](https://msdn.microsoft.com/en-us/library/vs/alm/build/vs/define-build)
*  [Specifying a Github Repository for VSTS](https://msdn.microsoft.com/library/vs/alm/build/define/repository#GitHub)
*  [Creating an personal access token in Github](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)
*  [.nuspec Reference](http://docs.nuget.org/Create/Nuspec-Reference)
*  [More on Visual Studio Team Services Package Management](https://www.visualstudio.com/docs/package/overview)
