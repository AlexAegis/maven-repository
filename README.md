# Maven Repository

##### How to create your own maven repository on GitHub

## Prerequisites 

### Setting up Java
- Download and install the JDK from http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
- Add "C:\Program Files\Java\jdk1.8.0_121\bin" to your 'PATH' variables
- Add 'JAVA_HOME' to your environment variables ("C:\Program Files\Java\jdk1.8.0_121")

### Setting up Maven
- Either use the bundled one with IntelliJ or download from https://maven.apache.org/download.cgi
- Add the Maven\bin folder to the 'PATH' variables ("D:\Program Files\JetBrains\IntelliJ IDEA\plugins\maven\lib\maven3\bin")
- In the bin folder of Maven, make a copy of "mvn.cmd" and rename it to "mvn.bat" (Without this you can't create archetypes)
- Set-up the super pom for Maven (settings.xml). You can encrypt your password with the "mvn --encrypt-master-password GITHUB-PASSWORD" command, but it wont work with the site plugin

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

    <localRepository>${user.home}/.m2/repository</localRepository>
    <interactiveMode>true</interactiveMode>
    <usePluginRegistry>false</usePluginRegistry>
    <offline>false</offline>

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
## 1) Creating and Hosting Artifacts

1.1) Create a project what you want to host as an artifact, or load an existing one
- (Suggested base archetype: org.apache.maven.archetypes:maven-archetype-quickstart)
- (Suggested GroupID com.{userID}.archetypes)
- Edit the pom as follows:
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- Artifact, change however you like -->

    <groupId>GROUP.I.D</groupId>
    <artifactId>ARTIFACTID</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <name>NAME</name>
    <url>http://maven.apache.org</url>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <!-- Settings, no need to edit -->

    <distributionManagement>
        <repository>
            <id>internal.repo</id>
            <name>Temporary Staging Repository</name>
            <url>file://${project.build.directory}/mvn-repo</url>
        </repository>
    </distributionManagement>

    <build>
        <directory>target</directory>
        <outputDirectory>target/classes</outputDirectory>
        <finalName>${project.artifactId}-${project.version}</finalName>
        <testOutputDirectory>target/test-classes</testOutputDirectory>
        <sourceDirectory>src/main/java/</sourceDirectory>
        <testSourceDirectory>src/test/java/</testSourceDirectory>

        <extensions>
            <extension>
                <groupId>org.apache.maven.archetype</groupId>
                <artifactId>archetype-packaging</artifactId>
                <version>2.4</version>
            </extension>
        </extensions>

        <plugins>
            <plugin>
                <artifactId>maven-archetype-plugin</artifactId>
                <version>2.4</version>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.6.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
                <executions>
                    <execution>
                        <id>default-deploy</id>
                        <phase>deploy</phase>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <artifactId>maven-deploy-plugin</artifactId>
                <version>2.8.2</version>
                <configuration>
                    <altDeploymentRepository>internal.repo::default::file://${project.build.directory}/mvn-repo
                    </altDeploymentRepository>
                </configuration>
            </plugin>

            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>${project.groupId}.Main</mainClass>
                        </manifest>
                    </archive>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>
            </plugin>

            <plugin>
                <groupId>com.github.github</groupId>
                <artifactId>site-maven-plugin</artifactId>
                <version>0.12</version>
                <configuration>
                    <message>Creating site for ${project.version}</message>
                    <noJekyll>true</noJekyll>
                    <outputDirectory>${project.build.directory}/mvn-repo</outputDirectory>
                    <branch>refs/heads/mvn-repo</branch>
                    <merge>true</merge>
                    <includes>
                        <include>**/*</include>
                    </includes>
                    <repositoryName>${github.repositoryname}</repositoryName>
                    <repositoryOwner>${github.username}</repositoryOwner>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>site</goal>
                        </goals>
                        <phase>deploy</phase>
                    </execution>
                </executions>
            </plugin>

        </plugins>
    </build>
</project>
```



## 2) Creating and Hosting Archetypes

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

## 3) Using Artifacts

- Add the repo to your project's pom (Not needed if the super pom had been configured)

```xml
  <repositories>
    <repository>
      <id>maven-repository</id>
      <url>https://raw.github.com/AlexAegis/maven-repository/mvn-repo/</url>
      <snapshots>
        <enabled>true</enabled>
        <updatePolicy>always</updatePolicy>
      </snapshots>
    </repository>
  </repositories>
```

- Import Artifacts as a dependency:

```xml
<dependency>
    <groupId>com.github.alexaegis</groupId>
    <artifactId>toolbox</artifactId>
    <version>0.0.1</version>
</dependency>
```









### A1) Using Archetypes

#### A11) Manually from cli

#### A12) From IntelliJ

## B) Hosting Dependencies



+tutorial point ideas:
	- maven from cmd (creating projects from archetypes etc)