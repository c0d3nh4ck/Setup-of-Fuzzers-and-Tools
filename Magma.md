# Magma

> A ground-truth binary fuzzing benchmark suite based on real programs with real bugs

1. Install Docker Engine by following steps from this [link](https://docs.docker.com/engine/install/)

2. Then, we need to install some other dependencies and clone the repository

    ```bash
    sudo apt install -y util-linux inotify-tools git
    git clone --branch v1.0.4 https://github.com/HexHive/magma.git
    ```

3. Now, you just need to configure the parameters of **captainrc** file located in the *magma/tools/captain* directory for selecting different fuzzers and their targets with the time, no. of cores of CPU and no. of cycles alloted to them. Then, execute `./run.sh`

4. For fuzzer based on AFL, you need to edit *magma/tools/benchd/fuzzers/afl.py* file and uncomment the part below.

    ```python
    os.system("sudo bash -c 'echo core >/proc/sys/kernel/core_pattern'")
    os.system("sudo bash -c 'cd /sys/devices/system/cpu; echo performance | tee cpu*/cpufreq/scaling_governor'")
    ```

5. After executing the `run.sh` script, it will generate the output and other files in `workdir` directory in the same directory.
For information related to what each files in it meant and to configure the `run.sh`, follow this [link](https://hexhive.epfl.ch/magma/docs/config.html)

> This fuzzing benchmark is still in its infancy and requires a lot of work to be done like documentation on how to add other fuzzers as well, toolset required to visualize the results. But, still it is well documented and easy to setup as each fuzzer with its target runs in a separate docker container
