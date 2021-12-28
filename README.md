UNIX BUILD NOTES
====================



Berkeley DB
-----------
It is recommended to use Berkeley DB 6.2. If you have to build it yourself:

```bash
BITCOIN_ROOT=$(pwd)

# Pick some path to install BDB to, here we create a directory within the SafeMineCoin directory
BDB_PREFIX="${BITCOIN_ROOT}/build"
mkdir -p $BDB_PREFIX

# Fetch the source and verify that it is not tampered with
wget 'http://download.oracle.com/berkeley-db/db-6.2.32.tar.gz'
echo 'a9c5e2b004a5777aa03510cfe5cd766a4a3b777713406b02809c17c8e0e7a8fb  db-6.2.32.tar.gz' | sha256sum -c
# -> db-6.2.32.tar.gz: OK
tar -xzvf db-6.2.32.tar.gz

# Build the library and install to our prefix
cd db-6.2.32/build_unix/
#  Note: Do a static build so that it can be embedded into the executable, instead of having to find a .so at runtime
../dist/configure --enable-cxx --disable-shared --with-pic --prefix=$BDB_PREFIX
make install

# Configure SafeMineCoin to use our own-built instance of BDB
cd $BITCOIN_ROOT
./autogen.sh
./configure LDFLAGS="-L${BDB_PREFIX}/lib/" CPPFLAGS="-I${BDB_PREFIX}/include/" # (other args...)
```


-----

PATH=$(echo "$PATH" | sed -e 's/:\/mnt.*//g')

cd depends

make -j8 HOST=x86_64-pc-linux-gnu


-----


git clean -dxf
make clean
chmod 777 ./autogen.sh
chmod 777 ./configure
chmod 777 ./share/genbuild.sh
chmod 777 ./src/secp256k1/autogen.sh
chmod 777 ./src/leveldb/build_detect_platform
./autogen.sh
./configure --disable-tests --disable-bench --without-gui
make -j8


-----

NEW DASH COMpiled

sudo -s

apt-get update

apt-get install -y curl build-essential libtool autotools-dev automake pkg-config python3 bsdmainutils cmake

wget https://github.com/codablock/bls-signatures/archive/v20181101.zip

unzip v20181101.zip

cd bls-signatures-20181101

cmake .

make -j30 install

cd ..

git clone https://github.com/dashpay/dash

cd dash/depends

make -j30

cd ..

./autogen.sh

./configure --with-gui=no --disable-tests --disable-bench --prefix=$(pwd)/depends/x86_64-pc-linux-gnu

make -j30

make -j8

-----