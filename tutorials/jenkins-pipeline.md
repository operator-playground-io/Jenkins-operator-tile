---
title: Create Jenkins Pipelines
description: This tutorial explains how to create Jenkins pipelines.
---

### Jenkins pipeline

* A pipeline in Jenkins (also called Jenkins Pipeline) is a collection of plugins which supports implementation and integration of continuous delivery pipelines into Jenkins.
* A plugin represents multiple Jenkins jobs as one whole workflow in the form of a pipeline.
* A continuous delivery pipeline refers to an automated expression of a process for getting software from version control up to the users and customers.

**key features of Pipelines**

* A pipeline is a Jenkin job enabled by the Pipeline plugin and created with text scripts using pipeline DSL based on the Groovy programming language.
* A pipeline makes use of the potential of multiple steps to perform simple, complex tasks based on the parameters you establish.
* Once created, a pipeline can be used to construct code and organize the work required to drive applications.

**Pipeline Functionality**

* A pipeline functionality assists Jenkins in ensuring continuous delivery.
* The key features of a pipeline functionality are as follows.
	* Durable: A pipeline can survive the scheduled and unexpected reboots of your Jenkins controller.
	* Pausable: A pipeline may eventually stop and await human intervention or approval before completing the jobs it has been built for.
	* Versatile: A pipeline assists in real-world continuous delivery requirements, and can work in parallel with other pipelines.
	* Efficient: A pipeline may restart from any of the recorded checkpoints.
	* Extensible: A Pipeline plugin offers support for custom extensions to integrate with other plugins.


### Jenkinsfile

* The definition of a Jenkins Pipeline is generally stored in a text file called a ***Jenkinsfile***.
* A Jenkinsfile saves the entire workflow as a code and it can be checked into source control.
* A Jenkinsfile is typically written using the Groovy DSL and it can be created through a text editor or through the configuration page on the Jenkins instance.

Creating a Jenkinsfile, provides many advantages including:
* Pipeline code review
* Pipeline audit trail

**Syntax**

A Jenkins pipeline supports two syntaxes which support continuous delivery requirements.
* Declarative pipeline syntax
* Scripted pipeline syntax

### Creating Jenkins pipeline

**Step 1**: Log into Jenkins and select `New item` from the dashboard.

![](_images/new-item.png)

**Step 2**: Enter a name for your pipeline and select `Pipeline` project. Click on `OK` to proceed.

![](_images/pipeline-demo.png)



**Step 3**: Scroll down to the pipeline and choose if you want a Scripted Pipeline (Pipeline Script) or Declarative pipeline(Pipeline Script from SCM).

![](_images/pipeline-option.png)

**Case 1: Scripted Pipeline (Pipeline Script):**

**Step 1: Select ‘pipeline script’ and start typing your code. (See the example script below)**

```
#!/usr/bin/env groovy
pipeline {
    agent any    
    stages {
        stage('stage one') {
            steps {
                script {
                    tags_extra = "value_1"
                }
                echo "tags_extra: ${tags_extra}"
            }
        }
        stage('stage two') {
            steps {
                echo "tags_extra: ${tags_extra}"
            }
        }
        stage('stage three') {
            when {
                expression { tags_extra != 'bla' }
            }
            steps {
                echo "tags_extra: ${tags_extra}"
            }
        }
    }
}
```

**Step 2: Click on `Apply` and `Save`.**

You have now successfully created your first Jenkins Scripted pipeline.

![](_images/pipeline-save.png)



**Case 2: Declarative pipeline (Pipeline Script from SCM):**

**Step 1:  If you want a declarative pipeline then select ‘pipeline script from SCM’ and choose your SCM. In this demo you will use Git. Enter your repository URL where you have kept your Jenkisfile.**

![](_images/pipeline.png)

**Step 2: Select the Branch. Within the script path is the name of the `Jenkinsfile` that is going to be accessed from your SCM to run.**

![](_images/scm.png)

**Step 3: Click on `Apply` and `Save`. You have successfully created your first Jenkins pipeline.**

### Build Your Jenkins Pipeline

**Step 1: Click on `Build Now`** 

![](_images/build-now.png)

You will see a stage view:

![](_images/stage-viiew.png)

**Step 2: Click on particular build like `#1` or `#2` and chose `Console Output` to see the Output.**

![](_images/console-output.png)

Below is a sample output:

![](_images/output.png)

Here you go ! You've created a pipeline successfully.
