# Speaker Notes

You express the state of your infrastructure with resources, defined in recipes, encapsulated in cookbooks. Chef provides a core set of resources (dependent on your version of Chef and your platform). These core resources allow you to express the desired state of your infrastructure in a majority of situations. They can also be combined together to express the desired state where these individual resources fall short.

Early on when working with Chef these core resources and their ability to be combined will handle a majority of the configuration management issues that you face. After awhile you will come across more specific resource needs that have not yet been created or perhaps help describe a common set of resources you continue to use together.

When a necessary resource does not exist or when you want to express a group of resources a single resource, Chef provides a few ways to accomplish this.

After completing this module you will be able to:

* Describe the differences between Definitions, Heavy-Weight Resource Providers, Light-Weight Resource Providers and Custom Resources
* Determine when a custom resource would be beneficial for clarity and reusability

Having reached the limit of the core set of resources presents a new set of challenges before you. Fortunately these challenges are not insurmountable because of some of the design choices Chef has made to make it possible to extend its functionality.

There are few ways to accomplish this:

* Definitions
* Heavy-Weight Resource-Provider
* Light-Weight Resource-Provider
* Custom Resources

We will define these so that you understand what they accomplish and how they accomplish that goal.

* Definitions behaves like a compile time macro that is reusable across recipes. Macros all you to write a small amount of code that expands out into the contents of the definition wherever it is found within the recipes. With a definition you give it a name, provide parameters, and specify a block of code.

The code that defines the definition is stored within a definitions directory in a Ruby file that is processed with the definition Domain Specific Language. When creating a definition you specify a name and a hash of any parameters you wish to provide. Within the definition the parameters are retrievable from a hash named params.

The use of the definition within a recipe looks similar to a resource but that is not the case. Definitions cannot notify other resources, subscribe to notifications from other resources, (i.e. `notifies` and `subscribes`) and cannot employ guards (i.e. `only_if` and `not_if`).

Definitions shipped in the earlier versions of Chef and are still supported today. However, as of Chef 12.5 it is strongly recommended that you choose a solution built with custom resources.

* Heavy-Weight Resource-Provider, or HWRP, are Chef resources defined in Ruby by subclassing the core resource class and provider classes. Subclassing is an object-oriented programming term that means to inherit characteristics (e.g. methods and variables) from the parent class. Within the subclass you are required to override specific methods for the class to behave as a resource within the system.

An HWRP is as much a resource as the core resources defined in Chef. They are stored in the 'libraries' directory in a Ruby file. All the Ruby files within that directory are parsed after the cookbook is synchronized and loaded.

HWRP are incredibly useful when you need the full power of Ruby to implement your own resource. However, they come at the cost of understanding a number of Object-Oriented Programming techniques and the Ruby language. When exploring community cookbooks you may find examples of these resources in use.

* Light-Weight Resource-Provider, or LWRP, are Chef resources defined in a Domain Specific Language (DSL) that allow you to create resources without having to understand the complexity presented by HWRP.

An LWRP is as much a resource as the core resources defined in Chef. A single LWRP definition is defined in two separate files. The file is named exactly the same but one file resides in the 'resources' directory; the other in the 'providers' directory. Both of these files are parsed after the cookbook is synchronized and loaded. Each file's DSL is then converted into Ruby class at runtime.

Within the file in the 'resources' directory you define the interface for the custom resource. There, within a resource DSL, you can specify a name of the resource, the list of available actions, the default action, and the properties that may be set for the resource.

Within the file in the 'providers' directory you define the implementation for the custom resource. There, within a provider DSL, you specify what happens when an action is chosen.

Implementing resources with LWRP is not the favored way to develop a resource in later versions of Chef (Chef 12.5). However, they are still in wide use within older cookbooks like those found within the Chef Supermarket.

* Custom Resources are Chef resources defined in a Domain Specific Language (DSL) that allow you to create resources without having to understand the complexity presented by HWRP. At its core it is a simplification of the work done with LWRP.

An custom resource is as much a resource as the core resources defined in Chef. A custom resource definition is defined in a single file that resides in the 'resources' directory. This file is parsed after the cookbook is synchronized and loaded. The custom resource DSL is then converted into Ruby class at runtime.

