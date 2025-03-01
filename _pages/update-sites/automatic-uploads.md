---
mediawiki: Automatic_Update_Site_Uploads
title: Automatic Update Site Uploads
section: Extend:Update Sites
project: /software/imagej2
nav-links: true
nav-title: Automatic Uploads
---

{% include notice icon="info" content='This guide is intended for maintaining non-core update sites by automating builds with [Travis CI](https://travis-ci.org/).  
- The [core update sites](/update-sites/core-uploads) are updated manually or by [Travis CI](/develop/travis).
- Travis CI is useful because it can freely build any open source project with minimal effort.' %}

# Requirements

-   An open-source project hosted on [GitHub](/develop/github)
-   Logging in to [Travis CI](https://travis-ci.org/auth) with your corresponding GitHub account
-   [Travis command line tools](https://github.com/travis-ci/travis.rb#installation)
-   An [initialized upload password](/update-sites/setup#add-your-personal-update-sit).

# Additional resources

-   [Travis CI user guide](https://docs.travis-ci.com/user/getting-started/)

# Automatic Uploads via Travis CI

Travis CI can be used to automatically build a repository in response to code changes. To ease the maintenance of ImageJ update sites, we can use Travis to automatically upload the latest version of a site. This is done by creating a `.travis.yml` file in your update site's GitHub repository that does the following:

1.  Create a fresh ImageJ.app
2.  Build the update site's repository and move the required artifacts (e.g. `.jars`) to their intended locations in the ImageJ.app
3.  Upload the local update site state to your Wiki update site

As a starting point you can copy the following `.travis.yml` :

```yml
language: java
sudo: false

cache:
  directories:
        - $HOME/.m2/

install:
      - mvn package

script:
      - ./.travis-deploy.sh

branches:
  only:
        - master
```

and this script `.travis-deploy.sh` :

```shell
#!/usr/bin/env sh
set -e

# Define some variables
export USER="Username"
export UPDATE_SITE="/update-sites"

export IJ_PATH="$HOME/Fiji.app"
export URL="http://sites.imagej.net/$UPDATE_SITE/"
export IJ_LAUNCHER="$IJ_PATH/ImageJ-linux64"
export PATH="$IJ_PATH:$PATH"

# Install ImageJ
mkdir -p $IJ_PATH/
cd $HOME/
wget --no-check-certificate https://downloads.imagej.net/fiji/latest/fiji-linux64.zip
unzip fiji-linux64.zip

# Install the package
cd $TRAVIS_BUILD_DIR/
mvn clean install -Dscijava.app.directory=$IJ_PATH -Ddelete.other.versions=true

# Deploy the package
# Deploy the package
$IJ_LAUNCHER --update edit-update-site $UPDATE_SITE $URL "webdav:$USER:$WIKI_UPLOAD_PASS" .
$IJ_LAUNCHER --update update
$IJ_LAUNCHER --update upload --update-site $UPDATE_SITE --force-shadow jars/YOUR-FILE.jar
```

Don't forget to replace

```shell
export USER="Username"
export UPDATE_SITE="/update-sites"
```

by your informations.

## Encrypting your password

To upload to your wiki update site, you will need to provide Travis CI with a `WIKI_UPLOAD_PASS` environment variable, which should evaluate to the [upload password](/update-sites/setup#add-your-personal-update-site) of the Wiki account performing the upload. To do so securely, follow the instructions on the [encrypting environment variables](https://docs.travis-ci.com/user/environment-variables/#Encrypting-Variables-Using-a-Public-Key).

Note that when you run:

```shell
$ travis encrypt WIKI_UPLOAD_PASS=super_secret --add env.matrix
```

in your repository, the `.travis.yml` will automatically be updated appropriately. You can simply commit and push the changes.

## Non-Mavenized Files

Travis CI is capable of building many languages besides Java. If you cannot use Maven with a `scijava.app.directory` then you need to replace the following line of your `.travis.yml`:

      - `mvn clean install -Dscijava.app.directory="$(pwd)" -Ddelete.other.versions=true`

with a sequence of commands that will move your build artifacts to the appropriate `/jars` or `/plugins` directory, as appropriate for your update site.

This is also true if you have custom scripts, macros, etc... if these files are not present in the correct locations of the local ImageJ.app, they will appear to have been deleted.

# Caveats

{% include notice icon="warning" content="**USE CAUTION HERE**

1.  You are configuring Travis CI to upload the state of an ImageJ installation to your update site. The current working directory IS the ImageJ.app that will be uploaded. If your build artifacts are not located in the `jars` or `plugins` directory, or you don't manually copy scripts to the correct location, ImageJ will see these items as having been deleted—**effectively removing all content from your update site.** You can mitigate this danger by customizing your `.travis-deploy.sh` to download your own update site into the base ImageJ.app; only changes to the update site state will be uploaded.
2.  By default—building the master branch of your repository—your update site will be updated with **every change** to the source code. Although we encourage the master branch to be \"[release ready](/develop/releasing#phase-2-on-master)\", a safer practice would be to configure Travis CI to [only build specific branches](https://docs.travis-ci.com/user/customizing-the-build/#Building-Specific-Branches)—and set it to build [release versions](/develop/architecture#reproducible-builds) only—e.g. with a release version integration branch.
3.  Using the Maven-based `.travis.yml` as suggested implies that you are conforming to the managed dependencies of the parent pom.xml. If you are not staying up-to-date with the ImageJ and Fiji update sites (by using the latest ImageJ or Fiji [bill of materials](/develop/architecture#bill-of-materials)) then this automation may break your own update site." %}

# See Also

-   [Travis use](/develop/travis)
