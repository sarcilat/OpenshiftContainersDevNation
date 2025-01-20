# Setup

## Requiered Tools

The following tools are required for running the exercises in this tutorial. You’ll need to have them installed and configured before you get started with any of the tutorial chapters.

> **Tip:** By default, the commands in this tutorial are using Docker, but you use podman instead of docker throughout the tutorial’s instructions, or alias docker=podman. The advantage of Podman is that it is 100% Free Open Source and does not need to run with elevated privileges.

| Tool | macOS | Fedora | Windows |
| --- | --- | --- | --- |
| Podman Desktop | [Podman Desktop for Mac](https://podman-desktop.io/downloads) | [Podman Desktop for Linux](https://podman-desktop.io/downloads) | [Podman Desktop for Windows](https://podman-desktop.io/downloads) |
| Java 21 (or 17) | [OpenJDK Eclipse Temurin](https://adoptium.net/installation/), `brew install openjdk@21` | [OpenJDK Eclipse Temurin](https://adoptium.net/installation/) | [OpenJDK Eclipse Temurin](https://adoptium.net/installation/) (Make sure you set the `JAVA_HOME` environment variable and add `%JAVA_HOME%\bin` to your `PATH`) |
| Apache Maven 3.8.5+ | `brew install maven` | `dnf install maven` | [Windows](https://maven.apache.org/download.cgi) (Make sure you set the `MAVEN_HOME` environment variable and add `%MAVEN_HOME%\bin` to your `PATH`)

---
## Instalación del Open JDK on RHEL

On RHEL or RHEL based systems you just need to run the following command:

```bash
sudo yum install java-21-openjdk
```

You will see a output like the one shown bellow.

```text
Updating Subscription Management repositories.
Red Hat CodeReady Linux Builder for RHEL 9 x86_64 (RPMs)                                                                                                                                                                                                                                      104 kB/s | 4.5 kB     00:00    
Red Hat Enterprise Linux 9 for x86_64 - AppStream (RPMs)                                                                                                                                                                                                                                      107 kB/s | 4.5 kB     00:00    
Red Hat Ansible Automation Platform 2.4 for RHEL 9 x86_64 (RPMs)                                                                                                                                                                                                                              114 kB/s | 4.0 kB     00:00    
Red Hat Enterprise Linux 9 for x86_64 - Supplementary (RPMs)                                                                                                                                                                                                                                  100 kB/s | 3.7 kB     00:00    
Red Hat Enterprise Linux 9 for x86_64 - BaseOS (RPMs)                                                                                                                                                                                                                                          85 kB/s | 4.1 kB     00:00    
Dependencies resolved.
```

You can now verify that all changes were applied correctly by running the following command:

```bash
java --version
```

If the version displayed is like the one bellow, well done, you have your Open JDK configured correctly and you can move on the next requirement.

```text
openjdk 21.0.4 2024-07-16 LTS
OpenJDK Runtime Environment (Red_Hat-21.0.4.0.7-1) (build 21.0.4+7-LTS)
OpenJDK 64-Bit Server VM (Red_Hat-21.0.4.0.7-1) (build 21.0.4+7-LTS, mixed mode, sharing)
```

Otherwise, if you have a different output, similar to the one shown below, you will need to complete some additional steps.

```text
openjdk 11.0.24 2024-07-16 LTS
OpenJDK Runtime Environment (Red_Hat-11.0.24.0.8-2) (build 11.0.24+8-LTS)
OpenJDK 64-Bit Server VM (Red_Hat-11.0.24.0.8-2) (build 11.0.24+8-LTS, mixed mode, sharing)
```

First you must find the path of the jdk you have already installed, to do this you must run the following command:

```bash
sudo update-alternatives --config java
```

In case you have more than one JDK, you will see output like the following. Make sure to select the openJDK 21, and copy the path. In this particular escenario is: `/usr/lib/jvm/java-21-openjdk-21.0.4.0.7-1.el9.x86_64` (ignore the last folders `/bin/java`).

```text
There are 2 programs which provide 'java'.

  Selection    Command
-----------------------------------------------
*  1           java-11-openjdk.x86_64 (/usr/lib/jvm/java-11-openjdk-11.0.24.0.8-2.el9.x86_64/bin/java)
 + 2           java-21-openjdk.x86_64 (/usr/lib/jvm/java-21-openjdk-21.0.4.0.7-1.el9.x86_64/bin/java)
```

Now you need to set the `JAVA_HOME`, as follows (use tha path that you copied on the previous step).

```bash
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-21.0.4.0.7-1.el9.x86_64
```

Add it to the PATH as follows:

```bash
export PATH=$JAVA_HOME/bin:$PATH
```

You can verify that you have everithing configured correctly by using the following command:

```bash
java --version
```

If everything is OK, you will see a result similar to the one below. Otherwise, it is recommended to go back to the last steps and make sure everything is set up correctly.

```text
openjdk 21.0.4 2024-07-16 LTS
OpenJDK Runtime Environment (Red_Hat-21.0.4.0.7-1) (build 21.0.4+7-LTS)
OpenJDK 64-Bit Server VM (Red_Hat-21.0.4.0.7-1) (build 21.0.4+7-LTS, mixed mode, sharing)
```

If you want to know more about this process, read this [link](https://docs.redhat.com/en/documentation/red_hat_build_of_openjdk/21/html-single/installing_and_using_red_hat_build_of_openjdk_21_on_rhel/index#installing-jre-on-rhel-using-archive_openjdk).

---
## Instaling maven on RHEL

Over RHEL systems you can not install maven simply using the command `dnf install maven`, or `yum install maven`, because Quarkus requiere at least maven on version 3.8.5, and at the moment to write this guide, on Red Hat repositories only is available maven on version 3.6.x. Because of that it is necessary to implement a particular installation process.
First you need to download the binary of maven on the last version, you can verify the current version of maven [here](https://maven.apache.org/download.cgi).

```bash
wget https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
```

```text
--2025-01-03 23:40:48--  https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
Resolving dlcdn.apache.org (dlcdn.apache.org)... 151.101.2.132, 2a04:4e42::644
Connecting to dlcdn.apache.org (dlcdn.apache.org)|151.101.2.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 9102945 (8.7M) [application/x-gzip]
Saving to: ‘apache-maven-3.9.9-bin.tar.gz’

apache-maven-3.9.9-bin.tar.gz                       100%[================================================================================================================>]   8.68M  --.-KB/s    in 0.05s   

2025-01-03 23:40:48 (187 MB/s) - ‘apache-maven-3.9.9-bin.tar.gz’ saved [9102945/9102945]
```

Now you need to decompress the file like this:

```bash
tar -xvzf apache-maven-3.9.9-bin.tar.gz
```

```text
apache-maven-3.9.9/README.txt
apache-maven-3.9.9/LICENSE
apache-maven-3.9.9/NOTICE
apache-maven-3.9.9/lib/
apache-maven-3.9.9/lib/aopalliance.license
apache-maven-3.9.9/lib/commons-cli.license
...
```

Now is time to move the decompressed maven folder, to the path `/opt/maven`.

```bash
mv apache-maven-3.9.9 /opt/maven
```

Create the file `maven.sh` in the path `/etc/profile.d`, excecuting the following command:

```bash
sudo vi /etc/profile.d/maven.sh
```

Add the content showed bellow, to insert text on the file first ou need to press the key `i`, then you can paste the text, and finally you can save and close the file pressing the key `esc`, and then typing `:x`, and the hit the key `enter`.

```text
export M2_HOME=/opt/maven
export PATH=${M2_HOME}/bin:${PATH}
```

To verify that the content was successfully saved, you can use the following command:

```bash
cat /etc/profile.d/maven.sh 
export M2_HOME=/opt/maven
export PATH=${M2_HOME}/bin:${PATH}
```

Now activate the environment variable with the following command:

```bash
source /etc/profile.d/maven.sh
```

Finally verify the installation process by running the following command:

```bash
mvn -version
```

You will see an output like this:

```text
Apache Maven 3.9.9 (8e8579a9e76f7d015ee5ec7bfcdc97d260186937)
Maven home: /opt/maven
Java version: 21.0.5, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-21-openjdk-21.0.5.0.11-2.el9.x86_64
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.14.0-427.18.1.el9_4.x86_64", arch: "amd64", family: "unix"
```

If you want to know more about this process, you can read the article that you will find on the following [link](https://www.atlantic.net/dedicated-server-hosting/how-to-install-apache-maven-on-fedora/).
