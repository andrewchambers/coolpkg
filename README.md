# coolpkg

coolpkg is a source level package manager inspired heavily by nix.

The project goals are as follows:

- Reproduceable builds always.
- Encourage users to curate their own custom/packaged package tree/repo, make this easy. (Package lists can be packages.)
- Forking a package and adding custom changes is just a single file copy.
  You could make a package to patch a package.
- Mixing versions of packages freely on the same machine, no dependency hell ever.
- Work on freebsd/openbsd.
- Different package trees per OS, don't force abstractions. Ok to depend on BSD
  base systems/compilers.
- Push self contained packages to other machines via ssh, even if they don't have ``coolpkg``.
- Nix's power, just far far simpler in nearly every way.


# Status

Prototype and proof of concept.

# Getting started:

Add ```coolpkg``` and ```coolpkg-install ``` to your path (requires python3).

## Try the example:

Read the file example.coolpkg, hopefully it is quite simple.

run the command below to install the package and it's dependencies to ~/coolpkgstore/:

```$ coolpkg-install ./example/example.coolpkg```

This will do some cool things...

- Install a package set from a git url and install that as a package.
- Install go1.9 from that set, implicitly knowing to use go1.4 from the same set to bootstrap itself.
- Install some other fun software.
- Print out a 'virtual env' you can use
  for running the software.


# How it works:

coolpkg-build hashes the package files and any arguments provided and uses that to calculate the install
directory. The package is content addressed, if you change any aspect of a package,
including it's dependencies, the install paths change, they never conflict and shared
dependencies are shared.

package scripts are simply shell scripts with
dependency information prepended.

package scripts have a simple contract, to follow - install into $out (currently ~/coolpkgstore/$HASH/), be reproducible by only using your listed dependencies (currently enforced by wiping $PATH).

# Todo

- Implement the garbage collector  ```coolpkgs-collect-garbage [rootdir...]```
- Better isolation of builds, fewer stuff in bootstrap.coolpkg.
- Write the best documentation in the world. Show how to do atomic upgrades
  by building a set of symlinks as a package, then using mv to overwrite an old one.
