# Apply Mutation Testing

Dextool mutate requires a `compile_commands.json` for its analyzer. Such a file
contains how each file that the project consist of is compiled. These flags are
extracted by dextool from `compile_commands.json` in order to analyze the
source code for mutants. Some build systems can produce such a json file
natively (cmake) while others can be inspected with
[BEAR](https://github.com/rizsotto/Bear).

You can follow the instruction below 

## GoogleTest Example

[Google Test project](https://github.com/google/googletest) is used as an example.

### Obtain the project you want to analyze
```sh
git clone https://github.com/google/googletest.git
cd googletest
```
<script id="asciicast-536112" src="https://asciinema.org/a/536112.js" async data-i="1" data-cols="80" data-rows="15"></script>

### Generate a JSON compilation database for the project
```sh
mkdir build
pushd build
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -Dgtest_build_tests=ON -Dgmock_build_tests=ON ..
make
popd
```
<script id="asciicast-536115" src="https://asciinema.org/a/536115.js" async data-i="1" data-cols="80" data-rows="15"></script>

### Create a configuration file
```sh
dextool mutate admin --init
```
<script id="asciicast-536116" src="https://asciinema.org/a/536116.js" async data-i="1" data-cols="80" data-rows="15"></script>

The file should be pretty self explaining.
Open the config file and change the following fields:
```toml
[workarea]
include = ["googlemock/include/*", "googlemock/src/*", "googletest/include/*", "googletest/src/*"]

[compiler]
extra_flags = [ "-D_POSIX_PATH_MAX=1024" ]

[compile_commands]
search_paths = ["./build/compile_commands.json"]

[mutant_test]
test_cmd = "./test.sh"
#test_cmd_dir = ["./build/test"]
build_cmd = "./build.sh"
analyze_using_builtin = ["gtest"]
```
<script id="asciicast-536117" src="https://asciinema.org/a/536117.js" async data-i="1" data-cols="80" data-rows="15"></script>

### Generate a database of containing the mutants
```sh
dextool mutate analyze
```
<script id="asciicast-536120" src="https://asciinema.org/a/536120.js" async data-i="1" data-cols="80" data-rows="15"></script>

Create a file `build.sh` that will build the subject under test when invoked:
```sh
#!/bin/bash
set -e
cd build
make -j$(nproc)
```
<script id="asciicast-536121" src="https://asciinema.org/a/536121.js" async data-i="1" data-cols="80" data-rows="15"></script>

Create a file `test.sh` that will run the entire test suite when invoked:
```sh
#!/bin/bash
set -e
cd build
ctest --output-on-failure
```
<script id="asciicast-536123" src="https://asciinema.org/a/536123.js" async data-i="1" data-cols="80" data-rows="15"></script>

Make the files executable so they can be used by dextool:
```sh
chmod 755 build.sh test.sh
```
<script id="asciicast-536124" src="https://asciinema.org/a/536124.js" async data-i="1" data-cols="80" data-rows="15"></script>

### Run the mutation testing on the LCR mutants
```sh
dextool mutate test
```

