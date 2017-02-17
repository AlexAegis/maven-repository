# maven-repository
Maven Repository

### How to create your own maven repository on GitHub

## Prerequisites 

### Setting up Java
- Download and install the JDK from http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
- Add "C:\Program Files\Java\jdk1.8.0_121\bin" to your 'PATH' variables
- Add 'JAVA_HOME' to your environment variables ("C:\Program Files\Java\jdk1.8.0_121")

### Setting up Maven
- Either use the bundled one with IntelliJ or download from https://maven.apache.org/download.cgi
- Add the Maven\bin folder to the 'PATH' variables ("D:\Program Files\JetBrains\IntelliJ IDEA\plugins\maven\lib\maven3\bin")
- In the bin folder of Maven, make a copy of "mvn.cmd" and rename it to "mvn.bat" (Without this you can't create archetypes)
- Set-up the super pom for Maven (settings.xml). You can encrypt your password with the "mvn --encrypt-master-password password" command

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

    <servers>
        <server>
            <id>SERVERID</id>
            <username>GITHUB-USERNAME</username>
            <!--<password>{ENCRYPTED-PASSWORD=}</password> Not working with site plugin-->
            <password>GITHUB-PASSWORD</password>
        </server>
    </servers>

    <profiles>
        <profile>
            <id>PROFILEID</id>
            <properties>
                <github.username>GITHUB-USERNAME</github.username>
                <github.repositoryname>GITHUB-REPOSITORYNAME</github.repositoryname>
                <github.global.server>GitHub.${github.username}</github.global.server>
                <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
            </properties>

            <repositories>
                <repository>
                    <id>REPOSITORYID</id>
                    <name>maven-repository</name>
                    <url>https://raw.github.com/${github.username}/${github.repositoryname}/mvn-repo/</url>
                    <!--<url>https://raw.github.com/${settings.servers.server.username}/maven-repository/mvn-repo/</url>-->
                    <snapshots>
                        <enabled>true</enabled>
                        <updatePolicy>always</updatePolicy>
                    </snapshots>
                </repository>
            </repositories>
        </profile>
    </profiles>

    <activeProfiles>
      <activeProfile>PROFILEID</activeProfile> 
    </activeProfiles>

</settings>
```

## 1) Creating and Hosting Archetypes

1.1) Create a project what you want to host as an archetype
- (Suggested base archetype: org.apache.maven.archetypes:maven-archetype-quickstart)
- (Suggested GroupID com.{userID}.archetypes)
- Edit it how you like
- Edit the pom as follows:
Change the identifiers. They will be inherited when the project gets created
```xml
	<groupId>${groupId}</groupId>
	<artifactId>${artifactId}</artifactId>
	<version>${version}</version>
```

## 2) Creating and Hosting Artifacts












### A1) Using Archetypes

#### A11) Manually from cli

#### A12) From IntelliJ

## B) Hosting Dependencies



+tutorial point ideas:
	- maven from cmd (creating projects from archetypes etc)