Within the file in the 'resources' directory you define the interface and the implementation for the custom resource. This is written in a custom resource DSL where you can specify the name of the resource, the default action, the properties that may be set, and all the actions that the resource supports.

Implementing resources with a custom resource is the current favored way to develop a resource for versions of Chef 12.5 or greater.


As you can see there are more than a few ways to extend Chef and create a resource or resource-like implementation within your recipes. But before we do that, it is important to understand the value that a custom resource brings to a recipes.

As an group exercise we are going to look at a series of resources and discuss their quality. Quality can be rather variable unless we select a criteria for which to judge it.

When defining resources within our recipes we are writing software. Software has a number of quality characteristics that have already been defined. ISO/IEC 9126 is an international standard for evaluation of software quality.

> While we choose to use this standard, you may instead define our own standard or select a different one if needed.

This standard identifies 6 main quality characteristics:

* Functionality
* Reliability
* Usability
* Efficiency
* Maintainability
* Portability

> ISO 9126 Software Quality Characteristics
>
> http://www.sqa.net/iso9126.html
> https://en.wikipedia.org/wiki/ISO/IEC_9126

Let's talk about each one of these so that we have a shared understanding of what we mean when using them in this exercise.

Functionality is the essential purpose of any product or service. Does the code accomplish what it is designed to accomplish? Functionality may also be concerned with if it does so securely and within compliance guidelines.

Reliability is a judgement of whether the code accomplishes its functional goal consistently, is able to withstand fault, and recover from a failure.

Usability refers to the ease of use for the given code. Is the code easy to understand? Is it easy to learn? Does it adhere to common team standards?

Efficiency is concerned with the system resources required to achieve the functionality. We may consider the time, CPU, memory, network requirements, or physical space it takes to accomplish the intended operation.

Maintainability measures the code to see if it is supportable. If there is a failure are you able to quickly identify the issue? Are you able to easily adapt the solution? Is it testable?

Portability refers to how well the software can adapt to changes in its environment or with its requirements. This may also include evaluating code for its adaptability and maybe even be easily replaced.

## First Example

Let's examine this first example and apply our criteria:

```
execute '/usr/bin/python3/pip3 install django'
```

VS

```
pip 'django'
```

> TODO: specify the correct path and perhaps the entire recipe as the notes below state

On the left is an execute resource that specifies a path to an executable file that is passed some parameters. This command is asking, `pip3`, a Python language library management command-line tool, to install the django library on this node.

On the right we will assume is exactly the same thing, though expressed as a custom resource that has been implemented within the cookbook or a dependent cookcook.

* Functionality - Does the code accomplish what it is designed to accomplish?

Assuming that when this code executes all the URLs, file paths, executables provided are correct then I would definitely say the code is functional. Depending on how the pip3 executable behaves this will execute multiple times - how this effects the system is up to the designers of that application.

* Reliability - Will this code accomplish its job consistently?

This code is dependent on pulling the content from a source repository that may change its url structure or more likely the service may not be available. But after that assuming that the operating system successfully installs the library, with all of its dependencies, we can assume that it may be reliable.

* Usability - Is the code easy to understand? Is it easy to learn? Does it adhere to common team standards?

This may be a more subjective answer here. It uses core Chef resources that are outlined in a logical order.

* Efficiency - Does this code work within the physical constraints of the system it is deployed upon?

We may, in this case, assume that as long as you are willing to tolerate the requirements of the chef-client application running on your node then you would accept this code as efficient as any other recipe that you define.

* Maintainability - Are you able to easily adapt the solution? Is it testable?

We have learned that you can easily test resources with ChefSpec and their intended effect on virtualized hardware with InSpec. And it would seem that the solution is fairly easy to maintain by simply changing any of the values as needed to updated urls.

However, this may become more problematic based on the nature of the software. There are a few places that would need to change if version 4 of Python were to be released with an updated version. The path itself would need to change as well for both the python and pip executable. So while it may be trivial to maintain for this version but maybe not so with future versions or other platforms.

* Portability - Is this code adaptable and can it easily be replace?
