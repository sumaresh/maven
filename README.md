# Maven Nexus configurations

## Upload artifacts to nexus through maven

### Step -1
Add following snippet pom.xml
```
<!-- Keep this code outside dependencies tag-->
<distributionManagement>
		 <snapshotRepository>
		    <id>nexus</id>
		    <url>http://172.31.15.236:8081/repository/maven-snapshots/</url>
		 </snapshotRepository>
		
		<repository>
		    <id>nexus</id>
		    <url>http://172.31.15.236:8081/repository/maven-releases/</url>
		</repository>
  	</distributionManagement>

```
### Step -2
Add following snippent in $MAVEN_HOME/conf/settings.xml

```

 <servers>
    <server>
      <id>nexus</id>
      <username>admin</username>
      <password>admin</password>
    </server>
  </servers>
```

### Step-3
Run maven command to deploy artifacts to nexus

```
 mvn clean deploy
```


## Download artifacts from nexus

### Step-1
Create poxy repository in nexus

create group repository with (release, snapshot, proxy)
### Step-2

Add following configurations in $MAVEN_HOME/conf/settings.xml


```

 <mirrors>
    <mirror>
      <id>central</id>
      <name>central</name>
      <url>http://172.31.34.129:8081/repository/javahome-group/</url>
      <mirrorOf>*</mirrorOf>
    </mirror>
  </mirrors>

```

### Full settings.xml file should look like this.

```
  <settings>
  <mirrors>
    <mirror>
      <!--This sends everything else to /public -->
      <id>nexus</id>
      <mirrorOf>*</mirrorOf>
      <url>http://localhost:8081/repository/maven-proxy-test/</url>
    </mirror>
  </mirrors>
  <profiles>
    <profile>
      <id>nexus</id>
      <!--Enable snapshots for the built in central repo to direct -->
      <!--all requests to nexus via the mirror -->
      <repositories>
        <repository>
          <id>central</id>
          <url>http://central</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
        </repository>
      </repositories>
     <pluginRepositories>
        <pluginRepository>
          <id>central</id>
          <url>http://central</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
        </pluginRepository>
      </pluginRepositories>
    </profile>
  </profiles>
  <activeProfiles>
    <!--make the profile active all the time -->
    <activeProfile>nexus</activeProfile>
  </activeProfiles>
</settings>
```

