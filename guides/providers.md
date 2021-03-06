---
layout: page
title: Providers
permalink: /guides/providers/
---

The following is the list of providers that are supported by the jclouds API.

1. [ComputeService](#compute)
    1. [ComputeService Providers](#compute-providers)
    1. [ComputeService APIs](#compute-apis)
1. [BlobStore](#blobstore)
    1. [BlobStore Providers](#blobstore-providers)
    1. [BlobStore APIs](#blobstore-apis)
1. [LoadBalancer](#loadbalancer) (Beta)
    1. [LoadBalancer Providers](#loadbalancer-providers)

## <a id="compute"></a>ComputeService
### <a id="compute-providers"></a>Providers

The Maven Group ID for all supported providers below is [org.apache.jclouds.provider](http://search.maven.org/#search%7Cga%7C1%7Corg.apache.jclouds.provider)

<table class="table table-striped table-hover">
    <thead>
        <tr>
            <th>Provider</th>
            <th>Maven Artifact ID</th>
            <th>ISO 3166 Codes</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>AWS</td>
            <td>aws-ec2</td>
            <td>US-VA,US-CA,IE,SG</td>
        </tr>
        <tr>
            <td>Bluelock</td>
            <td>bluelock-vcloud-zone01</td>
            <td>US-IN</td>
        </tr>
        <tr>
            <td>CloudSigma US</td>
            <td>cloudsigma-lvs</td>
            <td>US-NV</td>
        </tr>
        <tr>
            <td>CloudSigma CH</td>
            <td>cloudsigma-zrh</td>
            <td>CH-ZH</td>
        </tr>
        <tr>
            <td>CloudSigma DC</td>
            <td>cloudsigma-wdc</td>
            <td>US-DC</td>
        </tr>
        <tr>
            <td>DigitalOcean</td>
            <td>digitalocean</td>
            <td></td>
        </tr>
        <tr>
            <td>ElasticHosts GB</td>
            <td>elastichosts-lon-b</td>
            <td>GB-LND</td>
        </tr>
        <tr>
            <td>ElasticHosts GB</td>
            <td>elastichosts-lon-p</td>
            <td>GB-LND</td>
        </tr>
        <tr>
            <td>ElasticHosts US</td>
            <td>elastichosts-sat-p</td>
            <td>US-TX</td>
        </tr>
        <tr>
            <td>Go2Cloud</td>
            <td>go2cloud-jhb1</td>
            <td>ZA-GP</td>
        </tr>
        <tr>
            <td>GoGrid</td>
            <td>gogrid</td>
            <td>US-CA,US-VA</td>
        </tr>
        <tr>
            <td>Green House Data</td>
            <td>greenhousedata-element-vcloud</td>
            <td>US-WY</td>
        </tr>
        <tr>
            <td>HP</td>
            <td>hpcloud-compute</td>
            <td>US-NV</td>
        </tr>
        <tr>
            <td>Ninefold</td>
            <td>ninefold-compute</td>
            <td>AU-NSW</td>
        </tr>
        <tr>
            <td>OpenHosting</td>
            <td>openhosting-east1</td>
            <td>US-VA</td>
        </tr>
        <tr>
            <td>Rackspace UK (<a href="http://www.rackspace.com/knowledge_center/article/next-gen-vs-first-gen-feature-comparison">First Gen</a>)</td>
            <td>cloudservers-uk</td>
            <td>GB-SLG</td>
        </tr>
        <tr>
            <td>Rackspace US (<a href="http://www.rackspace.com/knowledge_center/article/next-gen-vs-first-gen-feature-comparison">First Gen</a>)</td>
            <td>cloudservers-us</td>
            <td>US-IL,US-TX</td>
        </tr>
        <tr>
            <td>Rackspace UK (<a href="http://www.rackspace.com/knowledge_center/article/next-gen-vs-first-gen-feature-comparison">Next Gen</a>)</td>
            <td>rackspace-cloudservers-uk</td>
            <td>GB-SLG</td>
        </tr>
        <tr>
            <td>Rackspace US (<a href="http://www.rackspace.com/knowledge_center/article/next-gen-vs-first-gen-feature-comparison">Next Gen</a>)</td>
            <td>rackspace-cloudservers-us</td>
            <td>US-IL,US-TX</td>
        </tr>
        <tr>
            <td>SeverLove</td>
            <td>serverlove-z1-man</td>
            <td>GB-MAN</td>
        </tr>
        <tr>
            <td>SkaliCloud</td>
            <td>skalicloud-sdg-my</td>
            <td>MY-10</td>
        </tr>
        <tr>
            <td>SoftLayer</td>
            <td>softlayer</td>
            <td></td>
        </tr>
    </tbody>
</table>

### <a id="compute-apis"></a>APIs

You can also set the context property `[Artifact ID].endpoint` to use the following APIs for your private cloud.

The Maven Group ID for all supported APIs below is [org.apache.jclouds.api](http://search.maven.org/#search%7Cga%7C1%7Corg.apache.jclouds.api)

<table class="table table-striped table-hover">
    <thead>
        <tr>
            <th>Provider</th>
            <th>Maven Artifact ID</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Abiquo</td>
            <td>abiquo</td>
        </tr>
        <tr>
            <td>AWS</td>
            <td>ec2</td>
        </tr>
        <tr>
            <td>Bring Your Own Node</td>
            <td>byon</td>
        </tr>
        <tr>
            <td>CloudSigma</td>
            <td>cloudsigma</td>
        </tr>
        <tr>
            <td>CloudStack</td>
            <td>cloudstack</td>
        </tr>
        <tr>
            <td>ElasticStack</td>
            <td>elasticstack</td>
        </tr>
        <tr>
            <td>OpenStack</td>
            <td>openstack-nova</td>
        </tr>
        <tr>
            <td>vCloud</td>
            <td>vcloud</td>
        </tr>
    </tbody>
</table>


## <a id="blobstore"></a>BlobStore
### <a id="blobstore-providers"></a>Providers

The Maven Group ID for all supported providers below is [org.apache.jclouds.provider](http://search.maven.org/#search%7Cga%7C1%7Corg.apache.jclouds.provider)

<table class="table table-striped table-hover">
    <thead>
        <tr>
            <th>Provider</th>
            <th>Maven Artifact ID</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>AWS</td>
            <td>aws-s3</td>
        </tr>
        <tr>
            <td>CloudOne</td>
            <td>cloudonestorage</td>
        </tr>
        <tr>
            <td>HP</td>
            <td>hpcloud-objectstorage</td>
        </tr>
        <tr>
            <td>Microsoft</td>
            <td>azureblob</td>
        </tr>
        <tr>
            <td>Ninefold</td>
            <td>ninefold-storage</td>
        </tr>
        <tr>
            <td>Rackspace US</td>
            <td>cloudfiles-us</td>
        </tr>
        <tr>
            <td>Rackspace UK</td>
            <td>cloudfiles-uk</td>
        </tr>
    </tbody>
</table>

### <a id="blobstore-apis"></a>APIs

You can also set the context property `[Artifact ID].endpoint` to use the following APIs for your private cloud.

The Maven Group ID for all supported APIs below is [org.apache.jclouds.api](http://search.maven.org/#search%7Cga%7C1%7Corg.apache.jclouds.api)

<table class="table table-striped table-hover">
    <thead>
        <tr>
            <th>Provider</th>
            <th>Maven Artifact ID</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Atmos</td>
            <td>atmos</td>
        </tr>
        <tr>
            <td>AWS</td>
            <td>s3</td>
        </tr>
        <tr>
            <td>Filesystem</td>
            <td>filesystem</td>
        </tr>
        <tr>
            <td>OpenStack</td>
            <td>swift</td>
        </tr>
        <tr>
            <td>Rackspace</td>
            <td>cloudfiles</td>
        </tr>
    </tbody>
</table>

## <a id="loadbalancer"></a>LoadBalancer (Beta)
### <a id="loadbalancer-providers"></a>Providers

The Maven Group ID for all supported providers below is [org.apache.jclouds.provider](http://search.maven.org/#search%7Cga%7C1%7Corg.apache.jclouds.provider) and [org.apache.jclouds.labs](http://search.maven.org/#search%7Cga%7C1%7Corg.apache.jclouds.labs).

<table class="table table-striped table-hover">
    <thead>
        <tr>
            <th>Provider</th>
            <th>Maven Artifact ID</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>AWS Elastic Load Balancer</td>
            <td>aws-elb</td>
        </tr>
        <tr>
            <td>Rackspace UK</td>
            <td>rackspace-cloudloadbalancers-uk</td>
        </tr>
        <tr>
            <td>Rackspace US</td>
            <td>rackspace-cloudloadbalancers-us</td>
        </tr>
    </tbody>
</table>
