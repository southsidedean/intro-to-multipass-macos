# Getting Started With Multipass on macOS

***Tom Dean - 1/23/23, updated 11/4/26***
## Introduction

Wouldn't it be nice if you could have your own personal mini-cloud running in the confines of your Mac?  Now you can, thanks to Multipass!

**Per Canonical:**

"Multipass is a tool to generate cloud-style virtual machines (VMs) quickly on Linux, macOS and Windows."

**From the Multipass GitHub repository:**

"Multipass is a lightweight VM manager for Linux, Windows and macOS. It's designed for developers who want to spin up a fresh Linux environment with a single command. It uses QEMU on Linux, HCS (Host Compute System) on Windows and QEMU or the Apple Virtualization Framework on macOS to run virtual machines with minimal overhead. Multipass will fetch images for you and keep them up to date.

Since it supports metadata for cloud-init, you can simulate a small cloud deployment on your laptop or workstation."

Multipass makes running Ubuntu virtual machines fast and easy.

***Let's see how we set it up!***

## References

[GitHub: Getting Started With Multipass on macOS](https://github.com/southsidedean/intro-to-multipass-macos)

[Multipass](https://canonical.com/multipass)

[Multipass Documentation](https://canonical.com/multipass/docs/stable/)

[Install Multipass](https://canonical.com/multipass/install)

[How to install Multipass](https://canonical-multipass1.readthedocs-hosted.com/stable/how-to-guides/install-multipass/)

[Multipass Drivers](https://canonical-multipass1.readthedocs-hosted.com/stable/explanation/driver/)

[Using cloud-init with Multipass](https://ubuntu.com/blog/using-cloud-init-with-multipass)

[Multipass Tutorial](https://canonical-multipass1.readthedocs-hosted.com/stable/tutorial/)

[Multipass: GitHub Repository](https://github.com/canonical/multipass)

[Homebrew](https://brew.sh)

## Prerequisites

The Homebrew cask we'll use requires macOS 13 or later.  For backend support and system requirements, see the **Multipass Drivers** link in the **References** section above.

Most of the required software will be installed using ***Homebrew***.  If you don't already have Homebrew installed, you can install it using:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

More information on Homebrew is available via the link in the **References** section above.

## Install Multipass Using Homebrew

Installation is straightforward, using Homebrew:
```bash
brew install --cask multipass
```

*The Homebrew cask is maintained by the community, and isn't officially supported by the Multipass team.  If you'd prefer the official route, download the installer from the [Multipass GitHub repository](https://github.com/canonical/multipass/releases).*

### Uninstall Multipass: OPTIONAL

If you'd like to *uninstall* Multipass, you can use:
```bash
brew uninstall multipass
```

To clean up Multipass data and uninstall:
```bash
brew uninstall --zap multipass
```

### Quick Test: Multipass

Let's validate our Multipass installation by checking the version:
```bash
multipass version
```

We should get a result similar to:
```bash
multipass   1.16.4+mac
multipassd  1.16.4+mac
```

*Remember, versions and architectures can vary based on when you install and what you install it on!*

***Now that Multipass is installed, let's try it out!***

## Working With Multipass

### Deploy Your First Instance

Before we get started, let's take a look at the online help available with Multipass:
```bash
multipass help
```

```bash
Usage: multipass [options] <command>
Create, control and connect to Ubuntu instances.

This is a command line utility for multipass, a
service that manages Ubuntu instances.

Options:
  -h, --help     Displays help on commandline options
  -v, --verbose  Increase logging verbosity. Repeat the 'v' in the short option
                 for more detail. Maximum verbosity is obtained with 4 (or more)
                 v's, i.e. -vvvv.

Available commands:
  alias         Create an alias
  aliases       List available aliases
  authenticate  Authenticate client
  clone         Clone an instance
  delete        Delete instances and snapshots
  exec          Run a command on an instance
  find          Display available images to create instances from
  get           Get a configuration setting
  help          Display help about a command
  info          Display information about instances or snapshots
  launch        Create and start an Ubuntu instance
  list          List all available instances or snapshots
  mount         Mount a local directory in the instance
  networks      List available network interfaces
  prefer        Switch the current alias context
  purge         Purge all deleted instances permanently
  recover       Recover deleted instances
  restart       Restart instances
  restore       Restore an instance from a snapshot
  set           Set a configuration setting
  shell         Open a shell on an instance
  snapshot      Take a snapshot of an instance
  start         Start instances
  stop          Stop running instances
  suspend       Suspend running instances
  transfer      Transfer files between the host and instances
  umount        Unmount a directory from an instance
  unalias       Remove aliases
  version       Show version details
```

Let's try deploying an instance.  We can use the `multipass find` command to retrieve a list of available images:
```bash
multipass find
```

This should give us output similar to the following:
```bash
Image                       Aliases           Version          Description
22.04                       jammy             20261020         Ubuntu 22.04 LTS
24.04                       noble             20261017         Ubuntu 24.04 LTS
26.04                       resolute,lts      20261016         Ubuntu 26.04 LTS
26.10                       stonking          20261025         Ubuntu 26.10

Blueprint (deprecated)      Aliases           Version          Description
anbox-cloud-appliance                         latest           Anbox Cloud Appliance
charm-dev                                     latest           A development and testing environment for charmers
docker                                        0.4              A Docker environment with Portainer and related tools
jellyfin                                      latest           Jellyfin is a Free Software Media System that puts you in control of managing and streaming your media.
minikube                                      latest           minikube is local Kubernetes
ros2-humble                                   0.1              A development and testing environment for ROS 2 Humble.
ros2-jazzy                                    0.1              A development and testing environment for ROS 2 Jazzy.
```

Note that the blueprints in the second list are marked as deprecated, so we'll stick with a regular Ubuntu image.

We'll launch the `lts` image, which, along with `resolute`, is aliased to the `26.04` image.  We can use any of those to launch the image.  The `lts` alias moves to each new LTS release, so you may see a newer version than the one shown here.

Using the `multipass launch` command:
```bash
multipass launch lts
```

Once the instance is up, we'll get a `Launched:` message, along with our random instance name:
```bash
Launched: resilient-dobsonfly
```

We can see a list of instances using the `multipass list` command:
```bash
multipass list
```

We should see something like this:
```bash
Name                    State             IPv4             Image
resilient-dobsonfly     Running           192.168.252.2    Ubuntu 26.04 LTS
```

If we want to get detailed information about our instance, we can use the `multipass info` command:
```bash
multipass info resilient-dobsonfly
```

We should see output similar to this:
```bash
Name:           resilient-dobsonfly
State:          Running
Snapshots:      0
IPv4:           192.168.252.2
Release:        Ubuntu 26.04.1 LTS
Image hash:     8eb876c2ccc4 (Ubuntu 26.04 LTS)
CPU(s):         1
Load:           1.37 0.35 0.12
Disk usage:     2.5GiB out of 4.8GiB
Memory usage:   328.7MiB out of 943.0MiB
Mounts:         --
```

This gives us a quick summary of our `resilient-dobsonfly` instance's vital statistics.

***Deploying an instance was fast and easy!  Now, how do we interact with this instance?***

### Interacting With Our Multipass Instance

Now that we have an instance up and running, let's take a look at a couple of ways to interact with it.

One way to interact with our instance is to open a shell in our instance using the `multipass shell` command:
```bash
multipass shell resilient-dobsonfly
```

Let's take a look at the contents of the `/etc/os-release` file to get some information on our instance:
```bash
ubuntu@resilient-dobsonfly:~$ cat /etc/os-release
PRETTY_NAME="Ubuntu 26.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="26.04"
VERSION="26.04.1 LTS (Resolute Raccoon)"
VERSION_CODENAME=resolute
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=resolute
LOGO=ubuntu-logo
```

So, we can interact with our instance using a shell, just like any other Linux machine.

We might want to execute a command without opening a shell.  We can do this with the `multipass exec` command:
```bash
multipass exec resilient-dobsonfly -- cat /etc/os-release
```

We see the same information as when we executed the command in a shell, without the interactive shell session:
```bash
PRETTY_NAME="Ubuntu 26.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="26.04"
VERSION="26.04.1 LTS (Resolute Raccoon)"
VERSION_CODENAME=resolute
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=resolute
LOGO=ubuntu-logo
```

### Cleaning Up

The beauty of having our own little private cloud is that we can easily get rid of resources that we're finished with.

Let's take a look at our instances:
```bash
multipass list
```
We see our `resilient-dobsonfly` instance:
```bash
Name                    State             IPv4             Image
resilient-dobsonfly     Running           192.168.252.2    Ubuntu 26.04 LTS
```

First, let's stop our instance:
```bash
multipass stop resilient-dobsonfly
```

Now that our `resilient-dobsonfly` instance is stopped, let's take a look:
```bash
multipass list
```

We can see that our instance is stopped:
```bash
Name                    State             IPv4             Image
resilient-dobsonfly     Stopped           --               Ubuntu 26.04 LTS
```

We'll delete our `resilient-dobsonfly` instance:
```bash
multipass delete resilient-dobsonfly
```

Checking our work:
```bash
multipass list
```

We can confirm our `resilient-dobsonfly` instance has been deleted:
```bash
Name                    State             IPv4             Image
resilient-dobsonfly     Deleted           --               Ubuntu 26.04 LTS
```

If you want to completely remove the `resilient-dobsonfly` instance, you can use the `multipass purge` command:
```bash
multipass purge
```

Checking our work:
```bash
multipass list
```

We see no instances:
```bash
No instances found.
```

***That was easy!  Good stuff!***

### Digging Deeper

If you'd like to dig a little deeper into running instances using Multipass, here are some good resources:

[Using cloud-init with Multipass](https://ubuntu.com/blog/using-cloud-init-with-multipass)

[Multipass Tutorial](https://canonical-multipass1.readthedocs-hosted.com/stable/tutorial/)

## Summary

By using Multipass, we can leverage the power of Ubuntu Linux and `cloud-init` to manage our own little private cloud, in the comfort of our Mac.

Enjoy!

*Tom Dean*

## License

This guide is licensed under [CC BY 4.0](LICENSE).
