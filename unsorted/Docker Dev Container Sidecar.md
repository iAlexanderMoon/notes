DockerContainer Sidecar

Is there a way to tell Visual Studio to use a different location for the bin and obj directories?

Yes. You used to be have to do it for every project...

now you can add a new property to every project in one step by defining it in a single file called Directory.Build.props in the root folder that contains your source


When searching for a Directory.Build.props file, MSBuild walks the directory structure upwards from your project location ($(MSBuildProjectFullPath)), stopping after it locates a Directory.Build.props file. 

There is a mechansim to have it continue searching to merge the files in.


SO: 
	Let's try setting a global solution file
	Remember our contexts: If we want them to stay outside of the working folder and so NOT SHARED between contexts
	we need to have the build props outside of the /working path that is mounted and inside each container only!
	so let's try just putting them in /tmp/
	
Solution/Directory.Build.props
```	
<Project>

  <PropertyGroup Condition="'$(DOTNET_RUNNING_IN_CONTAINER)' == 'true'">
    <BaseIntermediateOutputPath>/tmp/$(MSBuildProjectDirectory)/obj/container/</BaseIntermediateOutputPath>
    <BaseOutputPath>/tmp/$(MSBuildProjectDirectory)/bin/container/</BaseOutputPath>
  </PropertyGroup>

  <PropertyGroup Condition="'$(DOTNET_RUNNING_IN_CONTAINER)' != 'true'">
    <BaseIntermediateOutputPath>$(MSBuildProjectDirectory)/obj/local/</BaseIntermediateOutputPath>
    <BaseOutputPath>$(MSBuildProjectDirectory)/bin/local/</BaseOutputPath>
  </PropertyGroup>

</Project>
```
	

	







Reference:
https://stackoverflow.com/questions/4735534/how-can-i-redirect-the-bin-and-obj-directories-to-a-different-location
https://docs.microsoft.com/en-us/visualstudio/msbuild/customize-your-build?view=vs-2019