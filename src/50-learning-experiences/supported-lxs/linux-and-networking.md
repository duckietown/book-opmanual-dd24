(lx-dd-linux-and-networking)=
# LX: Linux and Networking

```{seo}
:description: Learn about Linux and networking in the context of Duckietown, with a focus on concepts and tools relevant to the Duckiedrone.
:keywords: Duckietown, Duckiedrone, DD24-B, LX, learning experience, Linux, networking
```

```{needget}
- Computer setup `dts`: [](dd24-initial-setup)
- (optional) A Duckiedrone: [](flying_your_drone)
- (optional) Review of "[Basics of networking for robotics](https://app-na1.hubspotdocuments.com/documents/8795519/view/717428796?accessId=9a5a79)" slides
---
- An understanding of the basics of Linux and networking, including the command line, file system, and network configuration.
```

This Learning Experience introduces Linux and networking concepts in the context of Duckietown. You will learn about the command line, file system, and network configuration.

```{figure} ../../_images/lxs/linux-and-networking/little-red-packet.jpg
:alt: Linux and Networking
:width: 60%
:name: duckiedrone-lx-linux-and-networking
:align: center

"The hardest part of robotics is networking."
```

```{admonition} Intended Learning Outcomes
:class: tip

After completing this learning experience, learners will be able to:

- Distinguish the Linux kernel, distributions, user space, and central processing unit (CPU) architecture before navigating a Linux filesystem. Create and safely manage files, use wildcards and searches, interpret command results, understand standard streams, redirection, pipes, variables, permissions, processes, and trusted shell scripts, and write and verify small Python and shell programs that read from and write to standard streams.

- Explain how Media Access Control (MAC) addresses, Internet Protocol (IP) addresses, subnets, gateways, Dynamic Host Configuration Protocol (DHCP), Network Address Translation (NAT), firewalls, Domain Name System (DNS), multicast Domain Name System (mDNS), ports, and network quality support Duckiedrone connectivity. Use authorized, targeted ip, getent, ping, nc, ss, curl, and dts fleet discover checks to investigate a connection.

- Distinguish a base-station shell, a physical Duckiedrone Secure Shell (SSH) shell, a virtual Duckiedrone shell, and a service shell. With explicit authorization, access one Duckiedrone and inspect its filesystem, processes, and running services without changing device state.
```

```{admonition} Available repositories
:class: seealso

- [Linux and Networking LX - Learning Experience](https://github.com/duckietown/lx-dd-linux-and-networking)
- [Linux and Networking LX - Recipe](https://github.com/duckietown/x-dd-linux-and-networking-recipe)
```

{{ dt_workspace_matrix_lx_warning.format(dt_workspace_note_prefix) }}

## About these learning activities

```{note}
This learning experience introduces a number of Linux concepts and tools that can be ran on any Ubuntu system. We recommend a native, supported, Ubuntu version installation, or using a Duckietown Workspace. Examples are worked out on [physical](https://get.duckietown.com/products/autonomous-raspberrypi-quadcopter-duckiedrone-dd24) and virtual Duckiedrones. 
```

## Notebooks

Each notebook includes hands-on learning activities and a checkpoint to self-assess understanding. Most of the notebooks can be used stand-alone, without having to have complete all the previous ones. 

| Notebook | Topics |
| --- | --- |
| 1 | Linux kernel, user space, CPU architecture, and distributions |
| 2 | Terminal, shell, current directory, and paths |
| 3 | Variables, quoting, `PATH`, and environment values |
| 4 | Creating, inspecting, copying, renaming, and removing files |
| 5 | Filename conventions, wildcards, `find`, and `grep` |
| 6 | Command help, error recovery, and exit status |
| 7 | Standard input (`stdin`), standard output (`stdout`), standard error (`stderr`), and redirection |
| 8 | Pipes and text processing |
| 9 | Shell scripts, execution, and sourcing |
| 10 | Users, ownership, and permissions |
| 11 | Processes, monitoring, and job control |
| 12 | Shell exercises and output verification |
| 13 | Physical Duckiedrone access through SSH |
| 14 | Virtual Duckiedrone connections |
| 15 | Duckiedrone filesystem inspection |
| 16 | Duckiedrone process inspection |
| 17 | Network addresses, subnets, routing, gateways, and IPv6 |
| 18 | Network protocols, ports, packet layers, and connection quality |
| 19 | Localhost and service binding |
| 20 | DHCP, private addresses, NAT, firewalls, and access policy |
| 21 | DNS, mDNS, URLs, and service discovery |
| 22 | Network diagnostics, local TCP listeners, and Duckiedrone connection testing |

(lx-forking-dd-linux-and-networking)=
## Forking the Repository

The recommended way to use the repository of an LX is to make a fork, and then clone that fork. Forking can be done through the GitHub web interface, and creates a personal copy that can still be synchronized with the upstream Duckietown code.

Cloning the repository directly also works, at the cost of not being able to push personal changes.

