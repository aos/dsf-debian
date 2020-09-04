## Ubuntu Package for diff-so-fancy

This repository holds the `debian/` files responsible for packaging
[diff-so-fancy](https://github.com/so-fancy/diff-so-fancy).

The PPA can be [found here](https://launchpad.net/~aos1/+archive/ubuntu/diff-so-fancy).

This package uses the overlay method with `git-buildpackage`. That is, this
package only holds the `debian` directory, downloads the release tarball,
overlays the `debian` directory, and creates the package.

### Requirements

In order to build this package you will need the following packages:

- `sbuild` (https://wiki.debian.org/sbuild)
- `git-buildpackage`

### New Release Usage (Ubuntu)

1. Make necessary directories if they don't exist:
```
$ mkdir tarballs build
```
2. Download latest release with `uscan --destdir tarballs`
   (This might be broken, you can instead download the release tarball and
   rename it to the appropriate name.)
3. Make changes if necessary, and run `gbp dch`
4. Run:
```
$ gbp buildpackage --git-builder="sbuild --source-only-changes --debbuildopts='--buildinfo-option=-O'"
```

This will create a `source-only package`, and not write-out the `.buildinfo`
(`-O`) flag. It also uses `sbuild` to make the package.
(See https://bugs.launchpad.net/launchpad/+bug/1699763)

5. Upload the changes file to PPA:
```
$ dput ppa:aos1/diff-so-fancy build/diff-so-fancy_1.2.6-1ubuntu1_source.changes
```

### Create Binary Packages (`.deb`)

Follow steps 1 - 3 above. Then, run this command instead:
```
$ gbp buildpackage --git-builder=sbuild
```

All other configuration is stored under `debian/gbp.conf`.
