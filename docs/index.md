# Installation

**Dextool** is a framework for writing plugins using libclang. The main focus
is tools for testing and static analysis.

### Dependencies

To build and run dextool, you will need the following packages:

 * [llvm](http://releases.llvm.org/download.html) >= 4.0
   Tested versions are 4.0, 5.0, 6.0, 7.0, 8.0, 10.0, 11.0, 12.0, 13.0, 14.0.
   Note that these are the versions that are tested or have been tested.
   Dextool usually is able to work with newer versions so if you version isn't in this list then please just try it. [Otherwise leave an issue on github issue tracker](https://github.com/haliliceylan/Dextool-Docs/issues).
 * llvm-xyz-dev >= 4.0
 * libclang-xyz-dev >= 4.0
 * [cmake](https://cmake.org/download) >= 3.5
 * [sqlite3](https://sqlite.org/download.html) >= 3.24.0
 * [D compiler](https://dlang.org/download.html). See the following files for
   the tested versions. They are guaranteed to work.
   * [ldc minimal version](https://github.com/joakim-brannstrom/dextool/blob/master/Docker/partial/ldc_min_version)
   * [ldc max version](https://github.com/joakim-brannstrom/dextool/blob/master/Docker/partial/ldc_latest_version)

Most of them can be installed using your package manager.

???+ note "About D Compiler"
    Only ldc is able to build a release build of dextool. dmd is for debug
    build. Which mean that for a *normal* user it is ldc that you should use.
    **Unlike the original document, this document does not mention anything about dmd.**


### Install Dextool on Ubuntu

#### (Optional) Step 1: Update and install the latest packages:

```
sudo apt-get update
sudo apt-get upgrade
```
<script id="asciicast-531680" src="https://asciinema.org/a/531680.js" async data-i="1" data-cols="80" data-rows="15"></script>

#### (Optional) Step 2: Check which llvm, clang and libclang version to use.
???+ note "Not required for latest version operating systems"

    If you are using the latest version and an updated operating system, you can skip this step, the updated document will ensure that you always install the latest version you can download. If for some reason you cannot access the required versions of the dependencies in the document, this step will be useful for you.

Run the following to check which versions are available:

```
apt search llvm-
apt search clang-
apt search libclang-
```
You should see for example `libclang-14-dev`.



<script id="asciicast-531690" src="https://asciinema.org/a/531690.js" async data-i="1" data-cols="80" data-rows="15"></script>

#### Step 3: Install the dependencies:

```
sudo apt install build-essential cmake llvm-14 llvm-14-dev clang-14 libclang-14-dev libsqlite3-dev
```

<script id="asciicast-531692" src="https://asciinema.org/a/531692.js" async data-i="1" data-cols="80" data-rows="15"></script>
#### Step 4: Install the D compiler:

The supported compiler versions are found at:

* [ldc minimal version](https://github.com/joakim-brannstrom/dextool/blob/master/Docker/partial/ldc_min_version)
* [ldc max version](https://github.com/joakim-brannstrom/dextool/blob/master/Docker/partial/ldc_latest_version)

You can install them via the install script at dlang.org.

In the example below replace the compiler version with one of the supported.
The version that is written below should only be seen as an example.

Example:

```sh
mkdir -p ~/dlang
wget https://dlang.org/install.sh -O ~/dlang/install.sh
sudo chmod +777 ~/dlang/install.sh
~/dlang/install.sh install ldc-1.30.0
```

Add the compilers to your `$PATH` variable:
```sh
source ~/dlang/ldc-1.30.0/activate
```

<script id="asciicast-531705" src="https://asciinema.org/a/531705.js" async data-i="1" data-cols="80" data-rows="15"></script>

#### Step 5: Build and Install

???+ note "About Ram Usage"
    To build dextool comfortably you need ~16Gbyte of RAM. Compiling dextool-mutate.o consume ~9Gbyte.
    You probably ran out of memory when compiling dextool-mutate.o. Run cmake with -DLOW_MEM=ON and single threaded.


```sh
apt-get install -y git # if you have not git installed
source ~/dlang/ldc-1.30.0/activate # enable env again
git clone https://github.com/joakim-brannstrom/dextool.git --single-branch
mkdir dextool/build
cd dextool/build
# if you have less than 16 gb memory (see note)
# cmake -DCMAKE_INSTALL_PREFIX=/usr -DLOW_MEM=ON .. (memory friend but slow)
cmake -DCMAKE_INSTALL_PREFIX=/usr .. 
make install
```


<script id="asciicast-531711" src="https://asciinema.org/a/531711.js" async data-i="1" data-cols="80" data-rows="15"></script>

#### Step 6: Check Dextool Command
```sh
dextool --version
dextool --plugin-list
```

<script id="asciicast-531713" src="https://asciinema.org/a/531713.js" async data-i="1" data-cols="80" data-rows="15"></script>

**Done! Have fun.**