# Fuzzbench

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

3. To add a new fuzzer follow this [link](https://google.github.io/fuzzbench/getting-started/adding-a-new-fuzzer)\
We need to create a fuzzer directory and then create a environment variable with FUZZER_NAME storing the name of the directory.\
In the fuzzer directory, we need to create 3 files i.e. builder.Dockerfile , runner.Dockerfile & fuzzer.py

   - **builder.Dockerfile** specifies the parent image and commands for installing system dependencies and fuzzer.
   - **runner.Dockerfile** defines the image that will be used to run benchmarks with your fuzzer.
   - **fuzzer.py** specifies how to build and fuzz benchmarks using the fuzzer and information about the binary that will be fuzzed.

   Now run `make build-$FUZZER_NAME-$BENCHMARK_NAME` where BENCHMARK_NAME is the environment variable containing name of the binary.

4. To run a local experiment follow this [link](https://google.github.io/fuzzbench/running-a-local-experiment)
We need to create an experiment configuration yaml file which contains information about docker registry, output directory, number of trials.
Then create a python script containing information about benchmarks i.e. binary to be fuzzed , fuzzers to compare , config yaml file.
We can optionally add attributes related to seeds, dictionaries, etc.

5. Then, it will generate a html file type report with all the data and statistics related to the fuzzer and the benchmark
in the output directory specified by configuartion yaml file
