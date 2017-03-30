# OneMobileSDK-tools-android

## Build scripts

This repo contains common use build scripts for android library development
that will be useful for CI and CD setup with Travis/Bintray stack.

## Content

- `git.gradle` helpful git values
- `travis.gradle` ci values
- `android.gradle` android library script
- `bintray.gradle` bintray upload script
- `javadoc.gradle` javadoc artifact generation
- `sources.gradle` sources artifact generation

## Usage

Considering that this repo is attach to project as git *submodule* as `tools`
folder, project build script should look like this:

```gradle
repositories {
    jcenter()
}

buildscript {
    repositories {
        jcenter()
    }

    dependencies {
        // Mandatory section
        // Android library plugin
        classpath 'com.android.tools.build:gradle:2.3.0'
        // Local maven deployment plugin
        classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'
        // Bintray upload plugin
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7.3'
    }
}

// Order is important
apply from: 'tools/travis.gradle'
apply from: 'tools/git.gradle'

ext {
    // Android library config
    BUILD_TOOLS_VERSION = "25.0.2"
    MIN_API_VERSION = 16
    TARGET_API_VERSION = 25
    LIBRARY_VERSION = GIT_VERSION
    // These values will be added to BuildConfig.java
    BUILD_CONFIG_CONSTS = [
            'MY_BUILD_CONFIG_VALUE': 'some://url.for/further/use'
    ]

    // Javadoc configuration
    JAVADOC_TITLE = "Javadoc for lib"

    // Bintray publish config
    BINTRAY_KEY = System.getenv('BINTRAY_KEY')   
    BINTRAY_USER = System.getenv('BINTRAY_USER')
    GROUP_ID = 'com.example.groupid'
    REPO_NAME = 'bintray-repo-name'
    ARTIFACT_ID = 'artifact'
}

// Order is important
apply from: 'tools/android.gradle'
apply from: 'tools/javadoc.gradle'
// This can be excluded if sources are not needed
apply from: 'tools/sources.gradle'
apply from: 'tools/bintray.gradle'

dependencies {
    // Your dependencies
}
```
