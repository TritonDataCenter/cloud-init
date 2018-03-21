# Joyent fork of cloud-init

This README contains hints to help you get started with any cloud-init hacking
for Joyent's images.

## Get the bits

```
~ $ git clone git@github.com:joyent/cloud-init.git
Cloning into 'cloud-init'...
remote: Counting objects: 29974, done.
remote: Compressing objects: 100% (280/280), done.
remote: Total 29974 (delta 252), reused 274 (delta 155), pack-reused 29536
Receiving objects: 100% (29974/29974), 18.17 MiB | 29.40 MiB/s, done.
Resolving deltas: 100% (21960/21960), done.

~ $ cd cloud-init

~/cloud-init $ git checkout joyent-bhyve

```

## Install dependencies

### Debian or Ubuntu with python3

```
~/cloud-init $ sudo apt update

~/cloud-init $ sudo apt install -y $(python3 ./tools/read-dependencies \
    --requirements-file requirements.txt --system-pkg-names  -d debian)
...

~/cloud-init $ sudo apt install -y $(python3 ./tools/read-dependencies \
    --requirements-file test-requirements.txt --system-pkg-names  -d debian)
...
```

## Run tests and generate package

Some of the tests try to connect to a link-local metadata service at
169.254.16.254.  The timeouts of the failed connects will take hours.  Avoid
that with a reject route.

```
~/cloud-init $ sudo route add 169.254.169.254 reject
```

### Build on Debian or Ubuntu

This runs tests before building the package.  If tests fail, no package for you.
Uncommitted changes also cause this to fail.

```
~/cloud-init $ make deb
'/usr/bin/debuild
python3 ./packages/bddeb
Creating a temporary tarball using the 'make-tarball' helper
Extracting temporary tarball 'cloud-init_17.2-33-gd40a3dc0.orig.tar.gz'
Creating a debian/ folder in '/tmp/tmpmefsxw0i/cloud-init-17.2-33-gd40a3dc0'
Running 'debuild -us -uc' in '/tmp/tmpmefsxw0i/cloud-init-17.2-33-gd40a3dc0'
Wrote 'cloud-init_17.2-33-gd40a3dc0-1~bddeb.dsc'
Linked 'cloud-init_17.2-33-gd40a3dc0-1~bddeb.dsc' to 'cloud-init.dsc'
Wrote 'cloud-init_17.2-33-gd40a3dc0.orig.tar.gz'
Wrote 'cloud-init_17.2-33-gd40a3dc0-1~bddeb_amd64.changes'
Wrote 'cloud-init_17.2-33-gd40a3dc0-1~bddeb.debian.tar.xz'
Wrote 'cloud-init_17.2-33-gd40a3dc0-1~bddeb_all.deb'
Linked 'cloud-init_17.2-33-gd40a3dc0-1~bddeb_all.deb' to 'cloud-init_all.deb'
Wrote 'cloud-init_17.2-33-gd40a3dc0-1~bddeb_amd64.build'
```