1. **Create a fork**: navigate to [the `lx-dd-linux-and-networking` repository](https://github.com/duckietown/lx-dd-linux-and-networking).

    Find and press the "Fork" button on the top right:

    ```{figure} ../../_images/lxs/duckietown-lx-forking.png
    :alt: how to fork a Duckietown LX repository
    :width: 90%
    :name: dd-lx-forking-linux-and-networking
    :align: center

    Fork the LX to be able to make local changes while still being able to receive updates.
    ```

    This creates a new repository at `<your_github_username>/lx-dd-linux-and-networking`.

2. **Clone the fork**: clone the fork on the computer, replacing the GitHub username in the command below, and navigate to the new folder:

        git clone git@github.com:<your_github_username>/lx-dd-linux-and-networking
        cd lx-dd-linux-and-networking

3. **Configure upstream repo**: configure the Duckietown version of this repository as the upstream repository to synchronize with the fork.

    List the current remote repository for the fork,

        git remote -v

    Specify a new remote upstream repository,

        git remote add upstream https://github.com/duckietown/lx-dd-linux-and-networking

    Confirm that the new upstream repository was added to the list,

        git remote -v

    Work can now be pushed to the personal repository using the standard GitHub workflow, and the beginning of every exercise prompts a pull from the upstream repository, updating the exercises to the latest version.

(lx-system-update-dd-linux-and-networking)=
## Keeping the System Up To Date

- 💻 These instructions are for `ente` learning experiences. Ensure that the Duckietown Shell is set to an `ente` profile (and not a `daffy` one). The current profile is shown by:

    ```bash
    dts profile list
    ```

    To switch to an `ente` profile, follow [](dd24-initial-setup).

- 💻 Pull from the upstream remote to synchronize the fork with the upstream repo:

    ```bash
    git pull upstream ente
    ```

- 💻 Make sure the Duckietown Shell is updated to the latest version:

    ```bash
    pipx upgrade duckietown-shell
    ```

- 💻 Update the shell commands:

    ```bash
    dts update
    ```

- 💻 Update the computer and the Duckiedrone: follow [](dd24-software-update).

(lx-ssl-setup-dd-linux-and-networking)=
## SSL Certificate Setup

```{note}
This procedure is only needed the first time an LX is run on a new computer.
```

Duckietown uses SSL certificates and TLS encryption to guarantee the highest standard of safety and privacy. Set up a local SSL certificate needed to run the LX editor inside the browser:

```bash
sudo apt install libnss3-tools
dts setup mkcert
```

```{note}
If a [Duckietown Workspace](https://docs.duckietown.com/ente/duckietown-manual/10-setup/00-computer/setup-duckietown-workspace.html) is in use, install `mkcert` on the host system by following the workspace setup instructions instead of running these commands inside the dev container.
```

(lx-code-editor-dd-linux-and-networking)=
## Launching the Code Editor

```{important}
All `dts code` commands must be executed inside the root directory of the learning experience.
```

From inside the directory of the learning experience, open the code editor by running:

```bash
dts code editor
```

Wait for a URL to appear on the terminal, then click on it or copy-paste it in the address bar of the browser to access the code editor. The first thing shown in the code editor is a version of these instructions. The indications shown in the code editor take precedence over this page.

(lx-navigating-notebooks-dd-linux-and-networking)=
## Walkthrough of Notebooks

Inside the code editor, use the navigator sidebar on the left-hand side to navigate to the `notebooks` directory and open the first notebook.

Follow the instructions on the notebook and work through them in sequence.

(lx-matrix-testing-dd-linux-and-networking)=
### Testing with the Duckiematrix

This learning experience explore fundamentals of bash programming and networking. There is no need to spin up the Duckiematrix to complete this learning experience. 

(lx-create-vdrone-dd-linux-and-networking)=
#### 1. Creating and starting a virtual Duckiedrone

If this has not been done already (e.g., for a different LX), create a virtual Duckiedrone with the command:

```bash
dts duckiebot virtual create --type duckiedrone --configuration DD24 [VDRONE]
```

where `[VDRONE]` is the hostname. It can be any name, subject to the [same naming constraints of physical Duckiedrones](dd24-hostname-constraints).

When the command runs, DTS prompts for a password for the virtual Duckiedrone's `duckie` account, and asks to confirm it. The password must contain at least eight characters and cannot contain colons or line breaks. The typed characters are not displayed.

Then start the virtual robot with the command:

```bash
dts duckiebot virtual start [VDRONE]
```

The robot appears with status `Booting` and finally `Ready` in the output of `dts fleet discover`:

```
         | Hardware |    Type     | Model |  Status  |    Hostname
-------- | -------- | ----------- | ----- | -------- | --------------
[VDRONE] |  virtual | duckiedrone | DD24  |  Ready   | [VDRONE].local
```

```{note}
The Duckiedrone software stack runs on ROS 2. When running ROS 2 commands against a virtual or physical Duckiedrone from the computer, the `ROS_DOMAIN_ID` must match the one used by the robot.
```

(lx-troubleshooting-dd-linux-and-networking)=
## Troubleshooting

For more detailed instructions, refer to [](dd24-lx-general-procedure). For help troubleshooting common problems, refer to [its troubleshooting section](dd24-lx-troubleshooting).
