## Setup, Installation and Working


### Nautilus-Fuzz

> Nautilus is a coverage guided, grammar based fuzzer. It improves the test coverage by specifying the grammar of semi valid inputs.

1. Install rust using the following command `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
2. Choose **custom option** when installing instead of **default**
3. Select default option everywhere now except when asked for the version of rust => choose **nightly** instead of **stable** version

> All the above steps will install the necessary dependencies related to Rust

4. Now, follow the commands below to install & run Nautilus-Fuzz
```bash
git clone https://github.com/nautilus-fuzz/nautilus.git
cd nautilus
afl-clang-fast test.c -o test  # provided by afl
cargo run --release -- -g grammars/grammar_py_example.py -o /tmp/workdir -- ./test @@
```

5. We can use Nautilus-Fuzz simulatneously with AFL++ to get more code & path coverage as it can be used to instrument the source code or binary files.
```bash
mkdir /tmp/seeds
mkdir /tmp/workdir

#Terminal/Screen 1
afl-fuzz -S afl -i /tmp/seeds -o /tmp/workdir/ ./test @@

#Terminal/Screen 2
cargo run --release -- -o /tmp/workdir -- ./test @@
```
7. Nautilus-Fuzz only works better in cases where it requires the input to be based on a more grammatical based context 
like in PDF parsers, Browsers, and in other programs where they require a more structured input rather than just random characters. 

8. Otherwise, AFL++ dictionary module works far better as it provides better path coverage than Nautilus-Fuzz.




### Qiling

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
First, we need to make a python script which contain information about which program to fuzz
with its architecture & root filesystem path mentioned to emulate the environment using Quiling.
Then, we can specify the address or buffer stream where we want to pass the input and fuzz it.

4. Run this command to start AFL++ in unicorn mode `afl-fuzz -i ./afl_inputs -o ./afl_outputs -m none -U -- python3 ./fuzz_script.py @@`

5. We can also create **snapshot** of a binary file in Quiling at the time it was running. 
And, then use that snapshot later on for fuzzing by changing the buffers at some addresses.

6. When traiging the crash, we can also do the crash analysis and try to exploit it using Quiling 
as it provides **API access** to the register, memory, filesystem, operating system and debugger.
It also allow hooking at various levels like syscalls, assembly instructions, IO, memory-access, etc.

7. It also provides a tool named **qltool** to quickly emulate shellcode & executable binaries.

> In my opinion, it is a very useful program when we want to fuzz firmwares or binary files of different architectures.




### Fuzzbench

> It is a free service that evaluates fuzzers on a wide variety of real-world benchmarks

1. Install Python 3.8 dev build as a python environment using pyenv
```bash
sudo apt-get install -y build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
sudo apt install libedit-dev  # if you can't install libreadline-dev
curl https://pyenv.run | bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
pyenv install 3.8-dev
```

2. Clone the repo of Fuzzbench and create python 3.8-dev environment
```bash
git clone https://github.com/google/fuzzbench
cd fuzzbench
git submodule update --init
pyenv local 3.8-dev
sudo apt install python3.8-venv
source .venv/bin/activate  
```

3. To add a new fuzzer follow this link https://google.github.io/fuzzbench/getting-started/adding-a-new-fuzzer 
We need to create a fuzzer directory and then create a environment variable with FUZZER_NAME storing the name of the directory.
In the fuzzer directory, we need to create 3 files i.e. builder.Dockerfile , runner.Dockerfile & fuzzer.py
- **builder.Dockerfile** specifies the parent image and commands for installing system dependencies and fuzzer.
- **runner.Dockerfile** defines the image that will be used to run benchmarks with your fuzzer.
- **fuzzer.py** specifies how to build and fuzz benchmarks using the fuzzer and information about the binary that will be fuzzed.

Now run `make build-$FUZZER_NAME-$BENCHMARK_NAME` where BENCHMARK_NAME is the environment variable containing name of the binary.

4. To run a local experiment follow this link https://google.github.io/fuzzbench/running-a-local-experiment
We need to create an experiment configuration yaml file which contains information about docker registry, output directory, number of trials.
Then create a python script containing information about benchmarks i.e. binary to be fuzzed , fuzzers to compare , config yaml file.
We can optionally add attributes related to seeds, dictionaries, etc.

5. Then, it will generate a html file type report with all the data and statistics related to the fuzzer and the benchmark
in the output directory specified by configuartion yaml file.
