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

## 0b) Prerequisites (On Linux.Debian)

### 0b.1) Setting up Java
	- 0b.1.1 Download it through aptitude
	- 0b.1.2 WIP

### 0b.2) Setting up Maven
	- 0b.2.1 Download it through aptitude
	- 0b.2.2 WIP

## B) Creating and Hosting Archetypes

1.) Create a project what you want to host as an archetype
	- (Suggested base archetype: org.apache.maven.archetypes:maven-archetype-quickstart)
	- (Suggested GroupID com.{userID}.archetypes)


### A1) Using Archetypes

#### A11) Manually from cli

#### A12) From IntelliJ

## B) Hosting Dependencies



+tutorial point ideas:
 	- maven from cmd (creating projects from archetypes etc)