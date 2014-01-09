---
layout: page
title: Installation Guide
---

<ul class="nav nav-tabs">
  <li class="active"><a href="#maven" data-toggle="tab">Maven</a></li>
  <li><a href="#lein" data-toggle="tab">Lein (Clojure)</a></li>
  <li><a href="#ant" data-toggle="tab">Ant</a></li>
  <li><a href="#download" data-toggle="tab">Download source</a></li>
</ul>

<div class="tab-content">

<div class="tab-pane fade in active" id="maven">
{% capture m %}
## Maven configuration

If your project is managed using Apache Maven, then it is very easy to use the jclouds, just add the following your project's *pom.xml*:

{% highlight xml %}
<dependencies>
    <dependency>
        <groupId>org.apache.jclouds</groupId>
        <artifactId>jclouds-all</artifactId>
        <version>1.7.0</version>
      </dependency>
</dependencies>
{% endhighlight %}

### Using the daily builds

If you want to use the bleeding edge release of jclouds, you'll need to setup a maven dependency pointing to our snapshot repository. You need to update your repositories and add the following in your project's pom.xml:

{% highlight xml %}
<repositories>
    <repository>
        <id>jclouds-snapshots</id>
        <url>https://repository.apache.org/content/repositories/snapshots</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
</repositories>
<dependencies>
    <dependency>
        <groupId>org.apache.jclouds</groupId>
        <artifactId>jclouds-allcompute</artifactId>
    <version>1.8.0-SNAPSHOT</version>
    </dependency>
    <dependency>
        <groupId>org.apache.jclouds</groupId>
        <artifactId>jclouds-allblobstore</artifactId>
        <version>1.8.0-SNAPSHOT</version>
    </dependency>
</dependencies>
{% endhighlight %}

### Download the binaries
    
* Create a *pom.xml* file with dependencies you need (ex. org.apache.jclouds/jclouds-all) and the snapshot repository, if you want snapshot version (ex. 1.8.0-SNAPSHOT).
* Execute `mvn dependency:copy-dependencies`.
* You'll notice a new directory **target/dependency** with all the jars you need.
{% endcapture %}
{{ m | markdownify }}
</div>

<div class="tab-pane fade" id="lein">
{% capture m %}
## Configuring Lein

You can add jclouds to your *project.clj* like below, supporting clojure 1.2 and 1.3:

{% highlight clojure %}
:dependencies [[org.clojure/clojure "1.3.0"]
               [org.clojure/core.incubator "0.1.0"]
               [org.clojure/tools.logging "0.2.3"]
               [org.apache.jclouds/jclouds-allcompute "1.7.0"]
               [org.apache.jclouds/jclouds-allblobstore "1.7.0"]]
{% endhighlight %}

### Using the daily builds

You can add jclouds snapshots to your *project.clj* like below:

{% highlight clojure %}
  :dependencies [[org.clojure/clojure "1.3.0"]
                 [org.clojure/core.incubator "0.1.0"]
                 [org.clojure/tools.logging "0.2.3"]
                 [org.apache.jclouds/jclouds-allcompute "1.8.0-SNAPSHOT"]
                 [org.apache.jclouds/jclouds-allblobstore "1.8.0-SNAPSHOT"]]
  :repositories { "jclouds-snapshot" "https://repository.apache.org/content/repositories/snapshots"}
{% endhighlight %}

### Download the binaries

* Download [lein](https://github.com/technomancy/leiningen/raw/stable/bin/lein) and make it executable.
* Create a *project.clj* file with the below contents.
{% highlight clojure %}
(defproject deps "1" :dependencies [[org.apache.jclouds/jclouds-all "1.7.0"] [org.apache.jclouds.driver/jclouds-sshj "1.7.0"]])
{% endhighlight %}
* Execute `lein pom`, then `mvn dependency:copy-dependencies` which will fill **target/dependency** with all the jclouds jars.

Replace the *provider* and *api* in the above directory paths to the ones you want to use in your project.
{% endcapture %}
{{ m | markdownify }}
</div>

<div class="tab-pane fade" id="ant">
{% capture m %}
## Ant configuration

You will need to install [maven ant tasks](http://maven.apache.org/ant-tasks/index.html).
Then, add jclouds to your *build.xml* as shown below:

{% highlight xml %}
<artifact:dependencies pathId="jclouds.classpath">
    <dependency groupId="org.apache.jclouds"artifactId="jclouds-all" version="1.7.0" />
</artifact:dependencies>
{% endhighlight %}

### Using the daily builds

You will need to install [maven ant tasks](http://maven.apache.org/ant-tasks/index.html). Then, add jclouds snapshot dependencies to your *build.xml* as shown below:

{% highlight xml %}
<artifact:remoteRepository id="jclouds.snapshot.repository"
    url="https://repository.apache.org/content/repositories/snapshots" />
    <artifact:dependencies pathId="jclouds.classpath">
        <dependency groupId="org.apache.jclouds"
            artifactId="jclouds-all"
            version="1.8.0-SNAPSHOT" />
    <remoteRepository refid="jclouds.snapshot.repository" />
</artifact:dependencies>
{% endhighlight %}

### Download the binaries

If you want to automate fetching the jclouds binaries, you can use the following Ant script.

Install [ant](http://ant.apache.org/), copy the following into a *build.xml* file, tweaking things like 'provider' and 'driver' as necessary. The following example uses **jclouds-all**, **jclouds-sshj** as a driver, and includes the logback jars for a logging implementation.

When you run this script with *ant*, it will build a *lib* directory full of jars you can later copy into your own project.

{% highlight xml %}
<project default="sync-lib" xmlns:artifact="urn:maven-artifact-ant" >
    <target name="sync-lib" depends="initmvn">
        <delete dir="lib" />
        <mkdir dir="lib" />
        <artifact:dependencies filesetId="jclouds.fileset" versionsId="dependency.versions">
            <dependency groupId="org.apache.jclouds" artifactId="jclouds-all" version="1.7.0" />
            <dependency groupId="org.apache.jclouds.driver" artifactId="jclouds-sshj" version="1.7.0" />
            <dependency groupId="ch.qos.logback" artifactId="logback-classic" version="[1.0.9,)" />
        </artifact:dependencies>
        <copy todir="lib" verbose="true">
        <fileset refid="jclouds.fileset"/>
            <mapper type="flatten" />
        </copy>
    </target>
  
    <get src="http://search.maven.org/remotecontent?filepath=org/apache/maven/maven-ant-tasks/2.1.3/maven-ant-tasks-2.1.3.jar"
         dest="maven-ant-tasks.jar"/>
  
    <target name="initmvn">
        <path id="maven-ant-tasks.classpath" path="maven-ant-tasks.jar"/>
        <typedef resource="org/apache/maven/artifact/ant/antlib.xml"
             uri="urn:maven-artifact-ant"
             classpathref="maven-ant-tasks.classpath"/>
    </target>
</project>
{% endhighlight %}

To only fetch the jars for a particular provider replace

{% highlight xml %}
      <dependency groupId="org.apache.jclouds" artifactId="jclouds-all" version="1.7.0" />
{% endhighlight %}

with

{% highlight xml %}
      <dependency groupId="org.apache.jclouds.provider" artifactId="the-provider-id" version="1.7.0" />
{% endhighlight %}
{% endcapture %}
{{ m | markdownify }}
</div>

<div class="tab-pane fade" id="download">
{% capture m %}
## Download the last version

Last version can be downloaded from one of the [Apache mirrors](http://www.apache.org/dyn/closer.cgi/jclouds)
{% endcapture %}
{{ m | markdownify }}
</div>

</div>
