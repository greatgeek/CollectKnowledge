#### JUnit 5

#### 1. What is JUnit 5?

Unlike previous versions of JUnit, JUnit 5 is composed of several different modules from three different sub-projects.

```JUnit 5``` = ```JUnit Platform``` + ```JUnit Jupiter``` + ```JUnit Vintage```

The **JUnit Platform** serves as a foundation for launching testing frameworks on the JVM. It also defines the TestEngine API for developing a testing framework that runs on the platform. Furthermore, the platform provides a Console Launcher to launch the platform from the command line and JUnit 4 based Runner for running any TestEngine on the platform in a JUnit 4 based environment. First-class support for the JUnit Platform also exists in popular IDEs and build tools.

**JUnit Jupiter** is the combination of the new programming model and extension model for writing tests and extensions in JUnit 5. The Jupiter sub-project provides a TestEngine for running Jupiter based tests on the platform.

**JUnit Vintage** provides a TestEngine for running JUnit 3 and JUnit 4 based tests on the platform.

--------------------------------------------------------------------------------------

JUnit is a unit testing framework for Java programming language. JUnit has been important in the development of test-driven development, and is one of a family of unit testing frameworks collectively known as xUnit, that originated with JUnit.

#### 参考来源

[JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)

[JUnit Tutorial](https://www.tutorialspoint.com/junit/index.htm)