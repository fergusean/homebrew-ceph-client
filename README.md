# Ceph client libraries for Homebrew

This is a [Ceph][] tap for [Homebrew][].

Mac users can use these formulae to easily install and update Ceph libraries.

## Initial setup

If you don't have Homebrew, install it from their [homepage][homebrew].

Then, add this tap:

```
brew tap bibarrav/ceph-client
```

## Installing

To install the Ceph client libraries:

```
brew install --cask macfuse
brew install ceph-client
```

## Updating

Simply run:

```
brew update
brew upgrade ceph-client
```

## Configuration

To automatically create configuration and authentication file, run this commands in ceph:

```
sudo mkdir /etc/ceph
ssh {user}@{mon-host} "sudo ceph config generate-minimal-conf" | sudo tee /etc/ceph/ceph.conf
chmod 644 /etc/ceph/ceph.conf
ssh {user}@{mon-host} "sudo ceph fs authorize cephfs client.{client_name} / rw" | sudo tee /etc/ceph/ceph.client.{client_name}.keyring
sudo chmod 600 /etc/ceph/ceph.client.foo.keyring
```

## Mounting CephFS

First, create a path/folder where the CephFS wants to be located

```
mkdir ~/cephfs
```

Second, initiate fuse client.  
```
sudo /usr/local/Cellar/ceph-client/17.2.7_1/bin/ceph-fuse ~/cephfs -n client.{client_name}
```
The first time configuring FUSE, MacOSX requiere you to allow it in System Settings/Privacy & Security, then restart.

After restart, run the same command again and you should get an output like this:
```
ceph-fuse[1670]: starting ceph client
2024-10-25T06:58:02.404-0300 7ff84aa05dc0 -1 init, newargv = 0x7fe80f604300 newargc=5
ceph-fuse[1670]: starting fuse
```

## Installing (Build process and bottle creation - tested on ventura)

Install requirements
```
xcode-select --install
brew install python@3.11
pip3.11 install "cython<3.0.0"
brew install --cask macfuse
```

Then, follow install process including bottle creation
```
brew tap bibarrav/ceph-client
brew install ceph-client --build-bottle
brew bottle ceph-client
```


[homebrew]: http://brew.sh/
[ceph]: https://ceph.com/
