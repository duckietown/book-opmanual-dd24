(lx-dd-docker)=
# LX: Introduction to Containerization

```{seo}
:description: Learn about containerization technology basics and explore Docker through local container, image, storage, and networking exercises, then connect these concepts to Duckiedrone services and Duckietown development tools.
:keywords: Duckietown, Duckiedrone, DD24-B, Docker, containers, LX, learning experience, Containerization, DockerHub, Dockerfile, Docker Compose, Docker contexts, bind mounts, named volumes, networking, isolation, development containers, Duckietown Shell, Workspace
```

```{needget}
- A computer with Docker and the Duckietown Shell (`dts`) set up: [](dd24-initial-setup)
- Familiarity with shell commands, paths, permissions, and basic networking: [](lx-dd-linux-and-networking)
- Internet access to download images and dependencies
- (optional) A physical or virtual Duckiedrone for the deployment-inspection activities
---
- Practical experience with Docker images, containers, storage, networks, and connections in Duckietown
```

This Learning Experience introduces containerization technology with Docker through small experiments on your base station. You will inspect containers, build and test a web-server image, preserve data, and connect applications over a Docker network. Later activities relate these concepts to Duckiedrone services, Docker contexts, the Duckietown Shell, and the Duckietown Workspace.

```{figure} ../../_images/lxs/docker/docker-lx-hero.jpg
:alt: Introduction to Containerization with Docker in Duckietown
:width: 90%
:name: duckiedrone-lx-introduction-to-containerization-docker
:align: center

Containers include applications and their dependencies, making software easy to ship and handle. 
```

```{admonition} Intended Learning Outcomes
:class: tip

After completing this learning experience, learners will be able to:

- Explain why reproducible software environments are useful in robotics, and distinguish Docker images, layers, containers, virtual machines, clients, daemons, and registries.

- Interpret image tags, digests, and platforms; identify the daemon receiving a command; and distinguish local practice resources from Duckiedrone services.

- Run, inspect, enter, stop, restart, and remove containers using logs, process information, and resource measurements.

- Build an image from a Dockerfile, inspect its configuration, and verify an application's response through a published port.

- Use named volumes and bind mounts, test communication between containers, and clean up exercise resources individually.

- Explain how permissions, mounts, and network settings affect container isolation, and relate Docker contexts and runtime configuration to Duckietown Shell and Workspace workflows.
```

```{admonition} Available repositories
:class: seealso

- [Docker LX - Learning Experience](https://github.com/duckietown/lx-dd-docker)
- [Docker LX - Recipe](https://github.com/duckietown/lx-dd-docker-recipe)
```

{{ dt_workspace_matrix_lx_warning.format(dt_workspace_note_prefix) }}

## About these learning activities

```{note}
The local container, image, storage, and network exercises require a prepared base-station environment with Docker, `dts`, and `curl`. Use a supported native Ubuntu installation or a Duckietown Workspace, following [](dd24-initial-setup).

A physical Duckiedrone is optional. Inspecting live platform configuration requires a physical or virtual deployment; the remote Docker-context comparison requires a physical Duckiedrone with configured SSH authentication and Docker access. Project build and workbench examples have their own project prerequisites, stated in the notebooks.
```

## Notebooks

Work through the twelve notebooks in order. Each includes activities and an interactive checkpoint. Run shell commands in the terminal specified by the notebook, and write or select a response before revealing each checkpoint answer.

| Notebook | Topics |
| --- | --- |
| 1 | Introduction to Docker: containers, images, layers, registries, clients, and daemons |
| 2 | Preparing the local practice environment and verifying the Docker connection |
| 3 | Image references, processor platforms, and image and container inventories |
| 4 | Container access, isolation, and Duckiedrone boundaries |
| 5 | Running, inspecting, entering, stopping, and removing containers |
| 6 | Building and testing a local web-server image |
| 7 | Named volumes, read-only bind mounts, persistence, and cleanup |
| 8 | Duckiedrone Docker hosts and Compose stacks |
| 9 | Communication between containers and real Duckiedrone runtime configuration |
| 10 | Docker contexts and local and remote daemon selection |
| 11 | Docker operations behind Duckietown Shell commands |
| 12 | Development containers and the Duckietown Workspace |

(lx-forking-dd-docker)=
## Forking the Repository

Fork the LX repository to keep your own changes while retaining access to upstream updates. You can also clone the Duckietown repository directly if you do not need to push personal changes.

