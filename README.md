
# Flutter - iOS Bitbucket Pipeline Implementation.🔥

Bitbucket Pipeline for Flutter Deployment is the most struggling part of the bitbucket pipeline, especially for iOS.

I have struggled a lot to build a pipeline for iOS. So, I thought let's help other people who are struggling the same.
Here I will share how to set up a pipeline to deploy an iOS application on Testflight.

Also, I will share the best structure of the Bitbucket pipeline which I am using for my pipelines.

This topic will surprise you with the 'Validate iOS App' and 'Upload iOS App' steps.
But don't worry, I already made a tutorial about how to set up exportOption file and different commands for validation and upload.

Please go and Check this out 👇

[Flutter - iOS App Release on App Store Using CLI](https://medium.com/@parthg500/flutter-ios-app-release-on-app-store-using-cli-f15ff3910148)

If you need any help. DM me on [Linkedin](https://www.linkedin.com/in/parth-gupta-/)

#### ✨ - - -  Let's Begin  - - - ✨

#### To understand today's topic. You have to first understand the Bitbucket Tag. Which uses the most in this pipeline.

# Bitbucket TAG

### 1 image

It is a file with bunch of code which can includes regular stuff of the manual process. Like setting up the flutter path, android sdk and many more.

#### Sample Code
```bash
image: ghcr.io/cirruslabs/flutter:latest
```

### 2 custom

If you have multiple branches and you want to make separate pipelines for the branches themselves.
Otherwise, you can use 'default' instead of 'custom' for one pipeline.

Then in the custom tag we are managing 2 branches. 'main' and 'development' as tag.

#### Sample Code
```bash
pipelines:
  custom:
    master:
      - step:
          name: Flutter Build Release APK
          script:
            - flutter build apk --release

    development:
      - step:
          name: Flutter Clean
          script:
            - flutter clean
      - step:
          name: Flutter Build
          script:
            - flutter pub get
```

### 3 runs-on
[OPTIONAL ->> IF YOU WANT TO RUN YOUR PIPELINE ON YOUR LOCAL MACHINE]

The 'runs-on' tag will work for your local machine. You can host your machine to build and run your pipeline.

You can specify a runner in the bitbucket pipeline and as per your operating system, you can implement this in your machine.

I will make a different tutorial to run the pipeline on your machine and how to set up a runner for it.

Note: Use this tag if you have a host machine. It will work fine also if you haven't it.

#### Sample Code
```bash
steps:
      - step:
          runs-on:
            - self.hosted
            - macos
          name: Flutter Build Clean
          script:
            - flutter clean
```


### 4 stage

The stage is a combination of Multiple steps and gives full control to start it manually, In which you can specify 4 parameters #1 name and #2 trigger #3 deployment #4 steps.

You can use these stages for two different ways different deployments and you can specify it as your different build process.

In this tutorial, I applied it as a build process.

Note: You can not use the 'runs-on' tag for the stage. 'runs-on' tag will work only with the 'step' tag.

#### Sample Code

```bash
- stage:
          name: Flutter Cleaning
          steps:
            - step:
                runs-on:
                  - self.hosted
                  - macos
                name: Flutter Build Clean
                script:
                  - flutter clean
            - step:
                runs-on:
                  - self.hosted
                  - macos
                name: Flutter Pub Get
                script:
                  - flutter pub get

      - stage:
          name: Flutter Build iOS
          steps:
            - step:
                runs-on:
                  - self.hosted
                  - macos
                name: Flutter Build iOS
                script:
                  - flutter build ios --release --no-sound-null-safety
            - step:
                runs-on:
                  - self.hosted
                  - macos
                name: Flutter Build ipa file
                script:
                  - flutter build ipa --export-options-plist=$IOS_EXPORTOPTION_PLIST_FILE --no-sound-null-safety
                artifacts:
                  - build/ios/**
```

### 5 step

'step' is the main TAG or main component of the pipeline.

Most useful Parameters ->

    #1 name - Name of this step.
    #2 runs-on - Host Machine.
    #3 image - Image with which you want to complete this step.
    #4 trigger - How do you want to run this step, as manual or automatic?
    #5 deployment - the name of the deployment environment like internal, staging, and production.
    #6 script - All commands will work here. It is the heart of the main component.

#### Sample Code
```bash
- step:
      name: Flutter Build AppBundle
      runs-on:
        - self.hosted
        - macos
      trigger: manual
      deployment: Production
      script:
        - flutter build appBundle --release
```

### 6 artifacts

This uses the most to store the apk or ipa file in the folder to use for the next step. Also, you can use it for the download itself.

It has one boolean parameter 'download'. It is false by default. If you want to download you can set it as true.

#### Sample Code

```bash
- step:
    runs-on:
      - self.hosted
      - macos
    name: Flutter Build ipa file
    script:
      - flutter build ipa --export-options-plist=$IOS_EXPORTOPTION_PLIST_FILE --no-sound-null-safety
    artifacts:
      download: true
        - build/ios/**
```

# 

Now we have understood how we are going to use them and their structure. Here I am Attaching the actual code.

#### Go and check bitbucket-pipelines.yml file for full code.

Thank You.
