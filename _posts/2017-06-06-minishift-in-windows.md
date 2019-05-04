---
layout: post
title: Run OpenShift locally in Windows 10
date: 2017-06-06
categories:
- cloud
- devops
- windows
tags:
- minishift
- openshift
- kubernetes
- container
- windows
status: publish
type: post
published: true
author: Budh Ram Gurung
thumbnail_path: blog/minishift.png
---

[OpenShift Origin](https://github.com/openshift/origin) is a distribution of [Kubernetes](https://github.com/kubernetes/kubernetes) offered as a Platform as a Service (PaaS) from Red Hat and comes with some cool features on top of Kubernetes. 
You can find more information about OpenShift [here](https://github.com/openshift/origin)..

In this blog, we will see how easy it is to setup a single-node cluster of OpenShift instance in Windows 10 with [Minishift](https://github.com/minishift/minishift) along with deploying a simple demo application.

For this blog, I am using PowerShell which I recommend. However, you can use the command prompt also.

## Get your Hyper-V hypervisor ready

Hyper-V is the native hypervisor comes default in Microsoft Windows 10 Professional. You need to enable it to create virtual machines.

- Refer this [link](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) to enable Hyper-V either via CMD/PowerShell or manually enable the Hyper-V role.
- Create an external [Virtual Switch](https://msdn.microsoft.com/en-us/virtualization/hyperv_on_windows/quick_start/walkthrough_virtual_switch) using the Hyper-V manager. Make sure, you select the network adapter having internet connection under **External network:** in **Connection type** while creating virtual switch.
In case, if you have multiple virtual switches, you need to set your preferred virtual switch using environment variable through PowerShell as :

    `PS C:\> $env:HYPERV_VIRTUAL_SWITCH="External (Wireless)"`

## Prepare Minishift Binary

Once, you are ready with your hypervisor, prepare binary using following instructions:

- Navigate to the Minishift [releases page](https://github.com/minishift/minishift/releases/).
- Click on **Latest release** and scroll down to **Downloads** section.
- Click on the archive with the format `minishift-<version>-windows-amd64.zip` to download it.
- Unzip it into anywhere in `%USERPROFILE%` directory (`C:\Users\budhram` in my case). See the reason [here](https://github.com/minishift/minishift/issues/236).
- Copy the contents of the directory to your preferred location.
- Add the minishift binary to your **PATH** environment variable.

## Quickstart

#### Starting Minishift

- Run the `minishift.exe start` command:

  ```
  PS C:\Users\budhram> minishift.exe start
  Starting local OpenShift cluster using 'hyperv' hypervisor...
  Downloading ISO 'https://github.com/minishift/minishift-b2d-iso/releases/download/v1.0.2/minishift-b2d.iso'
   40.00 MiB / 40.00 MiB [==============================================================================] 100.00% 0s
  Downloading OpenShift binary 'oc' version 'v1.5.1'
   19.05 MiB / 19.05 MiB [==============================================================================] 100.00% 0s
  -- Checking OpenShift client ... OK
  -- Checking Docker client ... OK
  ...
  ...
  -- Login to server ... OK
  -- Creating initial project "myproject" ... OK
  -- Removing temporary directory ... OK
  -- Checking container networking ... OK
  -- Server Information ...
     OpenShift server started.
     The server is accessible via web console at:
         https://10.65.193.168:8443

     You are logged in as:
         User:     developer
         Password: developer

     To login as administrator:
         oc login -u system:admin
  ```

- Setup OpenShift client binary `oc` as below:

  ```
  PS C:\Users\budhram> & minishift oc-env | Invoke-Expression

  # Verify 'oc' binary
  PS C:\Users\budhram> oc.exe version
  oc v1.5.1+7b451fc
  kubernetes v1.5.2+43a9be4
  features: Basic-Auth

  Server https://10.65.193.168:8443
  openshift v1.5.1+7b451fc
  kubernetes v1.5.2+43a9be4
  ```

  Note: Run `minishift.exe oc-env` to get more details about above command.

#### Deploying an application

Once you have `oc` binary ready, you can start deploying application with it as follows:

- Create a Node.js example app:

  ```
  PS C:\Users\budhram> oc new-app https://github.com/openshift/nodejs-ex -l name=myapp
  --> Found image 3b58cad (11 days old) in image stream "openshift/nodejs" under tag "4" for "nodejs"

  ...
  --> Success
      Build scheduled, use 'oc logs -f bc/nodejs-ex' to track its progress.
      Run 'oc status' to view your app.
  ```

- Track the build log until the app is built and deployed:

  ```
  PS C:\Users\budhram> oc logs -f bc/nodejs-ex
  Cloning "https://github.com/openshift/nodejs-ex" ...
          Commit: 3d44de3ba8fef0b2baca4ddd001c0db286ea4cd3 (Merge pull request #117 from openshift/revert-112-revert-111-nodejs6)
          Author: Ben Parees <bparees@users.noreply.github.com>
          Date:   Tue May 23 15:54:29 2017 -0400
  ...

  Pushing image 172.30.1.1:5000/myproject/nodejs-ex:latest ...
  Pushed 0/9 layers, 11% complete
  Pushed 1/9 layers, 14% complete
  Pushed 2/9 layers, 23% complete
  Pushed 3/9 layers, 34% complete
  Pushed 4/9 layers, 46% complete
  Pushed 5/9 layers, 57% complete
  Pushed 6/9 layers, 73% complete
  Pushed 7/9 layers, 85% complete
  Pushed 8/9 layers, 100% complete
  Pushed 9/9 layers, 100% complete
  Push successful
  ```

- Expose a route to the service(or application):

  ```
  PS C:\Users\budhram> oc expose svc/nodejs-ex
  route "nodejs-ex" exposed
  ```

- Access the application:

  ```
  PS C:\Users\budhram>  minishift openshift service nodejs-ex -n myproject
  Opening the service myproject/nodejs-ex in the default browser...
  ```

{% include image.html
           img="/blog/minishift/access-app.png"
           title="Application Access"
           class="centered"
%}

- When done with building the application for the day, stop the cluster as follows:

  ```
  PS C:\Users\budhram> minishift.exe stop
  Stopping local OpenShift cluster...
  Cluster stopped.
  ```

  If you want to access the same cluster again, use `minishift.exe start`.

- At anytime if you want to delete the cluster, use the following command:

  ```
  PS C:\Users\budhram> minishift.exe delete
  Deleting the Minishift VM...
  Minishift VM deleted.
  ```

You can see, how quickly we can get the single-node OpenShift cluster running locally in the workstation through Minishift within few minutes (based on your network connection).

## What's next?

#### Documentation

You can refer the [Minishift documentation](https://docs.openshift.org/latest/minishift/index.html) to get more details on getting start with other platforms, its features, troubleshooting guide and all the minishift command references.

#### Community

The Minishift community hangs out on the IRC channel `#minishift` on Freenode (https://freenode.net). Reach out to us for any sort of queries, suggestions or discussions.

## Resources
- [GitHub Repo](https://github.com/minishift/minishift)
- [Minishift documentation](https://docs.openshift.org/latest/minishift/index.html)
- [Blog to get OpenShift single node cluster in Linux](http://www.projectatomic.io/blog/2017/05/minishift-intro/)
