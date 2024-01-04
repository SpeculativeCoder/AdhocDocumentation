# Adhoc Documentation

Documentation / Guide for Adhoc (final name to be decided).

[https://github.com/SpeculativeCoder/adhoc-web](https://github.com/SpeculativeCoder/adhoc-web)

[https://github.com/SpeculativeCoder/AdhocPlugin](https://github.com/SpeculativeCoder/AdhocPlugin)

[https://github.com/SpeculativeCoder/AdhocDocumentation](https://github.com/SpeculativeCoder/AdhocDocumentation)

This is a work in progress, experimental, and subject to major changes.

The eventual ideal/goal of Adhoc is to be a system for running a multi-user, multi-server 3D world in the cloud (e.g. AWS) using Unreal Engine with the HTML5 ES3 (WebGL2) platform plugin.

Live Example: [**AdhocCombat** (https://adhoccombat.com)](https://adhoccombat.com) - work in progress

## Guide

**IMPORTANT: Any actions below (and usage of cloud services) will result in risks and costs to you. Everything done is entirely at your own risk. You are responsible for any and all costs incurred. Also see this documentation's [LICENSE](LICENSE) file which contains a disclaimer.**

**This is the initial _skeleton_ of the guide and it is just a basic outline for now with some notes about what still needs to be documented. Thus, this guide is not suitable for general use yet and is currently only of technical interest to help understand some of how Adhoc is set up.**

### Ensure your domain can be managed by Route53

**TODO: Write/complete documentation for this step.**

_Initial notes:_

Adhoc currently assumes you can manage your domain via AWS Route53. This is up to you to set up, pay for etc. **You can proceed without it for initial testing however you will only be able to connect via IP address and will not have HTTPS.**

From here the domain (if you have one) in this documentation will be referred to as `example.com`. This is just an example. Remember to replace this domain for whatever your domain is in all the steps later as `example.com` is just an example domain for this documentation.

### Setup UE 4.27 HTML5 ES3

**TODO: Write/complete documentation for this step.**

_Initial notes:_

https://github.com/SpeculativeCoder/UnrealEngine-HTML5-ES3

From here on we assume that Unreal Engine is located at `c:/UE/ue-4.27-html5-es3` (NOTE: the forward slashes rather than backslashes are how Git Bash see things, and we do all our work in Git Bash). Remember to replace this directory for whatever your situation is in all the steps later.

### Create an Unreal project

**TODO: Write/complete documentation for this step.**

_Initial notes:_

Create a C++ Unreal project (e.g. Third Person C++ sample project) or import an existing project into your UE 4.27 HTML5 ES3. Make sure you can package the project for HTML5 in both Development and Shipping packaging configuration and have ran it locally in a web browser (e.g. using HTML5LauncherHelper.exe).

From here on we assume this project is called `ThirdPerson` and is located at `c:/ThirdPerson/ThirdPerson` (the double level directory is just to keep things organised with other code we will be using later). Remember to replace this Unreal project name / directory for whatever your situation is in all the steps later.

### Legal requirements

Consider the legal requirements of distributing Unreal Engine, content etc. (e.g. users will typically be downloading the packaged engine/project to their machine and running it). You should adhere to all legal requirements. Legal advice is not provided here - it is your responsibility. I am not a lawyer. This is not legal advice.

Some relevant links:
- https://www.unrealengine.com/en-US/release
- https://www.unrealengine.com/en-US/eula/unreal
- https://www.unrealengine.com/en-US/eula/content

### Other Release Considerations

**TODO: Write/complete documentation for this step.**

_Initial notes:_

See https://github.com/SpeculativeCoder/UnrealEngine-HTML5-ES3#releasing-your-project for things to consider in addition to legal considerations. Enabling crypto and making sure you have appropriate texture sizes are some of these things.

### Create Client/Server targets

**TODO: Write/complete documentation for this step.**

_Initial notes:_

Create two copies of `ThirdPerson/Source/ThirdPerson.Target.cs called
- `ThirdPersonClient.Target.cs`
- `ThirdPersonServer.Target.cs`

In `ThirdPersonClient.Target.cs` rename the class and constructor `ThirdPersonTarget` to `ThirdPersonClientTarget` and change `TargetType.Game` to `TargetType.Client`.

In `ThirdPersonServer.Target.cs` rename the class and constructor `ThirdPersonTarget` to `ThirdPersonServerTarget` and change `TargetType.Game` to `TargetType.Server`.

I like to add `bUseLoggingInShipping = true;` to the constructors too to get logging in Shipping builds.

You should be able to build the client / server configurations for your project in Unreal Editor, although this will be done later anyway so optional to do it now.

### Enable command line / web socket URL parameters for Unreal project

**TODO: Write/complete documentation for this step.**

_Initial notes:_

Adhoc relies on telling the HTML5 client to connect to a particular server as a particular user. For this we make use of the command line and the ability to provide a web socket URL that the client will connect to.

Enable the command line and websocket URL via session storage functionality, which allows convenient passing of web socket / command line to the HTML5 client:

- Project Settings -> Platforms -> HTML5 -> Session storage command line key: `UnrealEngine_CommandLine`
- Project Settings -> Platforms -> HTML5 -> Session storage WebSocket URL key: `UnrealEngine_WebSocketUrl`

You won't notice anything different yet with the HTML5 client but later steps will actually make use of this.

### Clone AdhocPlugin

**TODO: Write/complete documentation for this step.**

_Initial notes:_

Clone the AdhocPlugin into the Plugins folder in your Unreal project:

    cd c:/ThirdPerson/ThirdPerson
    mkdir Plugins
    cd Plugins
    
    git clone https://github.com/SpeculativeCoder/AdhocPlugin.git AdhocPlugin

### Compile/test Unreal project with AdhocPlugin

**TODO: Write/complete documentation for this step.**

_Initial notes:_

Build the Visual Studio project again for your Unreal project. It should pick up the AdhocPlugin in the compile.

You should see/enable the plugin in your Unreal project now (see Edit -> Plugins -> Search for Adhoc -> AdhocPlugin -> Enabled - make sure this is ticked). Make sure you can still run the project in the Unreal Editor with the plugin enabled. It may try to connect to localhost when it starts (and fail with an error in the output log) but that is OK for now.

Make sure also that you can still build the HTML5 Client (e.g. HTML5 ThirdPersonClient) and it works (the plugin shouldn't cause any issues).

### Ensure you can build Linux dedicated server

**TODO: Write/complete documentation for this step.**

_Initial notes:_

https://docs.unrealengine.com/4.27/en-US/SharingAndReleasing/Linux/GettingStarted/

### Clone adhoc-web

**TODO: Write/complete documentation for this step.**

_Initial notes:_

    cd c:/ThirdPerson
    
    git clone https://github.com/SpeculativeCoder/adhoc-web.git adhoc-web
    
From here on we assume adhoc-web is located at `c:/ThirdPerson/adhoc-web`. Remember to replace this directory for whatever your situation is in all the steps later.

### Run AdhocManagerApplication locally

**TODO: Write/complete documentation for this step.**

_Initial notes:_

    cd c:/ThirdPerson/adhoc-web

    mvn clean package -DskipTests

Run the `AdhocManagerApplication` class in your Java IDE.

You should be able to see it running at http://localhost:80

### Test server / client interaction

**TODO: Write/complete documentation for this step.**

_Initial notes:_

You will need to set a local testing server password. AdhocPlugin needs a matching password to talk to the web application when testing locally.

Create a file `c:/ThirdPerson/adhoc-web/env/local.env`:

    SERVER_BASIC_AUTH_PASSWORD=TODO:\ local\ testing\ password

Now restart AdhocManagerApplication - instead of using a random password it will pick up this testing password above.

Run the project in the Unreal Editor and start play in editor. You should see it start talking to the manager running and should see a message regarding "OnStompConnected" which means the websocket is connected OK.

### Setup env/common.env and customization/app-environment.ts file

**TODO: Write/complete documentation for this step.**

_Initial notes:_

Create a file `c:/ThirdPerson/adhoc-web/env/common.env` with the following contents (remember to adjust to your name/directories etc.):

    UNREAL_PROJECT_NAME=ThirdPerson
    UNREAL_PROJECT_REGION_MAPS=ThirdPersonExampleMap
    UNREAL_PROJECT_TRANSITION_MAP=Entry
    UNREAL_PROJECT_DIR=c:/ThirdPerson/ThirdPerson
    UNREAL_ENGINE_DIR=c:/UE/ue-4.27-html5-es3
    ADHOC_NAME=thirdperson

NOTE: The Entry map is always available to use as a transition/loading screen in most Unreal projects as it comes from the engine so you should be OK using this if you don't have your own.

Create a directory `c:/ThirdPerson/adhoc-web/adhoc-angular-app/src/customization`

Copy `c:/ThirdPerson/adhoc-web/adhoc-angular-app/src/app/app-customization.ts` to `c:/ThirdPerson/adhoc-web/adhoc-angular-app/src/customization/app-customization.ts`

Modify it to be what you want to be known as in the About page - **this will be publicly visible on the deployed site so only include information you are comfortable presenting to the public.**

    /** you can create a copy of this in src/customization and tailor it to your needs */
    export const appCustomization = {
      appTitle: 'ThirdPerson',
      appDeveloper: 'ThirdPerson Developer',

      /** any additional about information (e.g. assets used etc.) can go in here */
      aboutPageMoreHtml: `
    <p>Hello, World!</p>
      `,
    };

### build_all_local

**TODO: Write/complete documentation for this step.**

_Initial notes:_

    ./build_all_local.sh
    
### Run AdhocManagerApplication locally, then connect via web HTML5 client

**TODO: Write/complete documentation for this step.**

Run AdhocManagerApplication.
Run the Unreal Editor as a Listen server.
On the web page - you should be able to connect to the server via double click.

### Run AdhocManagerApplication locally in Docker profile then connect via web HTML5 client

**TODO: Write/complete documentation for this step.**

To run the test servers in Docker (rather than having to run your own server in Unreal Editor) you can run AdhocManagerApplication using profiles `db-hsqldb,hosting-docker,dns-local` (the default profiles when running AdhocManagerApplication are `db-hsqldb,hosting-local,dns-local`).

This will start up the Unreal servers in your local Docker, rather than you having to manually run a server in Unreal Editor. It is thus much closer to the final cloud deployment as there are multiple servers running in Linux containers etc.

### Create AWS adhoc_acme user

**TODO: Write/complete documentation for this step.**

_Initial notes:_

Create a AWS adhoc_acme user with appropriate Route53 access, generate an access token, and set this as adhoc_acme as a user in your AWS profile file(s). This will be used when invoking acme.sh to do domain cert creation.

If you don't have a domain yet you can skip this step.

### Generate certs (using acme?)

**TODO: Write/complete documentation for this step.**

_Initial notes:_

Add the following to `c:/ThirdPerson/adhoc-web/env/common.env` in addition to its existing contents (remember to adjust to your domain):

```
SSL_ENABLED=true
ADHOC_DOMAIN=example.com
ROUTE53_ZONE=example.com
```

If you don't have a domain yet you don't need to add either of these lines (effectively this means SSL_ENABLED will treated as false) and should skip the rest of this step (you won't be able to generate certs).

Generate the certs:

    ./refresh_certs.sh
    
This will generate certs and copy them into c:/ThirdPerson/adhoc-web/certs

This directory or its files should **never** be checked in to git (the directory is thus in the adhoc-web .gitignore to reduce the chance of accidental commit).

### Enable SSL in Unreal project and web project 

**TODO: Write/complete documentation for this step.**

_Initial notes:_

If you don't have a domain yet, skip this step.

In your Unreal project, enable SSL:

Project Settings -> Plugins -> WebSocket Networking
- Enable SSL = true
- CA certificate = ../../../certs/thirdperson-ca.cer
- Server Certificate = ../../../certs/thirdperson.cer
- Private Key = ../../../certs/thirdperson.key
- Editor Client Skip Hostname Check = true

NOTE: If interested in a more detailed description of SSL in HTML5 see [here](https://github.com/SpeculativeCoder/UnrealEngine-HTML5-ES3/blob/main/Features/Feature-WebSocketSSL.md#enabling-websocket-ssl).

For local testing we can make the certs available to Unreal Editor.

Create a directory `c:/UE/ue-4.27-html5-es3/certs`

Copy the following files from `c:/ThirdPerson/adhoc-web/certs` to `c:/UE/ue-4.27-html5-es3/certs`:
- thirdperson.cer
- thirdperson.key
- thirdperson-ca.cer

This directory or its files should **never** be checked in to git (the UnrealEngine .gitignore should already be ignoring by default but be sure you never accidentally add or check them in).

### Add a hosts file entry for your domain

**TODO: Write/complete documentation for this step.**

_Initial notes:_

If you don't have a domain yet you can skip this step.

Add the following to hosts file:

```
127.0.0.1   manager-local.example.com
127.0.0.1   kiosk-local.example.com
127.0.0.1   1-server-local.example.com
127.0.0.1   2-server-local.example.com
```

### Run AdhocManagerApplication locally in Docker profile then connect via web HTML5 client to test HTTPS/WSS connections

**TODO: Write/complete documentation for this step.**

_Initial notes:_

If you don't have a domain yet you can skip this step.

### Create AWS adhoc_admin user

**TODO: Write/complete documentation for this step.**

_Initial notes:_

We need an AWS user which will be used to perform infrastructure maintenance (via terraform) and upload images during builds.

Create a AWS adhoc_admin user with Administrator rights, generate an access token, and set this as adhoc_admin as a user in your AWS profile file(s).

### Terraform Apply Dev

**TODO: Write/complete documentation for this step.**

_Initial notes:_

Create a file `c:/ThirdPerson/adhoc-web/terraform/terraform.tfvars` which will define various infrastructure settings. Remember to adjust to your domain and whatever regions you want to use (if you don't have a domain yet - do not include that line):

```
adhoc_name_dev = "thirdperson"
adhoc_name_qa = "thirdperson"
adhoc_name_prod = "thirdperson"
adhoc_region_dev = "eu-west-2"
adhoc_region_qa = "us-east-2"
adhoc_region_prod = "us-east-2"
route53_zone = "example.com"
```

Apply terraform in `dev` workspace:

    cd c:/ThirdPerson/adhoc-web/terraform

    terraform init

    terraform workspace new dev
    terraform plan
    terraform apply
    
Terraform state (containing sensitive information including passwords/certificates) is currently stored locally in files/directories such as terraform.tfstate, terraform.tfstate.backup, and terraform.tfstate.d - these state files/directories should **never** be checked in to git (the files/directories are thus in the adhoc-web/terraform/.gitignore to reduce the chance of accidental commit).

### Set dev/qa/prod regions in env

**TODO: Write/complete documentation for this step.**

_Initial notes:_

In `c:/ThirdPerson/adhoc-web/env/dev.env` (remember to adjust the below to whatever region you are using):

```
AWS_REGION=eu-west-2
SERVER_AVAILABILITY_ZONE=eu-west-2a
```
 
In `c:/ThirdPerson/adhoc-web/env/qa.env` (remember to adjust the below to whatever region you are using):

```
AWS_REGION=us-east-2
SERVER_AVAILABILITY_ZONE=us-east-2a
```

In `c:/ThirdPerson/adhoc-web/env/prod.env` (remember to adjust the below to whatever region you are using):

```
AWS_REGION=us-east-2
SERVER_AVAILABILITY_ZONE=us-east-2a
```

### Build All and Upload Dev

**TODO: Write/complete documentation for this step.**

_Initial notes:_

Build / upload the dev images:

    ./build_all_and_upload_dev.sh

### Start / Stop

**TODO: Write/complete documentation for this step.**

_Initial notes:_

Set thirdperson_manager and thirdperson_kiosk service to 1 instance via AWS console (go to the ECS section for the appropriate region and you should see the services).

Check logs.

Hit dev environment e.g. dev.example.com (if you don't have a domain you will have to access the kiosk task directly via the public IP that AWS will have assigned).

Make sure to set the services back to 0 at the end to switch off the tasks. Wait a few minutes and double check to be sure there are no longer any tasks running in the cluster.

### Terraform Destroy Dev

**TODO: Write/complete documentation for this step.**

_Initial notes:_

Remember to destroy the environment to avoid any further costs!

    cd c:/ThirdPerson/adhoc-web/terraform

    terraform destroy

## Documentation Copyright / License

Copyright (c) 2022-2024 SpeculativeCoder (https://github.com/SpeculativeCoder)

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

See [LICENSE](LICENSE) (CC-BY-4.0). The license only applies to the files in this repository.
