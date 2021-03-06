---
layout: page
title: "OpenStack: Getting Started Guide"
permalink: /guides/openstack/
---

1. [Introduction](#intro)
1. [Get OpenStack](#openstack)
1. [Get jclouds](#install)
1. [Terminology](#terminology)
1. [List Servers](#nova)
1. [List Containers](#swift)
1. [Next Steps](#next)
1. [OpenStack Dependencies](#pom)

## <a id="intro"></a>Introduction
[OpenStack](http://www.openstack.org/) is a global collaboration of developers and cloud computing technologists producing the ubiquitous open source cloud computing platform for public and private clouds. The project aims to deliver solutions for all types of clouds by being simple to implement, massively scalable, and feature rich. The technology consists of a series of interrelated projects delivering various components for a cloud infrastructure solution.

## <a id="openstack"></a>Get OpenStack
You can either install a private OpenStack cloud for yourself or use an existing OpenStack public cloud.

### <a id="clouds"></a>Public Clouds
Because the OpenStack API is also open, the jclouds APIs that talk to private OpenStack clouds work just as well with public OpenStack clouds. OpenStack is used by several large public clouds, both the [HP Cloud](https://www.hpcloud.com/) ([HP Cloud Getting Started Guide](/guides/hpcloud)) and [Rackspace Cloud](http://www.rackspace.com/cloud/) ([Rackspace Getting Started Guide](/guides/rackspace)) are based on it. If you don't want to sign up for a paid public cloud, you can use [TryStack](http://trystack.org/).

## <a id="install"></a>Get jclouds

1. Ensure you are using the [Java Development Kit (JDK) version 6 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
    * `javac -version`
1. Ensure you are using [Maven version 3 or later](http://maven.apache.org/guides/getting-started/maven-in-five-minutes.html).
    * `mvn -version`
1. Create a directory to try out jclouds.
    * `mkdir jclouds`
    * `cd jclouds`
1. Make a local copy of the [pom.xml](#pom) file below in the jclouds directory.
    * mvn dependency:copy-dependencies "-DoutputDirectory=./lib"
1. You should now have a directory with the following structure:
    * `jclouds/`
        * `pom.xml`
        * `lib/`
            * `*.jar`

## <a id="terminology"></a>Terminology
There are some differences in terminology between jclouds and OpenStack that should be made clear.

<div class="row clearfix">
  <div class="col-md-4 column">
    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th>jclouds</th>
          <th>OpenStack</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Compute</td>
          <td>Nova</td>
        </tr>
        <tr>
          <td>Node</td>
          <td>Server</td>
        </tr>
        <tr>
          <td>Location/Zone</td>
          <td>Region</td>
        </tr>
        <tr>
          <td>Hardware</td>
          <td>Flavor</td>
        </tr>
        <tr>
          <td>NodeMetadata</td>
          <td>Server details</td>
        </tr>
        <tr>
          <td>UserMetadata</td>
          <td>Metadata</td>
        </tr>
        <tr>
          <td>BlobStore</td>
          <td>Swift</td>
        </tr>
        <tr>
          <td>Blob</td>
          <td>File</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

## <a id="nova"></a>List Servers
### <a id="nova-intro"></a>Introduction

[OpenStack Compute](http://www.openstack.org/software/openstack-compute/) (aka Nova) is an easy to use service that provides on-demand servers that you can use to to build dynamic websites, deliver mobile apps, or crunch big data.

### <a id="nova-source"></a>The Source Code

1. Create a Java source file called JCloudsNova.java in the jclouds directory above.
1. You should now have a directory with the following structure:
    * `jclouds/`
        * `JCloudsNova.java`
        * `pom.xml`
        * `lib/`
            * `*.jar`
1. Open JCloudsNova.java for editing, read the code below, and copy it in.

{% highlight java %}
import static com.google.common.io.Closeables.closeQuietly;

import java.io.Closeable;
import java.util.Set;

import org.jclouds.ContextBuilder;
import org.jclouds.compute.ComputeService;
import org.jclouds.compute.ComputeServiceContext;
import org.jclouds.logging.slf4j.config.SLF4JLoggingModule;
import org.jclouds.openstack.nova.v2_0.NovaApi;
import org.jclouds.openstack.nova.v2_0.NovaAsyncApi;
import org.jclouds.openstack.nova.v2_0.domain.Server;
import org.jclouds.openstack.nova.v2_0.features.ServerApi;
import org.jclouds.rest.RestContext;

import com.google.common.collect.ImmutableSet;
import com.google.inject.Module;

public class JCloudsNova implements Closeable {
   private ComputeService compute;
   private RestContext<NovaApi, NovaAsyncApi> nova;
   private Set<String> zones;

   public static void main(String[] args) {
      JCloudsNova jCloudsNova = new JCloudsNova();

      try {
         jCloudsNova.init();
         jCloudsNova.listServers();
         jCloudsNova.close();
      }
      catch (Exception e) {
         e.printStackTrace();
      }
      finally {
         jCloudsNova.close();
      }
   }

   private void init() {
      Iterable<Module> modules = ImmutableSet.<Module> of(new SLF4JLoggingModule());

      String provider = "openstack-nova";
      String identity = "demo:demo"; // tenantName:userName
      String password = "devstack"; // demo account uses ADMIN_PASSWORD too

      ComputeServiceContext context = ContextBuilder.newBuilder(provider)
            .endpoint("http://172.16.0.1:5000/v2.0/")
            .credentials(identity, password)
            .modules(modules)
            .buildView(ComputeServiceContext.class);
      compute = context.getComputeService();
      nova = context.unwrap();
      zones = nova.getApi().getConfiguredZones();
   }

   private void listServers() {
      for (String zone: zones) {
         ServerApi serverApi = nova.getApi().getServerApiForZone(zone);

         System.out.println("Servers in " + zone);

         for (Server server: serverApi.listInDetail().concat()) {
            System.out.println("  " + server);
         }
      }
   }

   public void close() {
      closeQuietly(compute.getContext());
   }
}
{% endhighlight %}

In the init() method note that

* `String provider = "openstack-nova";`
  * This ones pretty self explanatory, we're using the OpenStack Nova provider in jclouds
* `String identity = "demo:demo"; // tenantName:userName`
  * Here we use the tenant name and user name with a colon between them instead of just a user name
* `String password = "devstack";`
  *  The demo account uses ADMIN_PASSWORD too
* `.endpoint("http://172.16.0.1:5000/v2.0/")`
  * This is the Keystone endpoint that jclouds needs to connect with to get more info (services and endpoints) from OpenStack
  * When the devstack installation completes successfully, one of the last few lines will read something like "`Keystone is serving at http://172.16.0.1:5000/v2.0/`"
  * Set the endpoint to this URL depending on the method used to get OpenStack above.

### <a id="nova-compile"></a>Compile and Run

    javac -classpath ".:lib/*" JCloudsNova.java

    java -classpath ".:lib/*" JCloudsNova

    [logging output]
    Servers in RegionOne
    [logging output]
      Server{uuid=...}
      ...

## <a id="swift"></a>List Containers
### <a id="swift-intro"></a>Introduction

[OpenStack Object Storage](http://www.openstack.org/software/openstack-storage/) (aka Swift) provides redundant, scalable object storage using clusters of standardized servers capable of storing petabytes of data.

### <a id="swift-source"></a>The Source Code

1. Create a Java source file called JCloudsSwift.java in the jclouds directory above.
1. You should now have a directory with the following structure:
    * `jclouds/`
        * `JCloudsSwift.java`
        * `pom.xml`
        * `lib/`
            * `*.jar`
1. Open JCloudsSwift.java for editing, read the code below, and copy it in.

{% highlight java %}
import static com.google.common.io.Closeables.closeQuietly;

import java.io.Closeable;
import java.util.Set;

import org.jclouds.ContextBuilder;
import org.jclouds.blobstore.BlobStore;
import org.jclouds.blobstore.BlobStoreContext;
import org.jclouds.logging.slf4j.config.SLF4JLoggingModule;
import org.jclouds.openstack.swift.CommonSwiftAsyncClient;
import org.jclouds.openstack.swift.CommonSwiftClient;
import org.jclouds.openstack.swift.domain.ContainerMetadata;
import org.jclouds.rest.RestContext;

import com.google.common.collect.ImmutableSet;
import com.google.inject.Module;

public class JCloudsSwift implements Closeable {
   private BlobStore storage;
   private RestContext<CommonSwiftClient, CommonSwiftAsyncClient> swift;

   public static void main(String[] args) {
      JCloudsSwift jCloudsSwift = new JCloudsSwift();

      try {
         jCloudsSwift.init();
         jCloudsSwift.listContainers();
         jCloudsSwift.close();
      }
      catch (Exception e) {
         e.printStackTrace();
      }
      finally {
         jCloudsSwift.close();
      }
   }

   private void init() {
      Iterable<Module> modules = ImmutableSet.<Module> of(
            new SLF4JLoggingModule());

      String provider = "swift-keystone";
      String identity = "demo:demo"; // tenantName:userName
      String password = "devstack"; // demo account uses ADMIN_PASSWORD too

      BlobStoreContext context = ContextBuilder.newBuilder(provider)
            .endpoint("http://172.16.0.1:5000/v2.0/")
            .credentials(identity, password)
            .modules(modules)
            .buildView(BlobStoreContext.class);
      storage = context.getBlobStore();
      swift = context.unwrap();
   }

   private void listContainers() {
      System.out.println("List Containers");
      Set<ContainerMetadata> containers = swift.getApi().listContainers();

      for (ContainerMetadata container: containers) {
         System.out.println("  " + container);
      }
   }

   public void close() {
      closeQuietly(storage.getContext());
   }
}
{% endhighlight %}

### <a id="swift-compile"></a>Compile and Run

    javac -classpath ".:lib/*" JCloudsSwift.java

    java -classpath ".:lib/*" JCloudsSwift

    [logging output]
    List Containers
    [logging output]
      ContainerMetadata{name=...}
      ...

## <a id="next"></a>Next Steps

* Try the example above with a keystone endpoint that defines multiple storage regions. For this init() becomes:

{% highlight java %}
private void init() {
   Iterable<Module> modules = ImmutableSet.<Module> of(new SLF4JLoggingModule());

   String provider = "swift-keystone"; //Region selection is limited to swift-keystone provider
   String identity = "demo:demo";
   String password = "devstack";
   String endpoint = "http://keystone-endpoint.example.com/v2.0";
   String region = "RegionOne";

   // If your keystone endpoint has multiple storage regions
   // then it is recommended that you specify which region to use.
   // You can do this via the "jclouds.region" property
   Properties properties = new Properties();
   properties.setProperty(LocationConstants.PROPERTY_REGION, region);

   BlobStoreContext context = ContextBuilder.newBuilder(provider)
         .endpoint(endpoint)
         .credentials(identity, password)
         .modules(modules)
         .overrides(properties)
         .buildView(BlobStoreContext.class);
   storage = context.getBlobStore();
   swift = context.unwrap();
}
{% endhighlight %}

* Try the example above with one of the public clouds. For the Rackspace Cloud init() becomes:

{% highlight java %}
private void init() {
   Iterable<Module> modules = ImmutableSet.<Module> of(new SLF4JLoggingModule());
   Properties overrides = new Properties();
   overrides.setProperty(KeystoneProperties.CREDENTIAL_TYPE, CredentialTypes.PASSWORD_CREDENTIALS);
   overrides.setProperty(Constants.PROPERTY_API_VERSION, "2");

   String provider = "openstack-nova";
   String identity = "myUsername"; // userName
   String password = "myPassword";

   ComputeServiceContext context = ContextBuilder.newBuilder(provider)
         .endpoint("https://identity.api.rackspacecloud.com/v2.0/")
         .credentials(identity, password)
         .modules(modules)
         .overrides(overrides)
         .buildView(ComputeServiceContext.class);
   compute = context.getComputeService();
   nova = context.unwrap();
   zones = nova.getApi().getConfiguredZones();
}
{% endhighlight %}

* Try using the `"openstack-cinder"` provider to list volumes (hint: see [VolumeAndSnapshotApiLiveTest.testListVolumes()](https://github.com/jclouds/jclouds/blob/master/apis/openstack-cinder/src/test/java/org/jclouds/openstack/cinder/v1/features/VolumeAndSnapshotApiLiveTest.java)).
* Have a look at how the optional extensions are handled (hint: see [FloatingIPApiLiveTest.testListFloatingIPs()](https://github.com/jclouds/jclouds/blob/master/apis/openstack-nova/src/test/java/org/jclouds/openstack/nova/v2_0/extensions/FloatingIPApiLiveTest.java#L42)).
* Change the example to do different things that you want to do.
* Browse the [Javadoc](http://demobox.github.com/jclouds-maven-site/latest/apidocs).
* Join the [jclouds community](/community/) as either a developer or user.

## <a id="pom"></a>OpenStack Dependencies

This pom.xml file specifies all of the dependencies you'll need to work with OpenStack. Replace the jclouds.version X.X.X with the latest version available according to the [Release Notes](/releasenotes/).

{% highlight xml %}
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <properties>
    <jclouds.version>X.X.X</jclouds.version>
  </properties>
  <groupId>org.apache.jclouds.examples</groupId>
  <artifactId>openstack-examples</artifactId>
  <version>1.0</version>
  <dependencies>
    <!-- jclouds dependencies -->
    <dependency>
      <groupId>org.apache.jclouds.driver</groupId>
      <artifactId>jclouds-slf4j</artifactId>
      <version>${jclouds.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.jclouds.driver</groupId>
      <artifactId>jclouds-sshj</artifactId>
      <version>${jclouds.version}</version>
    </dependency>
    <!-- OpenStack dependencies -->
    <dependency>
      <groupId>org.apache.jclouds.api</groupId>
      <artifactId>openstack-keystone</artifactId>
      <version>${jclouds.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.jclouds.api</groupId>
      <artifactId>openstack-nova</artifactId>
      <version>${jclouds.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.jclouds.api</groupId>
      <artifactId>swift</artifactId>
      <version>${jclouds.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.jclouds.api</groupId>
      <artifactId>openstack-cinder</artifactId>
      <version>${jclouds.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.jclouds.api</groupId>
      <artifactId>openstack-trove</artifactId>
      <version>${jclouds.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.jclouds.labs</groupId>
      <artifactId>openstack-glance</artifactId>
      <version>${jclouds.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.jclouds.labs</groupId>
      <artifactId>openstack-marconi</artifactId>
      <version>${jclouds.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.jclouds.labs</groupId>
      <artifactId>openstack-neutron</artifactId>
      <version>${jclouds.version}</version>
    </dependency>
    <!-- 3rd party dependencies -->
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.0.13</version>
    </dependency>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.25</version>
    </dependency>
  </dependencies>
</project>
{% endhighlight %}
