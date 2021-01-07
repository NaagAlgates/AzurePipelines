## How to build, sign, and distribute your Android App using Azure Pipelines? 

As a mobile developer, I know the pain of building the APKs regularly and sharing the latest version with different teams. Additionally, we need to upload the newest version to the Microsoft AppCenter and Google PlayStore. A minor mistake in the routine may lead to some serious trouble. For any unforeseen issue, the best solution is to automate the process. 

Here at [XAM](https://xam.com.au/), we use Azure Pipelines to build, Sign & publish the APKs without any manual intervention and thus preventing the time taken for the repeated tasks.

 

Continuous Integration – Azure DevOps: 

Today I’m going to explain the process involved in Continuous Integration using the Azure DevOps for Android.

The advantages of Azure DevOps are

    Accessible anywhere
    No server maintenance
    Good community support
    Often updated
    Low cost

Steps:

Setup Android project:

I have created a simple Hello World Android project in GitHub for your convenience. In the root folder, you should find a YAML file named ‘pipelines_android.yml’. If you don’t want to create it from scratch, please make sure you commit this file to your repo. 

Got to your project

Pipelines-> Create Pipeline
![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/knqo06oogqwokypq8k4u.png)


Select your repo source 

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/qp0l6a1a6r1njd51m22m.png)

Since you’ve already added the YAML file, select the existing file to create a pipeline. 

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/9431wuoige6rt9gpy5gm.png)

Else you can create a new YAML file by selecting the appropriate option.

I have already added the parameters to the pipelines. Please add as many parameters as you need.

Allow the user to select the build type and their preferred operating system.  

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/obfivtykgi66fds10elc.png)

Next, create a triggering point for this pipeline. 

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/dz2uj1r4y8ubq21z5vpu.png)

 In my pipeline, I’ve added the branch “master” as a trigger. So, whenever there is a pull request merged on the master branch, this pipeline will start.

Create a variable group, as shown below.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/eh72e3lu9m1rtup4pumt.png)

To create a new variable group:

Goto Pipelines-> Library
Click + Variable group
Add a variable group name.
Add variables.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/332jdgof0yi92m6a38db.png)

Clean the workspace and then add a pool image.
![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/jova9qy7wyzbs2p1m2fs.png)

 The first step is to create Gradle@2, as shown below.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/5nfg3pivj8rchtyc78i6.png)

The next task is to sign the generated APK.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/3d60ubt45dvpdxs25iik.png)

After Signing the APK, now we will have to publish the artifacts. 

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/lku9qxkjspdk5plxy5qg.png)

After publishing it internally, let’s publish the artifacts to the AppCenter.

Go to https://appcenter.ms/apps/create and create a new App.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/c96v8zw7p3voa6plf13a.png)

Once the App created in AppCenter, now introduce a testing group.
 
![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/m65xql3vub8xmvyzzyaj.png)

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/pl5u0kpeu3t8gtsbbbrr.png)

Click the spanner icon and make a note of the group id. 

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/m0iniy1lewgbo2pecbac.png)

The next step is to create a service connection between Azure DevOps and AppCenter. 

In AppCenter, go to the account settings page and add a new API token. 

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/q4912skow00xsilqd7k2.png)

In Azure DevOps:
Click the Settings menu (gear icon) in Azure DevOps.
Click the “Service Connection” menu on the left.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/n51cge7rt4tb4jkku0io.png)

Choose “Visual Studio App Center” and click the Next button.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/ixkaydgymnp311bpt6rm.png)

Paste the copied token, give an appropriate name for this connection, and click Save. 

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/gjm6kvtp1b2vkqbqle5x.png)

Go to Library-> Add new Variable group.
Add AppCenter Connection.
Add Slug Id: Username/AppName
Add Group Id: paste the copied group id.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/knuqm377w8fk715wg07t.png)

Then in the pipeline, add the AppCenterDistribute@3 task.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/muw81e4sa51tkb20q6ob.png)

Build the pipeline, and you should see the artifacts uploaded in the AppCenter.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/b0hc13czwm5b2vundc4t.png)

