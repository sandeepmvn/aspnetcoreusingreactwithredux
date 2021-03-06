# Sample quick start project to creating asp.net core with react js + Redux pattern

microsoft documentation:-

https://docs.microsoft.com/en-us/aspnet/core/client-side/spa/react-with-redux?view=aspnetcore-3.1

https://docs.microsoft.com/en-us/aspnet/core/client-side/spa/react?view=aspnetcore-3.1&tabs=visual-studio


Step-1 
Open VS(Visual Studio) 2019 and Select create new project ----> select the asp.net core template or search for asp.net core template ---> click on next

Step-2
Configure your new project with,

 1. Provide your project name
 2. Location
 3. Solution Name
 
Click on create 

Step-3
Now select the project template as "React Js and Redux" and click on the create. (Most probably this template is available in visual 2019)  

If in case this template is not availabe then go to
https://marketplace.visualstudio.com/search?target=VS&category=Templates&vsVersion=&subCategory=ASP.NET&sortBy=Relevance  search for the required template and install.

Step- 4
Now project is created with server: Asp.Net core server and Client app: React Js with Redux pattern

then how could asp.net framework identifies the react routing?

To solve above issue, framework has provided us middlware called UseSpa 

What is the UseSpa middlware?

Handles all requests from this point in the middleware chain by returning the default page for the Single Page Application (SPA).

This middleware should be placed late in the chain, so that other middleware for serving static files, MVC actions, etc., takes precedence.


In Startup.cs

          public void ConfigureServices(IServiceCollection services)
            {
                services.AddControllersWithViews();

                // In production, the React files will be served from this directory
               # services.AddSpaStaticFiles(configuration =>
                {
                    configuration.RootPath = "ClientApp/build";
                });
            }
            
       // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
              public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
              {
                 //.................
                  app.UseStaticFiles();
                 # app.UseSpaStaticFiles();

                  app.UseRouting();

                  app.UseEndpoints(endpoints =>
                  {
                      endpoints.MapControllerRoute(
                          name: "default",
                          pattern: "{controller}/{action=Index}/{id?}");
                  });

                 # app.UseSpa(spa =>
                  {
                      spa.Options.SourcePath = "ClientApp";

                      if (env.IsDevelopment())
                      {
                          spa.UseReactDevelopmentServer(npmScript: "start");
                      }
                  });
              }

How could react application build with asp.net core?

When we build(ctrl+shift+b) an core/web application, internally it calls npm start/build/install and all these is configured in your .csproj.\

    <Target Name="DebugEnsureNodeEnv" BeforeTargets="Build" Condition=" '$(Configuration)' == 'Debug' And !Exists('$(SpaRoot)node_modules') ">
      <!-- Ensure Node.js is installed -->
      <Exec Command="node --version" ContinueOnError="true">
        <Output TaskParameter="ExitCode" PropertyName="ErrorCode" />
      </Exec>
      <Error Condition="'$(ErrorCode)' != '0'" Text="Node.js is required to build and run this project. To continue, please install Node.js from https://nodejs.org/, and then restart your command prompt or IDE." />
      <Message Importance="high" Text="Restoring dependencies using 'npm'. This may take several minutes..." />
      <Exec WorkingDirectory="$(SpaRoot)" Command="npm install" />
    </Target>
    
At the time of deployment/publishing the application: -    

   <Target Name="PublishRunWebpack" AfterTargets="ComputeFilesToPublish">
       <!-- As part of publishing, ensure the JS resources are freshly built in production mode -->
       <Exec WorkingDirectory="$(SpaRoot)" Command="npm install" />
       <Exec WorkingDirectory="$(SpaRoot)" Command="npm run build" />

       <!-- Include the newly-built files in the publish output -->
       <ItemGroup>
         <DistFiles Include="$(SpaRoot)build\**; $(SpaRoot)build-ssr\**" />
         <ResolvedFileToPublish Include="@(DistFiles->'%(FullPath)')" Exclude="@(ResolvedFileToPublish)">
           <RelativePath>%(DistFiles.Identity)</RelativePath>
           <CopyToPublishDirectory>PreserveNewest</CopyToPublishDirectory>
           <ExcludeFromSingleFile>true</ExcludeFromSingleFile>
         </ResolvedFileToPublish>
       </ItemGroup>
     </Target>



