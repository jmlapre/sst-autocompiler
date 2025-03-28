# Installing SST-autocompiler

## Docker Image
```bash
# Use this Docker image, it has the clang-18 tools already configured for you
docker pull jmlapre/sst-clang18:latest
docker run -it jmlapre/sst-clang18:latest
```
Then you can use that terminal or connect using VSCode [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) to continue the instructions below.

```bash
# Install sst-core
git clone https://github.com/sstsimulator/sst-core.git
cd sst-core
./autogen.sh
mkdir build && cd build

../configure CXX=clang++ CC=clang \
--prefix=$HOME/install \
--disable-mpi

make V=1 && make install
export PATH=$HOME/install/bin:$PATH
cd

# Install sst-elements
git clone https://github.com/sstsimulator/sst-elements.git
cd sst-elements
gsw devel
./autogen.sh
mkdir build && cd build

../configure CXX=clang++ CC=clang \
--prefix=$HOME/install \
--with-sst-core=$HOME/install

make V=1 && make install
cd

# Install sst-autocompiler
git clone https://github.com/nab880/sst-autocompiler.git
cd sst-autocompiler
# As of 2025-02-26 we only have the main branch
./autogen.sh
mkdir build && cd build

../configure CXX=clang++ CC=clang \
--with-std=17 \
--prefix=$HOME/install \
--with-sst-core=$HOME/install \
--with-sst-elements=$HOME/install \
--with-clang=/usr/lib/llvm-18

make V=1 && make install

# Test that sst-autocompiler works
cd ../tests
make
```

The above command should generate (roughly) the following output:
```bash
SST_LIB_PATH=/home/vscode/install/lib/sst-elements-library:. \
sst test_tls.py 
my_global: 1
my_global: 1
Simulation is complete, simulated time: 1.76579 us
```
