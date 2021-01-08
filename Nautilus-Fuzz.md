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