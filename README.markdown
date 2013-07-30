Checkout and build, you may need to install some development
packages for your particular distribution.

$ git clone git@github.com:behlendorf/xfstests.git
$ cd xfstests
$ git checkout -b zfs-upstream origin/zfs-upstream
$ make -s

Create two test partitions to be used for testing.  On one
partition create a zpool and set the mountpoint property to
legacy, this allows the xfstests framework to mount/unmount
zfs datasets like other Linux filesystems.

$ zpool create -f -m legacy -O acltype=posixacl test /dev/vdb
$ zpool create -f -m legacy -O acltype=posixacl scratch /dev/vdc
$ zpool destroy scratch
$ mkdir /test
$ mkdir /scratch

Next setup your environment for xfstests, additional
detail on the valid options can be found in the README.

$ export TEST_DEV=/dev/vdb1
$ export TEST_DIR=/test
$ export SCRATCH_DEV=/dev/vdc1
$ export SCRATCH_MNT=/scratch

The full suite of generic tests can be run as follows:

$ ./check -zfs generic/[0-9][0-9][0-9]
