# Qiling

> Qiling is an advanced binary emulation framework which is used as sandbox to different kind of architectures and Operating Systems.

1. Follow the commands below to install Qiling

    ```bash
    git clone https://github.com/qilingframework/qiling
    cd qiling
    sudo pip3 install .
    ```

2. To setup Qiling for fuzzing

    ```bash
    cd ~/AFLplusplus/unicorn_mode    # if AFL++ repository is located at home directory
    ./build_unicorn_support.sh

    # Install Binwalk to extract filesystem from firmware
    git clone https://github.com/ReFirmLabs/binwalk.git
    cd binwalk
    sudo python3 setup.py install
    sudo ./deps.sh
    ```

3. We can check the fuzzing directory in the Qiling repository as an example on how to use it.
First, we need to make a python script which contain information about which program to fuzz with its architecture & root filesystem path mentioned to emulate the environment using Quiling.
Then, we can specify the address or buffer stream where we want to pass the input and fuzz it.

4. Run this command to start AFL++ in unicorn mode\
   `afl-fuzz -i ./afl_inputs -o ./afl_outputs -m none -U -- python3 ./fuzz_script.py @@`

5. We can also create **snapshot** of a binary file in Quiling at the time it was running.
And, then use that snapshot later on for fuzzing by changing the buffers at some addresses.

6. When traiging the crash, we can also do the crash analysis and try to exploit it using Quiling.\
As it provides **API access** to the register, memory, filesystem, operating system and debugger.\
It also allow hooking at various levels like syscalls, assembly instructions, IO, memory-access, etc.

7. It also provides a tool named **qltool** to quickly emulate shellcode & executable binaries.

> In my opinion, it is a very useful program when we want to fuzz firmwares or binary files of different architectures.
