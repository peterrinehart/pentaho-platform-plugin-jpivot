# Pentaho Platform JPivot Plugin #

This is a port of the JPivot functionality that existed in Pentaho Platform 4.8 
into a stand alone 5.0 plugin.  Here are some additional details about the plugin:

- this plugin creates .xjpivot files in the 5.0 repository, although it also can execute .xaction 
  based jpivot views as well

- In order to create the _jsp.java files, a 4.8 installation was used.  These files are generated by
  tomcat from JSPs and then have been heavily modified as servlets to work within the context of a 
  Pentaho Platform plugin.

- Modifications were necessary to JPivot and WCF in order to get the system to behave correctly within
  a plugin classloader.  These changes are in jpivot-src-mods, and are based off of source code grabbed
  from the JPivot Sourceforge repository on 6-20-2013:  http://jpivot.cvs.sourceforge.net/viewvc/jpivot/

- Due to a platform upgrade to a more recent version of fop (BISERVER-7286), PDF and Excel output 
  don't work unless you downgrade to version 0.20.5, which can be found in JPivot's repository 
  project on sourceforge.  This may have negative impacts on other system components, so downgrade
  sparingly and test the rest of the system when doing so.
  
Remaining Items:
 - Test in JBoss, may require jasper.jar, etc, not sure.  These are bundled in the lib/ folder
 - Determine why filename appears with extension in folder browser

#### Pre-requisites for building the project:
* Maven, version 3+
* Java JDK 11
* This [settings.xml](https://github.com/pentaho/maven-parent-poms/blob/master/maven-support-files/settings.xml) in your <user-home>/.m2 directory

#### Building it

This will build, unit test, and package the whole project (all of the sub-modules). The artifact will be generated in: ```target```

```
$ mvn clean install
```

#### Running the tests

__Unit tests__

This will run all tests in the project (and sub-modules).
```
$ mvn test
```

If you want to remote debug a single java unit test (default port is 5005):
```
$ cd core
$ mvn test -Dtest=<<YourTest>> -Dmaven.surefire.debug
```

__Integration tests__
In addition to the unit tests, there are integration tests in the core project.
```
$ mvn verify -DrunITs
```

To run a single integration test:
```
$ mvn verify -DrunITs -Dit.test=<<YourIT>>
```

To run a single integration test in debug mode (for remote debugging in an IDE) on the default port of 5005:
```
$ mvn verify -DrunITs -Dit.test=<<YourIT>> -Dmaven.failsafe.debug
```

__IntelliJ__

* Don't use IntelliJ's built-in maven. Make it use the same one you use from the commandline.
  * Project Preferences -> Build, Execution, Deployment -> Build Tools -> Maven ==> Maven home directory