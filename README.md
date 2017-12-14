# coolpkg

coolpkg is a source level package manager inspired heavily by nix.

The project goals are as follows:

- Reproduceable builds always.
- Nix's power, just far far simpler in nearly every way.
- Forking a package and adding custom changes is just a single file copy.
- Mixing versions of packages freely on the same machine, no dependency hell ever.
- Work on freebsd/openbsd.
- Different package trees per OS, don't force abstractions. Ok to depend on BSD
  base systems/compilers.
- Push self contained packages to other machines via ssh, even if they don't have ``coolpkg``.
- Encourage users to curate their own custom/packaged package tree/repo, make this easy.

# Status

Prototype and proof of concept.

# Getting started:

Add ```coolpkg``` to your path (requires python3).

## Try the example:

Read the file example.coolpkg, hopefully it is quite simple.

run the command below to install the package and it's dependencies to ~/coolpkgstore/:

```$ coolpkg ./example/example.coolpkg```

This will do some cool things...

- Bootstrap the package set from a sha256 checksummed url to get a rule for downloding git packages.
- Use the downloaded rule to install a fixed copy coolpkgs package sets, install that as a package.
- Install go1.9 from that set, implicitly knowing to use go1.4 from the same set to bootstrap itself.
- Use that new version of go.

# How it works:

coolpkg-build hashes the script and any arguments and uses that to calculate the install
directory. The package is content addressed, if you change any aspect of a package,
including it's dependencies, the install paths change, they never conflict and shared
dependencies are shared.

package scripts are simply shell scripts that have a single contract, to follow...
install into $out (currently ~/coolpkgstore/$HASH/), and be reproducible by recursively
installing your dependencies from specific commits via coolpkgs.


# Todo

- Implement the garbage collector  ```coolpkgs-collect-garbage [rootdir...]```
- Better isolation of builds.
- Write the best documentation in the world. Show how to do atomic upgrades
  by building a set of symlinks as a package, then using mv to overwrite an old one.

- parent's are not correctly settings deps. Need to add redo style .dep file or something.