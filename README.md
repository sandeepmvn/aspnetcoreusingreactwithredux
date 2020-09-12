# Sample quick start project to creating asp.net core with react js + Redux pattern

microsoft documentation:-

https://docs.microsoft.com/en-us/aspnet/core/client-side/spa/react-with-redux?view=aspnetcore-3.1


Step-1 
Open VS(Visual Studio) 2019 and Select create new project ----> select the asp.net core template or search for asp.net core template ---> click on next

Step-2
Configure your new project with,

 1. Provide your project name
 2. Location
 3. Solution Name
 
Click on create 

Step-3
Now select the project template as "React Js and Redux" and click on the create. If in case this template is not availabe then go to
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







