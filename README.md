# backend-meets-frontend-reloaded-001
A Vaadin 10 Tutorial for Core Java Developers - Part 01

## Target of this Tutorial
Target of this tutorial is the development of an industrial style project.
We will cover here in this part
* how to initialize the project
* how to handle different JDK vendors and versions
* how to ramp up a Vaadin 10 project

Have in mind, that this is only the first part of the tutorial.

## technical information's
To explore all possibilities of this tutorial you need docker installed.
Additionally docker-compose if you want to have a look at the TDD part
that includes full stack tests with Testbench, later.

>Notice: Testbench is a commercial product from Vaadin 
>that is available here [https://vaadin.com/testbench](https://vaadin.com/testbench)
>To play around, you can request a trial lic.

### Servlet - Container
For this tutorial we are using Meecrowave from Apache Open-Webbeans - Team
This is based on Tomcat, but more in a SpringBoot style.
So we are able to manage the complete container with a few lines of code like the following.

```java
public class BasicTestUIRunner {
  private BasicTestUIRunner() { }

  public static void main(String[] args) {
    new Meecrowave(new Meecrowave.Builder() {
      {
//        randomHttpPort();
        setHttpPort(8080);
        setTomcatScanning(true);
        setTomcatAutoSetup(true);
        setHttp2(true);
      }
    })
        .bake()
        .await();
  }
}
``` 

To start the App, invoke the main method from the class **BasicTestUIRunner**.
The Meecrowave is started at Port **8080**. But have in mind, that you can use a random port 
with **randomHttpPort();**. This is very usefull for concurrent UI Tests later.
But be patient, we will explore this together..  

Meecrowave will is offering a maven plugin as well.
This is not needed if you don´t want to use it. But it is convenient if
you want to start the container inside docker for development purposes.

For this have a look at the file **docker_run_locale.sh** 
How this is done and how to use this, I will explain later, as well.


## Compile with different JDK´s
For this I prepared a bunch of Docker images with different JDK´s vendors and versions.
To use this I created the file **docker_compile_locale.sh**

```bash
docker run \
       --rm \
       --name compile \
       -v "$(pwd)":/usr/src/mymaven \
       -w /usr/src/mymaven svenruppert/maven-3.5-jdk-openjdk-10 \
       mvn clean install \
       -Dmaven.test.skip=true
```

This will create a new Docker Container and will start a **mvn clean install**
The result will be under **target**

```Happy Coding ```