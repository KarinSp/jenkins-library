# Set up Continuous Integration for your ABAP Development using Git-Enabled CTS

Set up a pipeline for your ABAP development that enables a continuous integration process with Git-enabled CTS

## Prerequisites

* You have at least two ABAP systems in place: one development system and one target system. The ABAP systems fulfil the [prerequisites for gCTS](https://help.sap.com/viewer/4a368c163b08418890a406d413933ba7/latest/en-US/275739524c7348f0a0ab54fe1fe05954.html)
* You have configured your ABAP systems for Git-enabled CTS (gCTS) as described under [Configuring Git-Enabled Change and Transport System](https://help.sap.com/viewer/4a368c163b08418890a406d413933ba7/latest/en-US/26c9c6c5a89244cb9506c253d36c3fda.html).
* You have a Jenkins installation available that you can use for setting up project 'Piper' and for managing pipelines for your ABAP systems
* You have set up Project “Piper”. See [README](https://github.com/SAP/jenkins-library/blob/master/README.md).

## Context

Git-enabled CTS aims at supporting the [Use Cases](https://help.sap.com/viewer/4a368c163b08418890a406d413933ba7/latest/en-US/fb77f07bb9644979be357b701a4a441f.html#) Software Distribution and Continuous Integration for ABAP-based development projects. These include storing ABAP developments in Git repositories, deploying them on a target system, automatically running unit tests, and rolling back to previous coding versions in case of errors. To implement these use cases, you have to set up a pipeline. With this 'Piper' scenario, we provide a sample pipeline You can use this sample 'as is' by simply adapting the parameters described in the Configuration section below or you can set up your own pipeline with the help of the pipeline steps that are available for gCTS (look for the steps starting with 'gcts' in the section Library steps).


## Example
The sample pipeline shown here deploys a new Git commit in a target system. It then triggers the execution of ABAP unit tests on the target system. If the test result is ok, the pipeline finishes at that point. If the unit tests report errors, the latest commit that was deployed without any errors in the result of the unit tests, will be re-deployed to the target system.

Bild einfügen

In this example the following pipeline steps are used:
* [gctsDeployCommit](https://sap.github.io/jenkins-library/steps/gctsDeployCommit)
* [gctsRunUnitTestsForAllRepoPackages](https://sap.github.io/jenkins-library/steps/gctsRunUnitTestsForAllRepoPackages)
* [gctsRollbackCommit](https://sap.github.io/jenkins-library/steps/gctsRollbackCommit)

!!! Note:
    The step gctsRollbackCommit executes an immediate rollback to the last commit that was deployed successfully. This means that it activates the ABAP coding that is contained in that commit. Data that was created based on the coding of the failing commit (that is, based on coding that is no longer available after the rollback) is deleted without any notification or backup.


### Jenkinsfile

### Configuration

### Parameters
There are additional library steps available that you can use to create a repository on an ABAP system or clone the content of a remote repository to the repository on the ABAP system. You can find the details for these steps and the previously used steps including parameter descriptions in the Library steps under:
* [gctsCreateRepository](https://sap.github.io/jenkins-library/steps/gctsCreateRepository)
* [gctsCloneRepository](https://sap.github.io/jenkins-library/steps/gctsCloneRepository)
* [gctsDeployCommit](https://sap.github.io/jenkins-library/steps/gctsDeployCommit)
* [gctsRollbackCommit](https://sap.github.io/jenkins-library/steps/gctsRollbackCommit)
* [gctsRunUnitTestsForAllRepoPackages](https://sap.github.io/jenkins-library/steps/gctsRunUnitTestsForAllRepoPackages)