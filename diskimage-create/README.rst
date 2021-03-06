Diskimage-builder script for creation cloud images
==================================================

This script builds Ubuntu, Fedora, CentOS cloud images for use in Sahara. By default the all plugin are targeted, all images will be built. The '-p' option can be used to select plugin (vanilla, spark, hdp or cloudera). The '-i' option can be used to select image type (ubuntu, fedora or centos). The '-v' option can be used to select hadoop version (1, 2 or plain).

NOTE: You should use Ubuntu or Fedora host OS for building images, CentOS as a host OS has not been tested well.

For users:

1. Use your environment (export / setenv) to alter the scripts behavior. Environment variables the script accepts are 'DIB_HADOOP_VERSION_1' and 'DIB_HADOOP_VERSION_2', 'JAVA_DOWNLOAD_URL', 'JAVA_TARGET_LOCATION', 'OOZIE_DOWNLOAD_URL', 'HIVE_VERSION', 'ubuntu_[vanilla|spark|cloudera]_[hadoop_1|hadoop_2]_image_name', 'fedora_vanilla_hadoop_[1|2]_image_name', 'centos_[vanilla|hdp|cloudera]_[hadoop_1|hadoop_2|plain]_image_name'.

2. For creating all images just clone this repository and run script.

.. sourcecode:: bash

  tox -e venv -- sahara-image-create

3. If you want to use your local mirrors, you should specify http urls for Fedora, CentOS and Ubuntu mirrors using parameters 'FEDORA_MIRROR', 'CENTOS_MIRROR' and 'UBUNTU_MIRROR' like this:

.. sourcecode:: bash

  USE_MIRRORS=true FEDORA_MIRROR="url_for_fedora_mirror" CENTOS_MIRROR="url_for_centos_mirror" UBUNTU_MIRROR="url_for_ubuntu_mirror" tox -e venv -- sahara-image-create

NOTE: Do not create all images for all plugins with the same mirrors. Different plugins use different OS version.

4. To select which plugin to target use the '-p' commandline option like this:

.. sourcecode:: bash

  tox -e venv -- sahara-image-create -p [vanilla|spark|hdp|cloudera|storm|mapr]

5. To select which hadoop version to target use the '-v' commandline option like this:

.. sourcecode:: bash

  tox -e venv -- sahara-image-create -v [1|2|plain]

6. To select which operating system to target use the '-i' commandline option like this:

.. sourcecode:: bash

  tox -e venv -- sahara-image-create -i [ubuntu|fedora|centos]

7. If the host system is missing packages required for diskimage-create.sh, the '-u' commandline option will instruct the script to install them without prompt.

NOTE for 4, 5, 6:

For Vanilla you can create ubuntu, fedora and centos cloud image with hadoop 1.x.x and 2.x.x versions. Use environment variables 'DIB_HADOOP_VERSION_1' and 'DIB_HADOOP_VERSION_2' to change defaults.
For Spark you can create only ubuntu image with one hadoop version. You shouldn't specify image type and hadoop version.
For HDP you can create only centos image with hadoop 1.3.0 or 2.0 and without hadoop ('plain' image). You shouldn't specify image type.
For Cloudera you can create ubuntu and centos images with preinstalled cloudera hadoop. You shouldn't specify hadoop version.

NOTE for CentOS images (for vanilla, hdp and cloudera plugins):

Resizing disk space during firstboot on that images fails with errors (https://bugs.launchpad.net/sahara/+bug/1304100). So, you will get an instance that will have a small available disk space. To solve this problem we build images with 10G available disk space as default. If you need in more available disk space you should export parameter DIB_IMAGE_SIZE:

.. sourcecode:: bash

  DIB_IMAGE_SIZE=40 tox -e venv -- sahara-image-create -i centos

For all another images parameter DIB_IMAGE_SIZE will be unset.

`DIB_CLOUD_INIT_DATASOURCES` contains a growing collection of data source modules and most are enabled by default.  This causes cloud-init to query each data source
on first boot.  This can cause delays or even boot problems depending on your environment.
You must define `DIB_CLOUD_INIT_DATASOURCES` as a comma-separated list of valid data sources to limit the data sources that will be queried for metadata on first boot.


For developers:

If you want to add your element to this repository, you should edit this script in your commit (you should export variables for your element and add name of element to variables 'element_sequence').
