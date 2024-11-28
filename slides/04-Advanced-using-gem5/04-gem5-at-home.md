---
marp: true
paginate: true
theme: gem5
title: gem5 at home
author: William Shaddix
---

<!-- _class: title -->

## gem5 at home (or work/school)

---

## Getting help

gem5 has lots of resources to get help:

1. Documentation at [gem5 doxygen](http://doxygen.gem5.org/)
2. Ways to reach out for help:
   - [Github discussions](https://github.com/orgs/gem5/discussions) **This is the main place for questions**
   - [gem5 Slack channel](https://join.slack.com/t/gem5-workspace/shared_invite/zt-2e2nfln38-xsIkN1aRmofRlAHOIkZaEA)
   - Join our mailing lists:
      - [gem5-dev@gem5.org : For discussions regarding gem5 development](https://harmonylists.io/list/gem5-dev.gem5.org)
      - [gem5-users@gem5.org : For general discussions about gem5 and its use](https://harmonylists.io/list/gem5-users.gem5.org)
      - [gem5-announce@gem5.org : For general gem5 announcements](https://harmonylists.io/list/gem5-announce.gem5.org)
3. [Youtube videos](https://www.youtube.com/@gem5)

These links and more information are also available at [https://www.gem5.org/ask-a-question/](https://www.gem5.org/ask-a-question/)

> We do our best to get to questions, but they often go unanswered. This isn't because it's not a good question, but because we don't have enough volunteers.

---

## Running gem5 at home

- gem5 performance qualities
  - Single threaded
  - Consumes lots of RAM (if you want to model 32 GB of memory, it needs 32 GB of memory to model it)
  - Can take a lot of time
- Because of this its best to run multiple experiments in parallel
- Recommended hardware:
  - High single thread performance
  - Doesn't need many cores
  - LOTS OF RAM

---

## System software requirements

- Ubuntu 22.04+ (at least GCC 10)
  - 20.04 works, but there are bugs in GCC 8 (or 9, whatever the default is) and you have to upgrade the GCC version.
- Python 3.6+
- SCons
- Many optional requirements.

This *should* work on most Linux systems and on MacOS.

See our Dockerfiles for the most up-to-date version information:

[`gem5/util/dockerfiles/`](https://github.com/gem5/gem5/tree/stable/util/dockerfiles)

---

## Using dockerfiles

If you have trouble, we have docker images.

Here's a generic docker command that should work.

```sh
docker run --rm -v $(pwd):$(pwd) -w $(pwd) ghcr.io/gem5/ubuntu-24.04_all-dependencies:v24-0 <your command>
```

- Runs the image at `https://ghcr.io/gem5/ubuntu-24.04_all-dependencies:v24-0`.
- Automatically removes the docker image (`--rm`)
- Sets it up so that the current directory (`-v $(pwd):$(pwd)`) is available inside the docker container
- Sets the working directory to the current directory (`-w $(pwd)`)
- Runs a command.
- Every command will now need to run with this to make sure the libraries are set up correctly.

> I cannot **strongly enough** emphasize that you should not run interactively in the docker container. Use it to just run one command at a time.

---

## The devcontainer

The devcontainer we've been using is based off of `ghcr.io/gem5/ubuntu-24.04_all-dependencies:v24-0`, but also includes some gem5 binaries.

You can find it at `ghcr.io/gem5/devcontainer:bootcamp-2024`.

The source is at [`gem5/utils/dockerfiles/devcontainer`](https://github.com/gem5/gem5/blob/stable/util/dockerfiles/devcontainer/Dockerfile).

---

## Recommended practices

- Unless planning on contributing to gem5 or you need to use recently developed work, use the `stable` branch.
- Create branches off of stable.
- Don't modify parameters of python files in `src/`. Instead create *extensions* of stdlib types or SimObjects.
- Don't be afraid to read the code. The code is the best documentation.

---

<!-- _class: two-col code-60-percent tight -->

## gem5 Cheat Sheet

#### Downloading

```sh
git clone https://github.com/gem5/gem5
```

#### Building

```sh
scons build/ALL/gem5.opt -j$(nproc)
```

#### Customizing the build

```sh
scons menuconfig build/ALL
```

#### Running

```sh
build/ALL/gem5.opt [gem5 options] <your script> [your script options]
```

#### Example scripts

See `configs/examples/gem5_library`

#### Resources/Workloads/Disk Images/Suites

<https://resources.gem5.org/>
`obtain_resource(<resource id>)`

#### Debugging

```sh
build/ALL/gem5.opt --debug-flags=<debug flags> <your script>
build/ALL/gem5.opt --debug-help
```

#### Stats

Found in `m5out/stats.txt`

```python
simulator.get_simstats()
```

#### Full system

```sh
util/term/m5term localhost 3456
```

---

<!-- _class: two-col code-60-percent tight -->

## More cheat sheet

#### Creating a disk image

See <https://github.com/gem5/gem5-resources/>. Use the packer files already available or create your own.

```sh
packer build <packer json file>
```

#### Take a checkpoint

```python
sim.save_checkpoint('checkpoint')
```

#### Restore a checkpoint

```python
sim = Simulator(checkpoint_path='checkpoint')
```

#### Controlling simulation

```python
def my_exit_handler():
    yield False #continue simulation
    yield True #stop simulation
sim  = Simulator(on_exit_event={
  ExitEvent.WORKBEGIN: my_exit_handler})
```

#### Stdlib components

- `Board`: Connects things together
- `Processor`: Multiple cores
- `CacheHierarchy`: The caches. Either classic or Ruby
- `MemorySystem`: The main memory

---

<!-- _class: start -->

## Final things

---

## Big thanks

![Everyone who has contributed to the bootcamp width:1200px](../01-Introduction/00-introduction-to-bootcamp-imgs/devs.drawio.svg)

---

## Big thanks to you all!

Please let us know how we did:

<https://forms.gle/M6HZHxGjXpcdw4kZ8>

![QR code for the form](./04-gem5-at-home-imgs/qr-code.png)

Reach out to us:
Jason Lowe-Power <jlowepower@ucdavis.edu>
Tamara Silbergleit Lehman <tamara.lehman@colorado.edu>