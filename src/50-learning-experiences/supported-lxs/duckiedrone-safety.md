(lx-dd-safety)=
# LX: Duckiedrone Safety

```{seo}
:description: Learn about drone safety and regulations in the context of Duckietown, with a focus on concepts and tools relevant to the Duckiedrone.
:keywords: Duckietown, Duckiedrone, DD24-B, LX, learning experience, safety, FAA, robot safety, OSHA, TRUST, UAS, Brown University
```

```{needget}
- Time to complete the LX: 2-3 hours
---
- An understanding of robot safety regulations, particularly related to drones
- Knowledge of safety tips for soldering at home
- Optional TRUST certification for flying an unmanned aircraft system (UAS)
```

This LX covers the safety analysis every learner must complete before operating a drone. Learners work through the OSHA framework for industrial robot and robot system safety, FAA rules and the TRUST certification for flying an unmanned aircraft system, Brown University's UAS policy, and the precautions needed to fly safely at home. They also set up and document a safe soldering station and a safe indoor flight area.

```{figure} ../../_images/lxs/safety/drone-caution-safety.png
:alt: Duckiedrone safety learning experience
:width: 40%
:name: duckiedrone-lx-safety
:align: center

Safety first. Operating a drone requires understanding the safety considerations and regulations that apply to flying an unmanned aircraft system (UAS).
```

```{admonition} Intended Learning Outcomes
:class: tip

After completing this learning experience, learners will be able to:

- Analyze the safety considerations for operating a robot (drone) using the OSHA framework for industrial robot and robot system safety.
- Explain the FAA rules and regulations that govern operating an unmanned aircraft system (UAS), including how to complete the TRUST certification.
- Identify the institutional (Brown University) and personal (at-home) policies and precautions that apply to flying a drone outside of the lab.
- Set up a soldering station and a flight area that meet the safety requirements for this course.
```

```{admonition} Available repositories
:class: seealso

- [Safety LX - Learning Experience](https://github.com/duckietown/lx-dd-safety)
- [Safety LX - Recipe](https://github.com/duckietown/lx-dd-safety-recipe)
- [Safety LX - Solution](https://github.com/duckietown/lx-dd-safety-solution)

Access to the solution repository is reserved to Duckietown instructors. Reach out to [info@duckietown.com](mailto:info@duckietown.com) or [upgrade your plan](https://hub.duckietown.com/plans/?plan=institutional) through the Duckietown Hub.
```

{{ dt_workspace_matrix_lx_warning.format(dt_workspace_note_prefix) }}

## About these learning activities

```{note}
This learning experience introduces important safety guidelines for operating a drone. It is not a substitute for the FAA TRUST certification, which is required to operate a drone outdoors in the United States. The TRUST certification is free and can be completed online in about 30 minutes.
```

## Notebooks

Each notebook includes hands-on learning activities and a checkpoint to self-assess understanding. Work through the notebooks in order, since each one builds on the previous ones.

| Notebook | Topics |
| --- | --- |
| 1 |  OSHA Safety Analysis, FAA Rules, Brown University Community Guidelines, Soldering Station Safety |
| 2 | Case Study: Midair Collision Over the Hudson River |

(lx-forking-dd-safety)=
## Forking the Repository

The recommended way to use the repository of an LX is to make a fork, and then clone that fork. Forking can be done through the GitHub web interface, and creates a personal copy that can still be synchronized with the upstream Duckietown code.

Cloning the repository directly also works, at the cost of not being able to push personal changes.

1. **Create a fork**: navigate to [the `lx-dd-safety` repository](https://github.com/duckietown/lx-dd-safety).

    Find and press the "Fork" button on the top right:

    ```{figure} ../../_images/lxs/safety/lx-dd-safety-forking.png
    :alt: how to fork a Duckietown LX repository
    :width: 90%
    :name: dd-lx-forking-safety
    :align: center

    Fork the LX to be able to make local changes while still being able to receive updates.
    ```

    This creates a new repository at `<your_github_username>/lx-dd-safety`.

2. **Clone the fork**: clone the fork on the computer, replacing the GitHub username in the command below, and navigate to the new folder:

        git clone git@github.com:<your_github_username>/lx-dd-safety
        cd lx-dd-safety

3. **Configure upstream repo**: configure the Duckietown version of this repository as the upstream repository to synchronize with the fork.

    List the current remote repository for the fork,

        git remote -v

    Specify a new remote upstream repository,

        git remote add upstream https://github.com/duckietown/lx-dd-safety

    Confirm that the new upstream repository was added to the list,

        git remote -v

    Work can now be pushed to the personal repository using the standard GitHub workflow, and the beginning of every exercise prompts a pull from the upstream repository, updating the exercises to the latest version.

(lx-system-update-dd-safety)=
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

(lx-ssl-setup-dd-safety)=
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

(lx-code-editor-dd-safety)=
## Launching the Code Editor

```{important}
All `dts code` commands must be executed inside the root directory of the learning experience.
```

From inside the directory of the learning experience, open the code editor by running:

```bash
dts code editor
```

Wait for a URL to appear on the terminal, then click on it or copy-paste it in the address bar of the browser to access the code editor. The first thing shown in the code editor is a version of these instructions. The indications shown in the code editor take precedence over this page.

(lx-navigating-notebooks-dd-safety)=
## Walkthrough of Notebooks

Inside the code editor, use the navigator sidebar on the left-hand side to navigate to the `notebooks` directory and open the first notebook.

Follow the instructions on the notebook and work through them in sequence.

(lx-troubleshooting-dd-safety)=
## Troubleshooting

For more detailed instructions, refer to [](dd24-lx-general-procedure). For help troubleshooting common problems, refer to [this troubleshooting section](dd24-lx-troubleshooting).
