# Extending Cookbooks

This is the repository dedicated for developing the **Extending Cookbooks**.

## Abstract / Description

Extending Cookbooks takes you beyond the core functionality of a cookbooks. You’ll learn how to create custom resources and Ohai plugins.  With them, you can build any custom tools you need to configure your own infrastructure. At the end of class, you’ll be ready for the unique challenges you face when managing your network.

## Learner Requirements

Participants need a network-enabled laptop with a terminal that supports SSH.

* Windows 7+ through [Putty](http://www.putty.org/) or [Cygwin with OpenSSH](https://www.cygwin.com/).

* Mac OS X 10.11

* Ubuntu 14.04

It’s best that learners have some familiarity and comfort with the following:

* Completed Chef Essentials and Test Driven Cookbook Development or equivalent experience


## Agenda

1. Introduction
2. Custom Resources
3. Creating a Custom Resource
4. Ohai Plugins
3. Creating an Ohai Plugin

## Published Content

The latest published version of these training materials are located as follows:

### Participant Guide

The participant guide is a PDF that contains the notes export from the content slides.

This content can be found here: CONTENT IS CURRENTLY IN DEVELOPMENT
### Instructor Kit

* All slides for each module

* Instructor Guide for you to learn from, practice with, and perhaps use as reference while teaching. The instructor guide contains the notes export from the content slides with additional instructor notes and lab setup instructions.

* Participant Guide

This content can be found here: CONTENT IS CURRENTLY IN DEVELOPMENT

### Screencast Videos

This content can be found here: CONTENT IS CURRENTLY IN DEVELOPMENT

## Known Issues

There are no known issues at this time

## Workstation Setup

These modules focus on getting learners engaged with the content as quickly as possible. A workstation is provided to the learners.

This workstation is currently being managed as a Amazon Machine Instance (AMI). This AMI is managed by Chef through the Training AWS Account.

* Extending Cookbooks - CentOS 6.7 - 1.0.0 (ami-????????)

> The AMI was generated with [Packer](https://github.com/chef-training/chefdk-fundamentals-image) and adheres to the following [policy](https://github.com/chef-training/chefdk-image/blob/master/cookbooks/workstations/recipes/extending_cookbooks.rb). It is based on a Marketplace AMI so it cannot be made public. If you would like access to this AMI to deliver training please contact [training@chef.io](mailto:training@chef.io) the request that includes your Amazon Account Id.

### Creating the Workstation

> An chef recipe that automates the creation of the workstation can be found in the [ChefDK Image](
https://github.com/chef-training/chefdk-image/blob/master/cookbooks/workstations/recipes/extending_cookbooks.rb
) project

* Installation of ChefDK

* Create a user named 'chef' with the password 'chef'

* Ensure the yum package repository is up-to-date

```
$ yum update -y
```

* INSTALL various editors and tools that the participant will install: vim; emacs; nano; tree; and git.

* Clone the [Extending Cookbooks Repository](https://github.com/chef-training/extending_cookbooks-repo)

* Install [Docker on CentOS](https://docs.docker.com/engine/installation/centos/)

* Allow Password Authentication

* Disable the iptables service

* Disable SELINUX
