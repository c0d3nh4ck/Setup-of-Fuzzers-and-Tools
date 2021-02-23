# Angr

> angr is a multi-architectural binary analysis framework based on python for analyzing binaries. It combines both static and dynamic symbolic ("concolic") analysis, making it applicable to a variety of tasks.

## Installation

1. Angr has a lot of dependencies and it's own fork of various python libraries being used with it. So, it is better to install it in a virtual python environment OR just use the **docker** container for angr which is very convenient to install.

    ```bash
    docker pull angr/angr
    docker run --name angr -it angr/angr
    ```

2. You might need to install additional packages via apt. But it has no sudo command as it is uses the vanilla version of ubuntu. So, you cannot escalate privilege to install additional packages. Thus, you can follow the below steps :-

    ```bash
    docker exec -it -u root angr bash
    apt install sudo
    usermod -aG sudo angr   # This will grant root privilege to the user "angr"
    ```

3. And then, to access the container use the command below. The main benefit with docker container is that we don't need to install other tools which are as an extension to angr.

    ```bash
    docker exec -it -u angr angr bash
    ```
    
4. If you just want to run angr for one-time purpose use the command below.

    ```bash
    docker run --rm -it angr/angr
    ```
    
## Parts of Angr-Framework

### The Loader

It is the angr's binary loading component, CLE stands for "CLE Loads Everything" and is responsible for loading the binary file for the angr to work upon.

```python
import angr, monkeyhex    # monkeyhex will format numerical results into hexadecimal
proj = angr.Project('examples/fauxware/fauxware')
```

`proj.loader` represents the entire loaded binary objects. Using this we can find all the symbols, relocations,sections and segments in binary. And, can also be used to rebase address the of binary, set an entry_point to use, or if you want to use a `libc` library.

### The Factory

It provides several constructors for various common objects that we will use frequently.

- `project.factory.block()` - To extract a basic block of code from a given address.
- `project.factory.blank_state()` - This is just one of several state constructors. A state lets us represent a program at any given time.
- `project.factory.simgr()` - It is the primary interface in angr for performing execution, simulation with states.

### Solver Engine

Angr's solver engine is called Claripy. This is used to execute the program with symbolic variables. It performs arithmetic operation using an Abstract Syntax Tree or AST which can then be translated into constraints for an SMT solver, like z3.
