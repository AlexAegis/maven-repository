# maven-repository
Maven Repository

### How to create your own maven repository on GitHub

## 0a) Prerequisites (On Windows)

### 0a.1) Setting up Java
	- 0a.1.1 Download the JDK from http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
	- 0a.1.2 Install it
	- 0a.1.3 Add "C:\Program Files\Java\jdk1.8.0_121\bin" to your 'PATH' variables
	- 0a.1.4 Add 'JAVA_HOME' to your environment variables ("C:\Program Files\Java\jdk1.8.0_121")

### 0a.2) Setting up Maven
	- 0a.2.1 Either use the bundled one with IntelliJ or download from https://maven.apache.org/download.cgi
	- 0a.2.2 Add the Maven\bin folder to the 'PATH' variables ("D:\Program Files\JetBrains\IntelliJ IDEA\plugins\maven\lib\maven3\bin")
	- 0a.2.3 In the bin folder of Maven, make a copy of "mvn.cmd" and rename it to "mvn.bat" Without this you can't create archetypes
	- 0a.2.4 Provide GitHub login for Maven in the 'settings.xml'. You can encrypt your password with the "mvn --encrypt-master-password password" command

		```xml
		    <servers>
	        	<server>
		            <id>id</id>
		            <username>username</username>
		            <password>{password=}</password>
        		</server>
    		</servers>
		```
	- 0a.2.5 Set up a profile and provide location for the repo

		```xml
	    <profiles>
	        <profile>
	            <id>Profile.ID</id>
	            <repositories>
	                <repository>
	                    <id>Repository.ID</id>
	                    <name>maven-repository</name>
	                    <url>https://raw.github.com/${settings.servers.server.username}/maven-repository/mvn-repo/</url>
	                    <snapshots>
	                        <enabled>true</enabled>
	                        <updatePolicy>always</updatePolicy>
	                    </snapshots>
	                </repository>
	            </repositories>
	        </profile>
	    </profiles>
		```

### 0.3) Setting up GitHub
	- 0.3.1 Create a new GitHub repository
	- 0.3.2 Create a new branch called "mvn-repo" on it

## 1) Creating and Hosting Archetypes

1.1) Create a project what you want to host as an archetype
	- 1.1.1 (Suggested base archetype: org.apache.maven.archetypes:maven-archetype-quickstart)
	- 1.1.2 (Suggested GroupID com.{userID}.archetypes)
	- 1.1.3 Edit it how you like
	- 1.1.4 Edit the pom as follows:
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