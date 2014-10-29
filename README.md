OpenShift Deployer Plugin
=========================

[OpenShift Deployer Plugin](https://wiki.jenkins-ci.org/display/JENKINS/OpenShift+Deployer+Plugin)  enables Jenkins job to create containers(gears) on [OpenShift](http://www.openshift.com) and deploy applications to it. This plugin currently supports OpenShift v2.


Installing
----------

This plugin is available in the Jenkins plugin manager. 
To install in Jenkins, go to Manage Jenkins | Manage Plugins | Available | OpenShift Deployer Plugin | Install. 

You must restart Jenkins to complete the installation.

Configuration
-------------

SSH keys are essential when working with OpenShift. SSH keys must be uploaded to the OpenShift server in order for this Jenkins plugin to be able to deploy to OpenShift. 

If no SSH keys exist on the Jenkins server, follow these instructions to generate a pair:

```
ssh-keygen -t rsa -f /var/lib/jenkins/.ssh/id_rsa
chmod 755 /var/lib/jenkins/.ssh/
chmod 644 /var/lib/jenkins/.ssh/authorized_keys
```

In order to configure the OpenShift Deployer plugin, go to Manage Jenkins | Configure System | OpenShift. Enter the path to the public key (defaults to `/var/lib/jenkins/.ssh/id_rsa.pub`).

Add an OpenShift Server block for each OpenShift server you want to deploy to by specifying an arbitrary name, broker address, username and password. Click on "Check Login" to test the authentication. If successful, you can upload your SSH Public Key to the OpenShift server by clicking on "Upload SSH Keys". The broker address is by default openshift.redhat.com which is the address of broker for http://www.openshift.com.


Build Steps
-------------

The `Deploy to OpenShift` build-step creates an application on OpenShift and deploys the WAR/EAR file generated by your build to the created application. It also supports giving a URL (e.g. to Nexus or Artifactory) for the WAR or EAR deployment which will be fetched and deployed to OpenShift.

When building the WAR/EAR file by Maven, make sure your build is successful creating a deployable file. The `mvn package` command creates the WAR/EAR file into its `target` directory. Otherwise create the WAR/EAR in whatever way that is appropriate for you project.

After the WAR/EAR is created, add a `Deploy to OpenShift` build-step in your Jenkins job configuration and specify the OpenShift server from the list of added servers, application name, domain and cartridges for the OpenShift gear. It is also possible to choose the gear profile and the path to the WAR/EAR directory (defaults to `target`) or alternatively the URL to the WAR/EAR file.

Building & Installing from Source
-------------

1. Follow instructions on [setting up your environment](https://wiki.jenkins-ci.org/display/JENKINS/Plugin+tutorial#Plugintutorial-SettingUpEnvironment)
   from Jenkins
2. Build the plugin:
   ```
   git clone https://github.com/jenkinsci/openshift-deployer-plugin.git
   mvn clean package
   ```
3. This will create a `*.hpi` file in the `target` directory.

4. On your Jenkins instance, go to Manage Jenkins | Manage Plugins | Advanced | Upload Plugin.

5. Choose the generated `*.hpi` file and click upload.

6. Restart Jenkins.

