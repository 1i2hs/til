# sbt
## sbt-assembly : how to make executable jar/how to specify main class for scala project in `build.sbt` file?
**Date written: 2019-03-04**

you have to use `mainClass in assembly` like the following:
```
mainClass in assembly := Some("com.example.Main")
```
you can also use it in multi-project build.sbt to specify what is main class in each project:
```
lazy val commonSettings = Seq(
  version := "0.1-SNAPSHOT",
  organization := "com.example",
  scalaVersion := "2.10.1",
  test in assembly := {}
)

lazy val app = (project in file("app")).
  settings(commonSettings: _*).
  settings(
    mainClass in assembly := Some("com.example.app.Main"),
  )

lazy val utils = (project in file("utils")).
  settings(commonSettings: _*).
  settings(
    assemblyJarName in assembly := "utils.jar",
    mainClass in assembly := Some("com.example.utils.Main"),
  )
```