# Using Web.config Transformation Syntax in Web Project Deployment for Web Project Build, in Class Library and in Windows Applications

> A transform file is an XML file that specifies how the Web.config file should be changed when it is deployed. Transformation actions are specified by using XML attributes that are defined in the XML-Document-Transform namespace, which is mapped to the xdt prefix.
> The XML-Document-Transform namespace defines two attributes: Locator and Transform. The Locator attribute specifies the Web.config element or set of elements that you want to change in some way. The Transform attribute specifies what you want to do to the elements that the Locator attribute finds.

## To use trasformation with build in web application

 1. Unload the project
 2. Edit the project file *.csporj
 3. Append these line at the end of the file and before the `</project>` tag closing
	 - For web projects
     `<UsingTask  TaskName="TransformXml"  AssemblyFile="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\Web\Microsoft.Web.Publishing.Tasks.dll" />`
`     <Target  Name="ApplyWebConfigTransform"  Condition="Exists('Web.$(Configuration).config')">`
`     <!-- Importance="High" for messages mean they show up in VS output window. -->`
`     <Message  Importance="High"  Text="Clearing read-only flag..." />`
`     <!-- TFS insists on marking all files in source control as read-only... -->`
`     <Exec  Command="%SystemRoot%\system32\attrib -r Web.config" />`
`     <Message  Importance="High"  Text="Running configuration transform: $(Configuration) =&gt; Web.$(Configuration).config" />`
`     <TransformXml  Source="Web.config"  Transform="Web.$(Configuration).Config"  Destination="web.config" />`
`     </Target>`
`     <Target  Name="BeforeBuild"  Condition="'$(PublishProfileName)' == '' And '$(WebPublishProfileFile)' == ''">`
`     <CallTarget  Targets="ApplyWebConfigTransform" />`
`     </Target>`
	 - For windows and class library projects
	 `<UsingTask  TaskName="TransformXml"  AssemblyFile="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\Web\Microsoft.Web.Publishing.Tasks.dll" />`
`     <Target  Name="ApplyAppConfigTransform"  Condition="Exists('App.$(Configuration).config')">`
`     <!-- Importance="High" for messages mean they show up in VS output window. -->`
`     <Message  Importance="High"  Text="Clearing read-only flag..." />`
`     <!-- TFS insists on marking all files in source control as read-only... -->`
`     <Exec  Command="%SystemRoot%\system32\attrib -r App.config" />`
`     <Message  Importance="High"  Text="Running configuration transform: $(Configuration) =&gt; App.$(Configuration).config" />`
`     <TransformXml  Source="App.config"  Transform="App.$(Configuration).Config"  Destination="App.config" />`
`     </Target>`
`     <Target  Name="BeforeBuild">`
`     <CallTarget  Targets="ApplyAppConfigTransform" />`
`     </Target>`
 4. Add the transformation attributes to the configuration transform files and build 

> Check the sample web, class library and console projects