# qlever-maven-plugin
Build qlever database artifacts from data dependencies via docker.
The plugin automatically processes dependencies whose type denote RDF data, such as `nt.bz2` or `owl.gz`.
See the help section for a complete list of supported types.

## Maven Dependency

```xml
<dependency>
    <groupId>org.aksw.maven.plugins</groupId>
    <artifactId>qlever-maven-plugin</artifactId>
    <version>0.0.2</version>
</dependency>
```

Check here for the [latest published version](https://central.sonatype.com/artifact/org.aksw.maven.plugins/qlever-maven-plugin).


## Example Usage

This example builds a qlever database using a dataset dependency from our repository.
By default, Maven will install the artifact to the local repository for caching. To prevent that, use:

```
mvn clean package -Dmaven.install.skip
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <modelVersion>4.0.0</modelVersion>
  <groupId>my.org.data.text2sparql.2025.qlever</groupId>
  <artifactId>my-dbpedia</artifactId>
  <version>1</version>
  <packaging>pom</packaging>

  <build>
    <plugins>
      <plugin>
        <groupId>org.aksw.maven.plugins</groupId>
        <artifactId>qlever-maven-plugin</artifactId>
        <version>0.0.2</version>
        <executions>
          <execution>
            <goals>
              <goal>load</goal>
            </goals>
            <configuration>
              <dockerTag>commit-bb1bb54</dockerTag>
              <stxxlMemory>1g</stxxlMemory>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>

  <dependencies>
    <dependency>
      <groupId>org.aksw.data.text2sparql.2025</groupId>
      <artifactId>dbpedia</artifactId>
      <version>1.0.0</version>
      <type>ttl.bz2</type>
      <classifier>labels_en</classifier>
    </dependency>
  </dependencies>

  <repositories>
    <repository>
      <id>maven.aksw.internal</id>
      <name>AKSW Internal Repository</name>
      <url>https://maven.aksw.org/archiva/repository/internal</url>
      <releases><enabled>true</enabled></releases>
      <snapshots><enabled>false</enabled></snapshots>
    </repository>
  </repositories>

</project>
```

## Help

The plugin provides an overview of its own goals and parameters:

```bash
mvn org.aksw.maven.plugins:qlever-maven-plugin:0.0.2:help -Ddetail=true
```

This is a snapshot of the output:
```
Maven plugin for creating, loading and packaging a QLever RDF database.

This plugin has 3 goals:

qlever:help
  Display help information on qlever-maven-plugin.
  Call mvn qlever:help -Ddetail=true -Dgoal=<goal-name> to display parameter
  details.

  Available parameters:

    detail (Default: false)
      If true, display all settable properties for each goal.
      User property: detail

    goal
      The name of the goal for which to show help. If unspecified, all goals
      will be displayed.
      User property: goal

    indentSize (Default: 2)
      The number of spaces per indentation level, should be positive.
      User property: indentSize

    lineLength (Default: 80)
      The maximum length of a display line, should be positive.
      User property: lineLength

qlever:load
  Load a QLever database based on recognized RDF data dependencies.

  Available parameters:

    attachArchive (Default: true)
      Whether to attach the created archive (only applicable if an archive was
      created)

    createArchive (Default: true)
      Whether to create an archive from the database folder

    dockerImage (Default: adfreiburg/qlever)
      User property: qlever.docker.image

    dockerTag (Default: commit-a307781)
      User property: qlever.docker.tag

    files
      Mapping of extra files to graphs.

    includeTypes (Default:
    nt,ttl,nq,trig,owl,rdf,nt.gz,ttl.gz,nq.gz,trig.gz,owl.gz,rdf.gz,nt.bz2,ttl.bz2,nq.bz2,trig.bz2,owl.bz2,rdf.bz2)
      Comma separated list of dependency type suffixes which to include. A type
      matches if the string after stripping the suffix is empty or ends with a
      dot. Examples: "nt" matches the suffix "nt" because "" is the empty
      string. "rml.ttl" matches the suffix "ttl" because "rml." ends with a dot.
      "hint" does NOT match "nt" because "hi" is neither the empty string nor
      does it end with a dot.

    outputFile (Default: ${project.build.directory}/qlever.tar.gz)
      Output file (the folder as an archive)

    outputFolder (Default: ${project.build.directory}/qlever)

    skip (Default: false)
      User property: qlever.skip

    stxxlMemory
      User property: qlever.stxxlMemory

    targetGraph (Default: )
      If unspecified or blank then each dependency is loaded into its own graph
      by default. If a target graph is given then all dependencies are merged
      into this graph.
      User property: qlever.targetGraph

qlever:run
  Mojo to run a qlever database. mvn qlever:run For loading qlever database see
  QleverMojoLoad.

  Available parameters:

    accessToken
      User property: qlever.accessToken

    cacheMaxNumEntries
      User property: qlever.cacheMaxNumEntries

    cacheMaxSize
      User property: qlever.cacheMaxSize

    cacheMaxSizeSingleEntry
      User property: qlever.cacheMaxSizeSingleEntry

    containerPort (Default: 8080)
      The port in the container (qlever's -p option) - Usually there is no need
      to set this.
      User property: qlever.container.port

    dbFolder (Default: ${project.build.directory}/qlever)

    defaultQueryTimeout
      User property: qlever.defaultQueryTimeout

    dockerImage (Default: adfreiburg/qlever)
      User property: qlever.docker.image

    dockerTag (Default: commit-a307781)
      User property: qlever.docker.tag

    indexBaseName (Default: ${project.artifactId}-${project.version})
      User property: qlever.indexBaseName

    lazyResultMaxCacheSize
      User property: qlever.lazyResultMaxCacheSize

    memoryMaxSize
      User property: qlever.memoryMaxSize

    noPatterns
      User property: qlever.noPatterns

    noPatternTrick
      User property: qlever.noPatternTrick

    numSimultaneousQueries
      User property: qlever.numSimultaneousQueries

    onlyPsoAndPosPermutations
      User property: qlever.onlyPsoAndPosPermutations

    port (Default: 8080)
      The port exposed the host.
      User property: qlever.port

    serviceMaxValueRows
      User property: qlever.serviceMaxValueRows

    text
      User property: qlever.text

    throwOnUnboundVariables
      User property: qlever.throwOnUnboundVariables


```

## Changelog

### v0.2

* `qlever:load` goal now has `dockerTag` and `dockerImage` parameters wired up.

