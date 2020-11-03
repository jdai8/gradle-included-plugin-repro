# Gradle included build plugin repro

```
$ ./gradlew --no-configure-on-demand :main-project:compileJava

BUILD SUCCESSFUL in 538ms
```

```
$ ./gradlew --configure-on-demand :main-project:compileJava
Configuration on demand is an incubating feature.
> Task :included:plugin:compileJava FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':included:plugin:compileJava'.
> Could not resolve all files for configuration ':included:plugin:compileClasspath'.
   > Cannot acquire state lock for project ':included' as another thread (Thread[Daemon worker Thread 11,5,main]) currently holds the state lock for all projects.

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.

* Get more help at https://help.gradle.org

BUILD FAILED in 477ms
```

This seems to involve:
- A gradle plugin as a sub-project of an included build, with a dependency on its parent project
- Configuration-on-demand
- Gradle 6.7. The build works on 6.6, but fails on 6.7 and `6.8-20201102230031+0000`
