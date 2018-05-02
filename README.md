# Personal Package Archives (PPA) Packaging

## Quick start guide

This repository contains the modified files necessary to create a package. So once a PPA archive has been created, and a GPG key has been added to your launchpad account. 
You can try to create the package with: 

From the debian subdirectory:
```

bzr builddeb -S

```

From the created build-area directory:
```

dput ppa:<username>/ppa hello_2.7-0ubuntu2_source.changes

```

Making sure you update your naming / versioning appropriately


## Creating a PPA Package

In order to create a PPA package you first need to create a launchpad account [here](https://launchpad.net/+login).
The other two main steps are setting up a GPG key with your launchpad account see [here](http://packaging.ubuntu.com/html/getting-set-up.html) and
then I mainly followed [this tutorial](http://packaging.ubuntu.com/html/packaging-new-software.html) to build a package. For convenience the code in the hello directory already includes the changes needed to create a package. This makes it so that only the package has to be built, and then uploaded to your PPA. I think one component that might be missing here is the tar.gz itself that is used in the building process.

Once the package is built it can be uploaded to a PPA. The command to upload to your PPA can be found from your PPA url. 
For instance for me this would be `dput ppa:jnbigio/ppa <source.changes>`.

## Deviations and Errors

The main change that I made from the package tutorial was to change the version of the `hello` tar to `2.7` instead of `2.10`. I had trouble building the `2.10` version, but was able to successfully build the `2.7` version. If you do this make sure you save the tar.gz to something named `hello-2.7.tar.gz` to reflect the different version. I didn't do this initially, and it was one of the errors I ran into.

As a reference for PPA related errors you can utilize [this](https://help.launchpad.net/Packaging/UploadErrors) website.

### Specifying a section

In the control file you also need to specify a section. I tried to specify supported sections, but my PPA submissions were rejected many times. For example, even though there is a Utilities section for [Xenial](https://packages.ubuntu.com/xenial) this was rejected. The rejection process occurs when you are trying to upload a PPA and you will be notified through e-mail. Here's an example of a launchpad e-mail I got when a PPA submission was rejected: 

```

Rejected:
hello_2.10-0ubuntu1.dsc: Unknown section 'Utilities'
hello_2.10.orig.tar.gz: Unknown section 'Utilities'
hello_2.10-0ubuntu1.debian.tar.xz: Unknown section 'Utilities'
hello_2.10-0ubuntu1_amd64.deb: Unknown section 'Utilities'


Further error processing not possible because of a critical previous error.

hello (2.10-0ubuntu1) xenial; urgency=medium

```

One of the sections that finally worked was using `utils`.

### Mixed Versions

If you try to upload both a source and a binary you will receive the following error: 

```

Rejected:
Source/binary (i.e. mixed) uploads are not allowed.

```

I resolved this by building with a `-S` flag to only create the source, and by extension only upload the source.

### Versioning

If you try to reupload the same package without changing the version you will receive an error.
In fact I encountered this error when I changed the distribution version in the control file, and nothing else.

### Distro Versions

If you specify an incorrect or unsupported distro you will get an error that like: 

```

Unable to find distroseries: buster

```

I got the error above, and resolved it by using the actual version in this case which is `bionic`.