1. **Create a fork**: navigate to [the `lx-dd-docker` repository](https://github.com/duckietown/lx-dd-docker) and press the "Fork" button on the top right:

    ```{figure} ../../_images/lxs/duckietown-lx-forking.png
    :alt: how to fork a Duckietown LX repository
    :width: 90%
    :name: dd-lx-forking-docker
    :align: center

    Fork the LX to keep your own changes and receive upstream updates.
    ```

    This creates a repository at `<your_github_username>/lx-dd-docker`.

2. **Clone the fork**: replace `<your_github_username>` in the command below, then enter the repository directory:

    ```bash
    git clone git@github.com:<your_github_username>/lx-dd-docker
    cd lx-dd-docker
    ```

3. **Configure the upstream repository**: add the Duckietown repository as a source of updates, then check the configured remotes:

    ```bash
    git remote add upstream https://github.com/duckietown/lx-dd-docker
    git remote -v
    ```

    You can push your changes to your fork and obtain updates from the upstream repository.

(lx-system-update-dd-docker)=
## Keeping the System Up To Date

- These instructions are for the `ente` distribution. Check your Duckietown Shell profile:

    ```bash
    dts profile list
    ```

    If necessary, follow [](dd24-initial-setup) to select an `ente` profile.

- From the LX repository root, synchronize with the upstream `ente` branch:

    ```bash
    git pull upstream ente
    ```

    Commit or back up local edits you want to keep before updating.

- Update the Duckietown Shell and its commands:

    ```bash
    pipx upgrade duckietown-shell
    dts update
    ```

- Follow [](dd24-environment-setup) to prepare the computer and, if you plan to use one, the Duckiedrone.

(lx-ssl-setup-dd-docker)=
## SSL Certificate Setup

```{note}
This procedure is needed only when the LX editor certificates have not already been set up on the computer.
```

For a native Ubuntu setup, install the certificate tools and configure the local certificate used by the browser-based LX editor:

```bash
sudo apt install libnss3-tools
dts setup mkcert
```

```{note}
If you use a [Duckietown Workspace](https://docs.duckietown.com/ente/duckietown-manual/10-setup/00-computer/setup-duckietown-workspace.html), install `mkcert` on the host system following the Workspace setup instructions.
```

(lx-code-editor-dd-docker)=
## Launching the Code Editor

```{important}
Run all `dts code` commands from the LX repository root.
```

From the `lx-dd-docker` directory, launch the editor:

```bash
dts code editor
```

Wait for a URL to appear in the terminal, then open it in your browser. The repository's README and the instructions shown in the editor provide the current LX-specific guidance.

(lx-navigating-notebooks-dd-docker)=
## Walkthrough of Notebooks

In the editor's navigator sidebar, open the `notebooks` directory and start with Notebook 1. Continue through the notebooks in order.

Run the Docker practice commands in a separate base-station terminal in your prepared environment. Notebook 2 guides you through verifying its local Docker connection. Use the same practice daemon throughout the local activities, and run the build and bind-mount exercises from the LX repository root as directed.

Use the prepared LX editor environment for the interactive checkpoints. Follow each notebook's cleanup instructions before moving on.

(lx-matrix-testing-dd-docker)=
### Testing with the Duckiematrix

The Docker LX does not require a running Duckiematrix session. The local Docker experiments and source-reading activities can be completed without a robot. A virtual Duckiedrone is optional for the live configuration inspection in Notebook 9 and the virtual-connection example in Notebook 11.

(lx-create-vdrone-dd-docker)=
#### 1. Creating and starting a virtual Duckiedrone

If you want to complete those optional activities and do not already have a virtual Duckiedrone, create one:

```bash
dts duckiebot virtual create --type duckiedrone --configuration DD24 VDRONE
```

Replace `VDRONE` with your chosen virtual Duckiedrone name, following the [hostname constraints](dd24-hostname-constraints).

The Duckietown Shell prompts for a password for the virtual Duckiedrone's `duckie` account and asks you to confirm it. The password must contain at least eight characters and cannot contain colons or line breaks. Typed characters are not displayed.

Start the virtual Duckiedrone:

```bash
dts duckiebot virtual start VDRONE
```

Check its status:

```bash
dts fleet discover
```

Wait until it reports `Ready`, then follow the relevant notebook's connection instructions. After the activity, stop it:

```bash
dts duckiebot virtual stop VDRONE
```

(lx-troubleshooting-dd-docker)=
## Troubleshooting

Refer to [](dd24-lx-general-procedure) for the shared LX workflow and [](dd24-lx-troubleshooting) for common editor and setup problems. For Docker activity failures, use the checks and troubleshooting steps in the relevant notebook. The [Docker LX README](https://github.com/duckietown/lx-dd-docker#readme) lists the environment and activity prerequisites.
