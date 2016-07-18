---
title: Finding project in workspace with specific maven dependency
date: 2016-07-18 20:12:48
tags:
 - Maven
 - Shell
---

### 1. Context

You have several projects in your workspace and you were wondering which projects are depending on a specific version of a maven dependency. 

This article will show you how to solve this from the command line.

### 2. Generating Maven Tree

To print the dependency tree of a Maven pom, you can run the following command:

```
$ mvn dependency:tree
```

Output for e.g. javaslang submodule would be

```
robbypelssers1@Macbook-Robby-Pelssers:~/Documents/pelssers/javaslang/javaslang$ mvn dependency:tree
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building Javaslang 3.0.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ javaslang ---
[INFO] io.javaslang:javaslang:jar:3.0.0-SNAPSHOT
[INFO] +- io.javaslang:javaslang-match:jar:3.0.0-SNAPSHOT:compile
[INFO] +- junit:junit:jar:4.12:test
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO] \- org.assertj:assertj-core:jar:3.3.0:test
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.745s
[INFO] Finished at: Mon Jul 18 20:10:16 CEST 2016
[INFO] Final Memory: 17M/309M
[INFO] ------------------------------------------------------------------------
```


### 3. Filtering on the line with specific maven dependency for e.g. hamcrest-core version 1.3

```
robbypelssers1@Macbook-Robby-Pelssers:~/Documents/pelssers/javaslang/javaslang$ mvn dependency:tree | grep org.hamcrest:hamcrest-core:jar:1.3
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
```

### 4. Finding all pom files in workspace or specific project

```
robbypelssers1@Macbook-Robby-Pelssers:~/Documents/pelssers/javaslang$ find . -name pom.xml
./javaslang/pom.xml
./javaslang-benchmark/pom.xml
./javaslang-gwt/example/pom.xml
./javaslang-gwt/pom.xml
./javaslang-match/pom.xml
./javaslang-pure/pom.xml
./javaslang-test/pom.xml
./pom.xml
```

### 5. Pipe the search results (pom files) as input for generating dependeny tree

Ideally we want to pipe all pom's as input for generating the maven dependency tree and next filter on the specific dependency.

The problem is you will ony output something like

```
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
```

So we still don't now what file lead to this output printed to the shell.

We can however use following little trick.  We basically first find all pom files, next we first make sure to print which one we are processing. Next we execute inside the folder of the respective pom the maven dependency tree command. 

Now we need to make sure we both grep for lines containing  'Processing' AND also lines containing our maven dependency.

```
javaslang$ find . -name pom.xml -exec echo 'Processing  {}' \;  -execdir  mvn -f {} dependency:tree \; | grep "org.hamcrest:hamcrest-core:jar:1.3\|Processing"
```

This will result in below output:

```
Processing  ./javaslang/pom.xml
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
Processing  ./javaslang-benchmark/pom.xml
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
Processing  ./javaslang-gwt/example/pom.xml
Processing  ./javaslang-gwt/pom.xml
Processing  ./javaslang-match/pom.xml
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
Processing  ./javaslang-pure/pom.xml
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
Processing  ./javaslang-test/pom.xml
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
Processing  ./pom.xml
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
```
