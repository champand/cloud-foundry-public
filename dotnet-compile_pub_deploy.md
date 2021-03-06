---

copyright:
  years: 2015, 2020
lastupdated: "2020-09-16"

keywords: cloud foundry

subcollection: cloud-foundry-public



---


{:beta: .beta}
{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:java: data-hd-programlang="java"}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}

 
# Compile, Publish, and Deploy Apps
{: #publish_configure_deploy}

## Compiling your app in Release configuration (MSBuild only)
{: #compiling_in_release_configuration}

MSBuild based projects are now published using the `dotnet publish` command during staging.  By default, the buildpack will publish your app in `Debug` configuration.
To publish your app in `Release` configuration, set the `PUBLISH_RELEASE_CONFIG` environment variable to `true`.

You can do this with the {{site.data.keyword.cloud_notm}} CLI with the following command:

```
ibmcloud cf set-env <app_name> PUBLISH_RELEASE_CONFIG true
```
{: pre}

Alternatively, you can set the variable in your app's `manifest.yml` file:

```
---
apps:
- name: sample-aspnetcore-app
  memory: 512M
  env:
    PUBLISH_RELEASE_CONFIG: true
```
{: codeblock}

## Pushing a Published App
{: #pushing_published_app}

If you want your app to contain all its required binaries so that the buildpack does not download any
external binaries, you can push a published *self-contained* app.  See [.NET Core App Types](https://docs.microsoft.com/en-us/dotnet/articles/core/app-types){: external}
for more information on self-contained apps.

To publish an app issue a command such as:
```
dotnet publish -r ubuntu.14.04-x64
```
{: pre}

For self-contained apps, the app can then be pushed from the
```
  bin/<Debug|Release>/<framework>/<runtime>/publish
```
{: codeblock}
directory.

For portable apps, the app can be pushed from the
```
  bin/<Debug|Release>/<framework>/publish
```
{:codeblock}
directory.

Also note that if you are using a `manifest.yml` file in your app, you can specify the path to the publish output folder in your `manifest.yml` file.  Then you don't have to be in that folder when you push the app.

## Deploying apps with multiple projects
{: #developing_apps_with_multiple_projects}

To deploy an app which contains multiple projects, you will need to specify which project you want the buildpack to run as the main project. This can be done by creating a `.deployment` file in the root folder of the solution which sets the path to the main project. The path to the main project can be specified as the project folder or the project file (`.xproj` or `.csproj`).

For example if a solution which contains three projects, `MyApp.DAL`, `MyApp.Services`, and `MyApp.Web` in the `src` folder, and `MyApp.Web` is the main project, the format of the `.deployment` file is as follows:
```
  [config]
  project = src/MyApp.Web/MyApp.Web.csproj
```
{: codeblock}

In this example, the buildpack will automatically compile the `MyApp.DAL` and `MyApp.Services` projects if they are listed as dependencies in the `project.json` file for `MyApp.Web`, but the buildpack only attempts to execute the main project, `MyApp.Web`, when running `-p src/MyApp.Web`.


