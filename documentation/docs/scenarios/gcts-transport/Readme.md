# Set up continous integration for your ABAP development using git-enabled CTS

Set up a pipeline for your ABAP development that enables a continous integration process with git-enabled CTS

## Prerequisites

* You have at least two ABAP systems in place: one for development and one as target. These ABAP systems ahve to fulfill the [prerequisites for gCTS](https://help.sap.com/viewer/4a368c163b08418890a406d413933ba7/latest/en-US/275739524c7348f0a0ab54fe1fe05954.html)
* You have configured your ABAP systems for git-enabled CTS (gCTS) as described on the SAP Help Portal in the chapter [Configuring Git-Enabled Change and Transport System](https://help.sap.com/viewer/4a368c163b08418890a406d413933ba7/latest/en-US/26c9c6c5a89244cb9506c253d36c3fda.html).
* You a Jenkins installation available that you can use for setting up project 'piper' and for managing piplines for your ABAP systems
* You have set up Project “Piper”. See [README](https://github.com/SAP/jenkins-library/blob/master/README.md).

## Context

Git-enabled CTS aims for supporting the [Use Cases](https://help.sap.com/viewer/4a368c163b08418890a406d413933ba7/latest/en-US/fb77f07bb9644979be357b701a4a441f.html#) Software Distribution and Continuous Integration for ABAP-based development projects. gCTS in itself cannot automate these processes. A pipeline has to do the job. Please find below a sample pipeline and the library steps that are available. You can use the sample pipeline 'as is' by simply adapting the parameters descibed in the Configuration section below or you can set up your own pipeline by the help of the pipeline steps that are available for gCTS (look for the steps starting with 'gcts' in the section Library steps).


## Example
The sample pipeline shown in here deploys a new commit to a target system. It then triggers the execution of the ABAP unit test on the target system. If the testresult is ok, then the pipeline finishes at that point. If the unit tests report errors, then the latest commit that could be deployed without any errors in the result of the unit tests, will be deployed to the target system.

Bild einfügen

In this example the following pipeline steps were used:
* [gctsDeployCommit](https://sap.github.io/jenkins-library/steps/gctsDeployCommit)
* [gctsRunUnitTestsForAllRepoPackages] (https://sap.github.io/jenkins-library/steps/gctsRunUnitTestsForAllRepoPackages)
* [gctsRollbackCommit](https://sap.github.io/jenkins-library/steps/gctsRollbackCommit)

!!! note
    The step gctsRollbackCommit does an immediate rollback to the last commit that could be deployed successfully. It does NOT e.g. save any data that was created based on the coding of the error-prone commit. This means that all data that cannot be handeled via the coding that the previous commit contains will be lost or dammaged.


### Jenkinsfile

### Configuration

### Parameters
There are additional libary steps available that you can use to create or clone a repository. You can find the details for these steps and the above used ones including a parameter description for all of them in the Library steps:
* [gctsCreateRepository](https://sap.github.io/jenkins-library/steps/gctsCreateRepository)
* [gctsCloneRepository](https://sap.github.io/jenkins-library/steps/gctsCloneRepository)
* [gctsDeployCommit](https://sap.github.io/jenkins-library/steps/gctsDeployCommit)
* [gctsRollbackCommit](https://sap.github.io/jenkins-library/steps/gctsRollbackCommit)
* [gctsRunUnitTestsForAllRepoPackages] (https://sap.github.io/jenkins-library/steps/gctsRunUnitTestsForAllRepoPackages)