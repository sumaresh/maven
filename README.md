### Maven Nexus configurations

## Upload artifacts to nexus through maven
add following snippet pom.xml

### Step -1
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

