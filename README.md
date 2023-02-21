# CVE-2022-24815

> [...] Currently, SQL injection is possible in the findAllBy(Pageable pageable, Criteria criteria) method of an entity repository class generated in these applications as the where clause using Criteria for queries are not sanitized and user input is passed on as it is by the criteria. [...]


1. [Content of the directory](#content-of-the-directory)
2. [History of the CVE](#history-of-the-cve)


## Content of the directory
This directory contains the material related to CVE-2022-24815. In particular, the content of this repository is the following:
- `README.md`: this file. It contains the report in `.md` format, with all the information regarding the CVE, the application and the exploit.
- `Runnable`: it contains the runnable vulnerable application. The application can either start locally or as a Docker container. 
- `Application`: it contains the source code of the application.
- `Images`: it contains images used in this file.

## History of the CVE
### Opened issue and pull request
### Bug bounty

## Details of the CVE


## How to generate the application
This section describes how to generate the application from scratch, using `generator-jhipster`, and configure it to replicate both the CVE vulnerability and the proposed exploit. If you would like to run the already vulnerable application, refers to the following section ([Setting up the application](#setting-up-the-application)).

> **NB**: more detailed documentation can be found either on the GitHub repository ([generator-jhipster](https://github.com/jhipster/generator-jhipster)) or on the official website ([jhipster.tech](https://www.jhipster.tech/)).

### JHipster
```bash

        ██╗ ██╗   ██╗ ████████╗ ███████╗   ██████╗ ████████╗ ████████╗ ███████╗
        ██║ ██║   ██║ ╚══██╔══╝ ██╔═══██╗ ██╔════╝ ╚══██╔══╝ ██╔═════╝ ██╔═══██╗
        ██║ ████████║    ██║    ███████╔╝ ╚█████╗     ██║    ██████╗   ███████╔╝
  ██╗   ██║ ██╔═══██║    ██║    ██╔════╝   ╚═══██╗    ██║    ██╔═══╝   ██╔══██║
  ╚██████╔╝ ██║   ██║ ████████╗ ██║       ██████╔╝    ██║    ████████╗ ██║  ╚██╗
   ╚═════╝  ╚═╝   ╚═╝ ╚═══════╝ ╚═╝       ╚═════╝     ╚═╝    ╚═══════╝ ╚═╝   ╚═╝
                            https://www.jhipster.tech
```

What is **JHipster**?
> JHipster is a development platform to quickly generate, develop, & deploy modern web applications & microservice architectures. 

It is an open-source community-driven software hosted on GitHub ([generator-jhipster](https://github.com/jhipster/generator-jhipster)). 


### Requirements
> **NB**: for the official documentation, please refers to the official website ([jhipster.tech](https://www.jhipster.tech/installation/)).

In order to install and use `generator-jhipster`, you will need to have installed on your local machine: 
- **Java SE**:  as there are several versions of Java, the one recommended here is `Eclipse Temurin`, the open source Java SE build based upon OpenJDK. However, the application has been tested using `java 17.0.6 2023-01-17 LTS`. As a reminder, to check the version of Java installed run the command: 
    ```bash
    java --version
    ```
- **Node.js**: an LTS 64-bit version is required, since non-LTS versions are not supported. As a reminder, to check the version of Node.js installed run the command: 
    ```bash
    node --version
    ```
- **Maven**: eventually, JHipster will automatically install the Maven Wrapper. As a reminder, to check the version of Maven installed run the command: 
    ```bash
    maven --version
    ```

### Installing JHipster
Once the requirements have been dealt with, `generator-jhipster` can be installed globally as a `npm` package with the command:
```bash
npm -g install generator-jhipster@<version>
```
As specified in the CVE details, in order to replicate the vulnerability, you will need to install versions from version `7.7.0` up to (and not included) version `7.8.1`. 

> **NB**: the already generated application has been tested with JHipster version `7.7.0`.

Check that the specified version has been correctly installed by running the command:
```bash
npm -g list | grep generator-jhipster
``` 

### Generate the application
To generate the vulnerable application in order to replicate the vulnerability, you will need to perform a couple a tasks:
1. generate an application with Jhipster;
2. add a new `Examples` Entity with JHipster;
3. disable API Authentication (you will not need authentication and this will speed up the process);
4. make the `/api/examples` accepts a `name` query parameter and bind it to the `findAllBy(Pageable pageable, Criteria criteria)` repository method;
5. run the exploit.

#### Generate an application with JHipster
Once a vulnerable version of JHipster has been correctly installed, you can generate a new application by creating a new folder (e.g., `vulnerableApp`) and running `jhipster`. JHipster will ask to configure the application stack. In particular, in order to replicate the vulnerability, you will need to configure the following:
    - (having a vulnerable version of JHipster);
    - using a Reactive Relational Database Connectivity (`r2dbc`) with SpringFlux;
    - using a SQL database.

Here the complete list of answers used to configure JHipster:
| JHipster | Configuration |
|:--:|:--:|
| Which *type* of application would you like to create? | Monolithic application (recommended for simple projects) |
| What is the base name of your application? | testApp |
| Do you want to make it reactive with Spring WebFlux? | ***Yes*** |
| What is your default Java package name? | com.mycompany.myapp |
| Which *type* of authentication would you like to use? | HTTP Session Authentication (stateful, default Spring Security mechanism) |
| Which *type* of database would you like to use? | SQL (H2, PostgreSQL, MySQL, MariaDB, MSSQL) |
| Which *production* database would you like to use? | ***MySQL*** |
| Which *development* database would you like to use? | H2 with in-memory persistence |
| Would you like to use Maven or Gradle for building the backend? | Maven |
| Do you want to use the JHipster Registry to configure, monitor and scale your application? | No |
| Which other technologies would you like to use? |  |
| Which *Framework* would you like to use for the client? | No client |
| Would you like to enable internationalization support? | No |
| Please choose the native language of the application | English |
| Besides JUnit and Jest, which testing frameworks would you like to use?Besides JUnit and Jest, which testing frameworks would you like to use? |  |
| Would you like to install other generators from the JHipster Marketplace? | No |

#### Add a new Entity
Once the application has been generated, you will need to create a new **Entity** (e.g., `Example`). JHipster will take care of everything. Therefore, you will only need to create a `.jdl` file, i.e. [**JHipster Domain Language**](https://www.jhipster.tech/jdl/getting-started) (**JDL**), containing the following information: 
```jdl
entity Example {
	id String
    name String
}
```
Now, you can the JHipster command: 
```bash
jhipster jdl example.jdl
```
and JHipster will generate, add to the project and manage all the files related to this new Entity. 

#### Disable API Authentication
In order to speed up the process and avoiding to sign up as a new user and log in every time, you can disable the `/api` authentication filter in `src/main/java/com/mycompany/myapp/config/SecurityConfiguration.java`. Initially, the endpoint will be marked as `authenticated()`, but we can change it: 
```java
.pathMatchers("/api/**").permitAll()
```
> **NB**: this operation has nothing to do with the exploit per se. Indeed, the CVE does not requires authentication, as stated in the CVE details: `Authentication - Not required (Authentication is not required to exploit the vulnerability.)`. The API Authentication has been removed with the intent to avoid the sign up/log in routine every time the exploit is running. However, authentication can be performed and the user (or the attacker) will be required to perform a log in operation before accessing the API.

#### Request parameter
You need now to expose an endpoint that allows users to specify a  

## Setting up the application



## Running the exploit

## Patches and fixes

## Conclusions