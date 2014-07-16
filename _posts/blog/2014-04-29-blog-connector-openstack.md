---
layout: article
title: How to Configure Your OpenStack SlipStream Connector
category: blog
image: /img/design/slipstream_category.png
author: Marc-Elian Bégin / Lionel Schaub
comments: true
---

*Update: Removed the parapgraph about licensing options for OpenStack connector since
it's open-source and available for free.*

The following instructions will guide you through the procedure of configuring
a SlipStream OpenStack connector, such that you can add your OpenStack cloud
to the list of clouds your SlipStream can talk to.


## Prerequisite

This installation recipe assumes that you have already installed the
SlipStream service on the same server. For details on how to install the
service, please refer to the [online documentation](https://slipstream.sixsq.com/html/administrator-manual.html).


## Install the connector

The OpenStack connector is currently bundled with the SlipStream service. You
therefore don't need to do anything special to have access to the connector
software, once you have installed the service.


## Load the connector in SlipStream

Once the connector software is installed, you can configure SlipStream to load
this connector by either setting the parameter in the configuration file (i.e.
`/etc/slipstream/slipstream.conf`) or via the web interface.


### Configuration file

The list of connector instances SlipStream uses is defined by the
`cloud.connector.class` configuration parameter:

    # cat /etc/slipstream/slipstream.conf
    # SlipStream(tm) Server configuration file
    ...

    cloud.connector.class = com.sixsq.slipstream.connector.openstack.OpenStackConnector

    ...

By default, the connector instance will be named *openstack*. You can change
the name of the connector by prepending the name followed by a semi colon
(i.e. `:`). Here is an example:

    cloud.connector.class = my-openstack:com.sixsq.slipstream.connector.openstack.OpenStackConnector

You can also instantiate the connector several times (in compliance with your
license) by comma separating the connector string. Here is an example:

    cloud.connector.class = my-os-1:com.sixsq.slipstream.connector.openstack.OpenStackConnector, my-os-2:com.sixsq...

Once the configuration file is set, login to SlipStream as a privileged user
and load the configuration. The configuration page is available by clicking on
the spanner icon at the top right of the screen. To load the changes made in
the configuration file, simply click on the **Reload Configuration File**
button.  You should then see the changed value in the *SlipStream Basics*
section.


### Configuration page

You can also define which connector to load, as per the Configuration file
section above, using the web user interface.  Once logged-in with a privileged
user (e.g. *super*), open the configuration page by clicking on the spanner
icon at the top right of the screen.  Then open the *SlipStream Basics*
section and drop in the value of the configuration, as per described in the
section above. Here is a screenshot of the parameter to define:

<p align="center"><img src="/img/content/blogs/doc_OpenStack_ss_system_parameters.png" alt="SlipStream Configuation - OpenStack section" class="shadow"  width="900" /></p>

**Don't forget to save the configuration!**

Now that the connector is loaded, you need to configure it.


**Super/privileged user**

By default, SlipStream configures a privileged user called *super*, with the
default password `supeRsupeR`. **Please make sure you change this default
password.**


## Configure the connector instance in SlipStream

Once the connector is loaded, you need to configure the connector.  While you
can also define these parameters using the configuration file, we only show
here how to do it using the web user interface.

With the connector loaded in SlipStream, a new section in the configuration
page will appear, allowing you to configure how the connector is to
communicate with the IaaS cloud endpoint.

**Type name of the service which provides the instances functionality**

This field should always be `compute`. If it doesn't work with this value,
please ask your OpenStack provider/administrator for the correct value.


**Service endpoint**

The service endpoint is the URL SlipStream will use to communicate with the
OpenStack. This url needs to match the OpenStack identity service (Keystone).
You can find it in the OpenStack dashboard under *Access & Security* in the
tab *API Access*. Most of the time this value will match the following
pattern: `https://OpenStack_ip:5000/v2.0`

<p align="center"><img src="/img/content/blogs/doc_OpenStack_endpoint.png" alt="Openstack web interface - Access & Security - API Access" class="shadow"  width="750" /></p>

**Quota**

The quota is SlipStream feature which enable the SlipStream administrator to
set a default quota for all users of a specified connector. You can also
override this value per user in the user profile. If this feature is disabled
in the *SlipStream Advanced* section of this page, you can leave this field
blank.


**Image Id of the Orchestrator**

The image id of the Orchestrator needs to match a Linux image with `wget` and
`python` installed. An Ubuntu 12.04 will do the job perfectly.

To find an image id go on the OpenStack web interface and click on the link
named *Images & Snapshots* and then click on the image you want. The *ID*
value is what you need to paste in.

<p align="center"><img src="/img/content/blogs/doc_OpenStack_imageId.png" alt="Openstack web interface - Image details" class="shadow"  width="750" /></p>


**Region**

Check this value in the OpenStack documentation or ask your OpenStack
administrator.  The default region is `RegionOne` or `regionOne` depending of
the OpenStack version.


**Name of the service which provides the instances functionality**

Most of time the value of this field will be `nova` and sometime `compute`. If
it doesn't work with these values, please ask your OpenStack administrator for
the correct value.


**Flavor of the Orchestrator**

The flavor (instance type) is a name which is linked to a hardware
specification defined by the Cloud. To find the list of all possible values,
please go on the OpenStack web interface and find a link called *Flavor* or
*Instance type*. The Orchestrator doesn't need a big amount of resources so
you can choose a small flavor (like 1 CPU and 512 MB of RAM).


## Configure native images for this connector instance

Now you need to update SlipStream native images to add the image id and some
parameters for OpenStack.

Please go on a SlipStream base image (e.g. Ubuntu 12.04) and click on the
*Edit* button. Add the image id for OpenStack in the section named *Cloud
Image Identifiers and Image Hierarchy*.

And then configure the default amount of CPU and RAM on the tab *OpenStack*
(or the name you gave your OpenStack connector earlier) of the section *Cloud
Configuration*.

<p align="center"><img src="/img/content/blogs/doc_OpenStack_image_parameters.png" alt="SlipStream Image - edit mode" class="shadow"  width="900" /></p>


## User credentials

Now that the connector is configured and the native images updated, inform
your users that they need to configure their credentials for OpenStack in
their user profile to take advantage of your new connector.

<span class='contact-us-placeholder'></span>