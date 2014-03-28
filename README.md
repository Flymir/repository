Flymir LuaDist Repository
=========================

This repository aggregates all the supported modules of the Flymir LuaDist project. Its primary purpose is to provide a manifest for Flymir modules which supplement the primary LuaDist repository. Modules are referenced using [git submodules][sub] and should always point to individual module repositories in the Flymir LuaDist project. Its secondary purpose is to act as an install and bootstrap process for Flymir LuaDist based installations.

This repository contains an installation script that allows automated building of LuaDist modules. There are two modes of operation available. First mode is for bootstrapping the _luadist_ deployment utility that offers complete package management functionality and automated dependency resolving. However this requires compilation of _openssl_ and other utilities you may not want.

The second mode of operation directly checks out repositories using git or available submodules and installs the modules without dependency handling. Using this approach you can tailor your distribution from ground up without unneeded dependencies.

Bootstrapping LuaDist deployment tool
---

Please make sure your system has git, CMake 2.8  and a compiler tool-chain available. On Ubuntu this requires git, cmake, build-essential. This build will take quite a while to compile, please be patient.

```bash
git clone git://github.com/Flymir/repository.git
cd repository
git submodule update --init --recursive bootstrap
./install bootstrap
```

Once the installation finishes the LuaDist folder should contain a fully versioned LuaDist distribution.

```bash
cd _install/bin
./luadist list # lists installed modules
./luadist search # lists online repository
./luadist install luaexpat # installs luaexpat
```

Using the install script to generate distribution without versioning.
---

To make a distribution containing luajit, luasocket and luafilesystem you can use the install utility directly:

```bash
./install luajit luasocket luafilesystem
```

Note that this mode of installation installs most recent versions of modules and does not handle dependencies automatically. If you checked out any of the modules using submodules the utility will use the local files, otherwise it will access remote git repositories. However, the installation script is able to install specific tags of modules. It is up to you to install correct dependencies, otherwise the distribution may be unusable.

```bash
./install lua-5.1.4 md5-1.1.2 
./_install/bin/lua
> require "md5"
```

Cloning
-------

To clone the full repository:

```bash	
git clone git://github.com/LuaDist/Repository.git
cd Repository
git submodule update --init --recursive
```

To clone individual modules you can specify the module name as follows:

```bash
git submodule update --init --recursive lua
```

Note that submodules do not point to latest versions of modules but rather to _stable_ versions. To update to latest version do:

```bash
cd module
git checkout master
git pull
```

By default all submodules are accessed using the `git://` protocol. Developers can update all remotes to support push through ssl as reqired by GitHub using the following command:

```bash
git submodule foreach 'git remote set-url --push origin git@github.com:LuaDist/$path.git'
```

We also recommend switching all submodules to the master branch using the following command:

```bash
git submodule foreach 'git checkout master && git pull'
```
