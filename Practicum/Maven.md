## Install Eclipse

* Get latest Mars release of Eclipse: https://eclipse.org/downloads/
  *You can using the new Eclipse Installer tool.*
  
  Install the Eclipse IDE for Java EE Devleopers.

* Get `m2e`, to help being able to use maven from within Eclipse. 
  
  1. Help > Install New Software
  2. Add repo to site, Name: Maven, Location: http://download.eclipse.org/technology/m2e/releases/
  3. Select components, install. 

## Install Maven 3.3

Make sure to use a 64-bit version, so you can run tests with enough memory.

1. Install a binary package manager if you don't have one, e.g., for mac can use homebrew, for windows can use https://chocolatey.org/

2. Create a hello world maven project

    mvn -B archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=altcode -DartifactId=mavendemo

3. Create a project file that Eclipse can understand.

    mvn eclipse:eclipse

4. Import existing maven project into eclipse.

5. Search maven central for "mysql jdbc driver".

6. Edit `pom.xml` file to add new dependancy.

7. Run `mvn test` to run unit tests. Notice that new dependency is downloaded.

## Errors

Set `JAVA_HOME` to your home directory. *e.g.*, in Admin command window:

    setx /M JAVA_HOME "C:\Program Files\Java\jre1.8.0_45"

Set variable `M2_REPO` in Eclipse.

    mvn -Declipse.workspace="C:\Users\Chris\workspace" eclipse:configure-workspace
    
