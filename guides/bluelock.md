---
layout: page
title: "BlueLock vCloud: Getting Started Guide"
---

**Please note that the support for BlueLock vCloud Director is not complete**

1. Sign up for [BlueLock vCloud](http://www.bluelock.com/cloud-services/)
2. Ensure you are using a recent JDK 6
3. Setup your project to include bluelock-vcdirector
	a. Get the dependency `org.jclouds.provider/bluelock-vcdirector` using jclouds [Installation](/start/install).
4. Start coding.

{% highlight java %}

ComputeServiceContext context = ContextBuilder.newBuilder("bluelock-vcdirector")
                      .credentials("username@orgname", password)
                      .modules(ImmutableSet.<Module> of(new Log4JLoggingModule(),
                                                        new SshjSshClientModule()))
                      .buildView(ComputeServiceContext.class);

// create a customization script to run when the machine starts up
String script = "cat > /root/foo.txt<<EOF\nI love candy\nEOF\n";
TemplateOptions options = client.templateOptions();
options.as(VCloudTemplateOptions.class).customizationScript(script);

// run a couple nodes accessible via group
nodes = context.getComputeService().createNodesInGroup("webserver", 2, options);

// get a synchronous object to use for manipulating vcloud objects in BlueLock
VCloudClient client =
	VCloudClient.class.cast(context.getProviderSpecificContext().getApi());

// look at all the thumbnails of my vApps
VApp app = client.getVApp(vApp.getHref());

for (Vm vm : app.getChildren()) {
     assert client.getThumbnailOfVm(vm.getHref()) != null;
}

// release resources
context.close();

{% endhighlight %}

6. Run on [BlueLock](http://www.bluelock.com/cloud-services/)
