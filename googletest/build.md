How to setup googleTest as a shared library on Linux
====================================================

Before you start make sure your have read and understood this note from Google! This tutorial makes using gtest easy, but may introduce nasty bugs.

1. Get the googletest framework
wget https://github.com/google/googletest/archive/release-1.8.0.tar.gz
Or get it by hand. I won't maintain this little How-to, so if you stumbled upon it and the links are outdated, feel free to edit it.

2. Unpack and build google test
tar xf release-1.8.0.tar.gz
cd googletest-release-1.8.0
cmake -DBUILD_SHARED_LIBS=ON .
make

3. "Install" the headers and libs on your system.
This step might differ from distro to distro, so make sure you copy the headers and libs in the correct directory. I accomplished this by checking where Debians former gtest libs were located. But I'm sure there are better ways to do this. Note: make install is dangerous and not supported

$ sudo cp -a include/gtest /usr/include
$ sudo cp -a libgtest_main.so libgtest.so /usr/lib/
4. Update the cache of the linker
... and check if the GNU Linker knows the libs

$ sudo ldconfig -v | grep gtest
If the output looks like this:

libgtest.so.0 -> libgtest.so.0.0.0
libgtest_main.so.0 -> libgtest_main.so.0.0.0
, everything is fine.

gTestframework is now ready to use. Just don't forget to link your project against the library by setting -lgtest as linker flag and optionally, if you did not write your own test mainroutine, the explicit -lgtest_main flag.

From here on you might want to go to Googles documentation about the framework to learn how it works. Happy coding!

How to build a simple example?

The first step is download the source code and build the GTest library, which can be performed using g++ compiler (replace $(GTEST_DIR) by the place of GTest directory):

  wget http://googletest.googlecode.com/files/gtest-1.6.0.zip
  unzip gtest-1.6.0.zip
  g++ -I ${GTEST_DIR}/include -I ${GTEST_DIR} -c ${GTEST_DIR}/src/gtest-all.cc
  ar -rv libgtest.a gtest-all.o


Reference:
https://stackoverflow.com/questions/13513905/how-to-setup-googletest-as-a-shared-library-on-linux
