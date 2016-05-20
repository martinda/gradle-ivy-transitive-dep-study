# Gradle transitive dependency retrieval study for non-jars

The purpose of this small project is to study Gradle's ability to retreive
transitive dependencies for non-jar projects.

## Executive summary

Yes it is possible for Gradle to resolve transitive dependencies for
non-jar and non-zip dependencies when using Ivy artifacts.

It is also possible to publish to a file system url, to an Ivy repository,
and to a Maven repository. However, the files in this repository only use
a file url repository.

## Preparation

You can run the code in this project using Gradle 2.13. Gradle was
installed with [SDKMAN](http://sdkman.io/), so there is no gradle wrapper
file in this project.

## How this study is structured

There are 2 levels of dependencies in this study, appropriately named
`level2` and `level3` (`level1` has no dependencies):

Each level is implemented as an independent Gradle project.

* `level1`: this project have no dependencies
* `level2`: this project depends on `level1`
* `level3`: this project depends on `level2`

## Executing the code

Simply clone this repository, and issue the following commands
inside the clone:

```
cd level1
gradle publish
cd ../level2
gradle publish
cd ../level3
gradle copyDeps
```

Now examine what files were copied to the `level3/build/` folder:

```
$ tree build
build
`-- layout
    |-- level1-1.0.0-bit1.8.bit
    |-- level1-1.0.0-source.txt
    |-- level1-1.0.0-zac3.1.zac
    |-- level2-2.1.0-bit1.8.bit
    `-- level2-2.1.0-zac3.1.zac
```

The files in the `level3/build/` folder contain all the dependencies for
the `level3` project, including the transitive dependencies.

# References

The following documentation was important in preparing this study:

* [Depending on modules with multiple artifacts](https://docs.gradle.org/current/userguide/dependency_management.html#ssub:multi_artifact_dependencies)
* [DependencyHandler API](https://docs.gradle.org/current/dsl/org.gradle.api.artifacts.dsl.DependencyHandler.html)
* [IvyArtifactRepository](https://docs.gradle.org/current/dsl/org.gradle.api.artifacts.repositories.IvyArtifactRepository.html)

I also prepared a similar study for [Ivy](https://github.com/martinda/ivy-transitive-dep-study).

