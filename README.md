# ASP.NET CORE + Vue + Typescript

This template is based in React Asp.Net Core template because Microsoft doesn't have plans to introduce Vue-specific features.

## Create .NET Project

There’s no ASP.NET Core Vue template, so we will create a project from React template and modify it to play well with Vue. To create a new .NET Core app just run the following command:

`dotnet new react -o aspnet-core-vue-app`

Open folder in your favourite editor, for VS Code owners:

_code aspnet-core-vue-app/.__

## Modify Template

Change Startup.cs:

Remove HTTPS redirect middleware for development on line 45 (otherwise it’s necessary to configure ASP.NET Core SSL certificate for a Vue app, it’s better to disable it).

```diff
if (!env.IsDevelopment())
{    
    // app.UseHttpsRedirection();
    app.UseExceptionHandler("/Error");
    app.UseHsts();
}
```

 Install the [VueCliMiddleware](https://www.nuget.org/packages/VueCliMiddleware) package:

`dotnet add package VueCliMiddleware`

Replace React middleware with Vue CLI on line 62:

```diff
+ using VueCliMiddleware;

- spa.UseReactDevelopmentServer(npmScript: "start");
+ // run npm process with client app
+ spa.UseVueCli(npmScript: "serve", port: 8080);
+ // if you just prefer to proxy requests from client app, use proxy to SPA dev server instead,
+ // app should be already running before starting a .NET client:
+ // spa.UseProxyToSpaDevelopmentServer("http://localhost:8080"); // your Vue app port
```

**Disable SSL config in the Application properties.**

## Create Vue Project

My settings were:

![Settings Vue CLI](./docs/settings-vue-cli.PNG)

Move all the contenst from the new folder /client-app to /ClientApp.

## Modify Template

Create a `vue.config.js` file in ClientApp root to change Vue production build output directory to _build_:

```diff
+ const path = require('path');
+ 
+ module.exports = {
+   outputDir: path.resolve(__dirname, 'build'),
+ };
```

Now the application is ready to run and deploy.
