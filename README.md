# jasmine-maven
This project is implementation of the Jasmine-Maven-Plugin along with code coverage.

Below are the steps for setting up the Jasmine-Maven-Plugin
1) Create a simple Maven project in eclipse without selecting the archetype
2) Edit the POM file and add the below code

<build>
  <plugins>
    <plugin>
      <groupId>com.github.searls</groupId>
      <artifactId>jasmine-maven-plugin</artifactId>
      <version>2.1</version>
      <executions>
        <execution>
          <goals>
            <goal>test</goal>
          </goals>
        </execution>
      </executions>
      <!-- keep the configuration out of the execution so that the bdd goal has access to it -->
      <configuration>
        <!-- configuration properties will go here -->
      </configuration>
    </plugin>
	</plugins>
</build>

3) Save the POM file and use mvn jasmine:bdd from command line from project root folder

4) If you receive the below error. Then add the codehaus dependency
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ JasmineBDD ---
[WARNING] Error injecting: org.codehaus.plexus.compiler.javac.JavacCompiler
java.lang.NoClassDefFoundError: org/codehaus/plexus/util/cli/CommandLineException

Complete POM file:
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.jasmine.test</groupId>
  <artifactId>JasmineBDD</artifactId>
  <version>0.0.1-SNAPSHOT</version>
<build>
  <plugins>
    <plugin>
      <groupId>com.github.searls</groupId>
      <artifactId>jasmine-maven-plugin</artifactId>
      <version>2.1</version>
      <executions>
        <execution>
          <goals>
            <goal>test</goal>
          </goals>
        </execution>
      </executions>
      <!-- keep the configuration out of the execution so that the bdd goal has access to it -->
      <configuration>
        <!-- configuration properties will go here -->
      </configuration>
    </plugin>
	<plugin>
	  <artifactId>maven-compiler-plugin</artifactId>
        <version>3.1</version>
   		<dependencies>
			<dependency>
			<groupId>org.codehaus.plexus</groupId>
			<artifactId>plexus-compiler-javac</artifactId>
			<version>2.2</version>
			<scope>runtime</scope>
			</dependency>
          </dependencies> 
        <executions>
          <execution>
            <id>default-testCompile</id>
            <phase>test-compile</phase>
            <goals>
              <goal>testCompile</goal>
            </goals>
          </execution>
          <execution>
            <id>default-compile</id>
            <phase>compile</phase>
            <goals>
              <goal>compile</goal>
            </goals>
          </execution>
        </executions>
	</plugin>    
  </plugins>
</build>
</project>

5) Save the POM file and rerun the mvn jasmine:bdd from command line. The below console output should come

D:\EclipseInfinitestWS\JasmineBDD>mvn jasmine:bdd
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building JasmineBDD 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] --- jasmine-maven-plugin:2.1:bdd (default-cli) @ JasmineBDD ---
[INFO] jetty-8.1.18.v20150929
[INFO] Started SelectChannelConnector@0.0.0.0:8234
[INFO]

Server started--it's time to spec some JavaScript! You can run your specs as you develop by visiting
 this URL in a web browser:

 http://localhost:8234

The server will monitor these two directories for scripts that you add, remove, and change:

  source directory: src/main/javascript

  spec directory: src/test/javascript

Just leave this process running as you test-drive your code, refreshing your browser window to re-run your specs. You can kill the server with Ctrl-C when you're done.

6) For code coverage add the following to your POM file

    <plugin>
      <groupId>com.github.timurstrekalov</groupId>
      <artifactId>saga-maven-plugin</artifactId>
      <version>1.4.0</version>
      <executions>
        <execution>
          <goals>
            <goal>coverage</goal>
          </goals>
        </execution>
      </executions>
      <configuration>
        <baseDir>http://localhost:${jasmine.serverPort}</baseDir>
        <outputDir>${project.basedir}/target/coverage</outputDir>
        <noInstrumentPatterns>
          <pattern>.*/spec/.*</pattern> <!-- Don't instrument specs -->
        </noInstrumentPatterns>
      </configuration>
    </plugin>     

Type the command mvn verify for getting the coverage report


For further customization refer the site: http://searls.github.io/jasmine-maven-plugin/index.html

For any clarification regarding this sample project please drop me a note on manas.sahu@gmail.com.
