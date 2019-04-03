OpenShift S2I Builder for Java8 Jar App
====
This Source-to-Image Builder let's you create projects targetting Java OpenJDK 8 and built with:
* maven
* gradle

NOTE: If a project has a pom.xml and a build.gradle, maven will take precedence

Supported tags and respective `Dockerfile` links
---------

- [`s2i-java8` (*Dockerfile*)](https://github.com/shinemacro/s2i-java8/blob/master/Dockerfile)


This repository contains the source for building various versions of
the Java application as a reproducible Docker image using
[source-to-image](https://github.com/openshift/source-to-image).
Users can choose between RHEL and CentOS based builder images.
The resulting image can be run using [Docker](http://docker.io).

For more information about using these images with OpenShift, please see the
official [OpenShift Documentation](https://docs.openshift.org/latest/using_images/s2i_images/python.html).

Versions
--------
Java versions currently provided are:
* JDK 1.8 + Gradle 2 + Maven 3

CentOS versions currently supported are:
* CentOS 7

Installation
------------

To build a Java image:

  To build a Java image with Maven, you need to run the build on a properly.

    ```
    $ git clone https://github.com/shinemacro/s2i-java8.git
    $ cd s2i-java8
    $ make
    ```

  This image is also available on DockerHub. To download it run:

    ```
    $ docker pull zhaoayohong/s2i-java8:latest
    ```

Test in Openshift
----
  1.First load ImageStream:

    ```
    $ oc create -n openshift -f https://raw.githubusercontent.com/shinemacro/s2i-java8/master/s2i-java8-is.json
    ```
  
  2.Once the ImageStream s2i-java8 has been registered, you can create an template (include: route, service, is, bc, dc and pod) by Git with:
  
    ```
    $ oc create -n openshift -f https://raw.githubusercontent.com/shinemacro/s2i-java8/master/s2i-java8-git-all-template.json
    ```
  As also you can create an template (only include: bc, dc and pod) with:
  
    ```
    $ oc create -n openshift -f https://raw.githubusercontent.com/shinemacro/s2i-java8/master/s2i-java8-git-withoutservice-template.json
    ```
  You can create an template (include: route, service, is, bc, dc and pod) by SVN with:
  
    ```
    $ oc create -n openshift -f https://raw.githubusercontent.com/shinemacro/s2i-java8/master/s2i-java8-svn-all-template.json
    ```
  As also you can create an template (only include: bc, dc and pod) with:
  
    ```
    $ oc create -n openshift -f https://raw.githubusercontent.com/shinemacro/s2i-java8/master/s2i-java8-svn-withoutservice-template.json
    ```
  
  3.Click on 'Add to Project' in OpenShift CP Web Console (UI) to create a new application and then select the 's2i-java8-all' or 's2i-java8-withoutserivce' template from the 'Browse' images tab.  You will then be presented with a form where you can specify 
  * APPLICATION_NAME: A *name* for your web application.
  * APPLICATION_VERSION: A *version* for your web application, different version will create different bc, dc and pod.
  * APPLICATION_PATH(Optional): Specify the application build path, the tomcat webapps file name.
  * APPLICATION_HOSTNAME(Optional): A hostname for route
  * GIT_URI: The Git repository URL containing your Java8 application source code.
  * SVN_URI: The SVN repository URL containing your Java8 application source code.
  * SVN_USERNAME: Specify the subversion username.
  * SVN_PASSWORD: Specify the subversion user's password.
  
  Next, click on 'Create' application.  This will invoke the *S2I process* which will build your application, containerize your application (as explained above), push the newly built image into the integrated docker registry and finally deploy a Pod containing your application.
  
  Congrats! You have now successfully created your own S2I builder image for building and deploying containerized Java Web applications on OpenShift.


