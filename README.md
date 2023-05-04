# spring-microservice-template
<a name="readme-top"></a>

## Table of Contents
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#creating-a-new-project">Creating a new project</a>
      <ul>
        <li><a href="#creating-a-new-project-with-gradle">Creating a new project with Gradle</a></li>
        <li><a href="#creating-a-new-project-manually">Creating a new project manually</a></li>
      </ul>
    </li>
    <li>
      <a href="#automated-pojo-generation">Automated POJO generation (Optional)</a>
      <ul>
        <li><a href="#generating-pojos-from-xsd">Generating POJOs from XSD</a></li>
        <li><a href="#generating-pojos-from-json-schema">Generating POJOs from JSON schema</a></li>
      </ul>
    </li>
    <li><a href="#build-and-run-the-project">Build and run the project</a></li>
  </ol>

## About The Project
<a name="about-the-project"></a>

This is a template repository which has an API and a Kafka streams example with kafka disabled by default. 
This project is intended to be used as a template to generate new project
In this template we can find:
* Controller: Used to define all the API endpoints
* Services: Used for business logic
* Data Transformer: Used to transform JSON to XML and vice versa
* Mapper: Used to map source and target beans
* TransformerTopology: Used to transform and process Kafka topic data
* *IntegrationTest: Used for Integration testing
* *Test: used for unit testing
* *.conf: Configuration files. Here the suffix represents the environment name, for e.g. `spring-microservices-template-dev.conf`, here `-dev` represents development environment. In these files we can set the Java Options like heap size, spring profiles etc.
* *.yml: Runtime configuration files where we can set the properties like spring profile, server port etc. Here also the suffix represents environment name.
* application.yml: This file can be used to define application specific properties like server port number, enabling/disabling kafka, kafka topic details etc. For e.g. We can enable Kafka as:
   ````
     kafka:
       enabled: true
   ````
* \*.log4j\*.yml: These files can be used to define application logging properties.
+ bootstrap-*.yml: This is the first file that gets loaded when the application starts, here we can define our properties like runtime resources, server port etc.
* docker-compose.yml: This file is used to containerize(docker image) the application and define the components of container.
* build.gradle: Thisfile contains the execution of a collection of tasks in a Gradle project.
* sonar-project.properties: SonarQube is a Code Quality Assurance tool that collects and analyzes source code, and provides reports for the code quality of the project. SonarQube is integrated with our Git and CI/CD pipelines.
<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Built With
<a name="built-with"/></a>

This template is built with the following technologies
* [Spring](https://spring.io)
* [Gradle](https://gradle.org)
<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Creating a new project
<a name="creating-a-new-project"/></a>

There are two options to create a project from this template:
* To execute a custom gradle task
* Generating the project doing the changes manually on the system
<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Creating a new project with Gradle
<a name="creating-a-new-project-with-gradle"/></a>

A custom task `generateProjectFromTemplate` was defined in `template-to-project.gradle` file. This custom task executes the steps 1 to 4 described in the manual project creation section.
To execute this task it is mandatory to provide the parameter `projectName` with the name of the project, for example:
````
  gradle generateProjectFromTemplate --projectName=legacy-support-service
````
Additional documentation of this task can be found in [Generate a new project from the template with gradle task](https://pivotree.jira.com/wiki/spaces/WSI/pages/4081877013/Generate+a+new+project+from+the+template+with+gradle+task)
<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Creating a new project manually
<a name="creating-a-new-project-manually"/></a>

There are 4 steps to create a project:
1. Rename and refactor the package `com.wsgc.springmicroservicetemplate` to `con.wsgc.yourappname`.Then rename the following Java classes (Change `SpringMicroserviceTemplate` to `YourAppName`)
    ````
    SpringMicroserviceTemplateApplication
    SpringMicroserviceTemplateController
    SpringMicroserviceTemplateService
    SpringMicroserviceTemplateDataTransformer
    SpringMicroserviceTemplateMapper
    SpringMicroserviceTemplateDummyRepository
    SpringMicroserviceTemplateBean
    SpringMicroserviceTemplateResponse
    SpringMicroserviceTemplateTransformerTopology
    SpringMicroserviceTemplate
    SpringMicroserviceTemplateTransformerTopologyTest
    SpringMicroserviceTemplateTopologyIntegrationTest
    SpringMicroserviceTemplateDataTransformerIntegrationTest
    SpringMicroserviceTemplateDataTransformerTest
    SpringMicroserviceTemplateServiceTest
    SpringMicroserviceTemplateControllerIntegrationTest
    SpringMicroserviceTemplateResponseTestData
    ````
2. Rename, update and/or create the application/runtime config and yaml files
   All the runtime config and yaml files are present under `runtime/conf` and `runtime/resources` directories respectively, similarly all the application yaml files are present under `main/resources` directory.
   * Rename all the files as per your app name.
   * Edit the sections of `runtime/resources/yourappname*.yml` as per your app requirements.
   * Edit the sections of `main/resources/*.yml` as per your app (for eg. `Kafka topic details`).
   * Edit the `LOG_FOLDER` property of all the config files as per your app name.
   * Delete unwanted sections from the yml files.

3. Update the gradle file: 
   * Modify mainClassName property with the renamed class name and package.
   * Modify Main-Class attribute in jar->manifest->attributes section with the renamed class name and package.
   * Modify jacocoTestCoverageVerification section with the renamed classes name and packages.

4. Update the Sonar properties file: 
   * Edit `sonar.projectName` and `sonar.projectKey` properties of the file as per your app.
<p align="right">(<a href="#readme-top">back to top</a>)</p>
   
##  Automated POJO generation (Optional)
<a name="automated-pojo-generation"/></a>

### Generating POJOs from XSD
<a name="generating-pojos-from-xsd"/></a>

The `jaxb-build.gradle` file is present under the root directory and contains a custom gradle task that uses JAXB to generate POJO classes using an XSD(XML Schema Definition) file . Here we can register a task which will get executed at the time of code build and generate the POJO classes.
* Please place your XSD file under `main/resources` directory and update the XSD file of the task `addXjcTask`.
* All the POJOs will be generated under the provided directory, that can be modified in the build script. The default directory is `build/generated/jaxb/src/main/java`. 
<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Generating POJOs from JSON schema
<a name="generating-pojos-from-json-schema"/></a>

The `json-schema-to-pojo.gradle` file can be used to generate POJO classes using a JSON file. The 'jsonSchema2Pojo' task will get executed at the time of code build and generate the POJO classes in the provided directory.
* This file is present under root directory. Source and Target directory can be defined in the build script.
  * The default source directory is `main/resources/json`.
  * The default target directory is `build/generated/js2p/src/main/java`.
* Please place your JSON file under the source directory or add a new source directory.
* All the POJOs will be generated under the target directory.
<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Build and run the project
<a name="build-and-run-the-project"/></a>

* Build the project either by using the key `ctrl+f9` or by clicking on the build icon or by using the `gradle clean build` command on terminal (if you have gradle installed).
* Now go to `*YourAppName*Application` file and run the code, in this file always remember to update the `basePackages` property with your app package name here `@ComponentScan(basePackages = {"com.wsgc.springmicroservicetemplate"})`.
<p align="right">(<a href="#readme-top">back to top</a>)</p>
