# mullvad-browser

Arch Linux package of [Mullvad Browser](https://mullvad.net/browser), as
Mullvad releases it, for the Maze Linux repository.

The upstream Linux tarball from `cdn.mullvad.net` is installed unmodified to
`/opt/mullvad-browser`. The package only adds what a system-wide install needs:

- `is-packaged-app`: the browser keeps its profile in `~/.mullvad-browser`
  instead of the read-only install directory.
- `policies.json`: the built-in updater is turned off and its binary removed;
  pacman updates the browser.
- `/usr/bin/mullvad-browser`, a menu entry and hicolor icons.
- Permissions: upstream ships `Browser/` as `0700`; the package makes it
  readable for all users.

The Maze look is a separate package on top of this one:
[maze-mullvad-browser-theme](https://github.com/berk-kucuk/maze-mullvad-browser-theme).

## Build

```sh
makepkg -s
sudo pacman -U mullvad-browser-<version>-x86_64.pkg.tar.zst
```

makepkg checks the upstream tarball's sha256 and its signature by the Tor
Browser Developers key (`EF6E286DDA85EA2A4BA7DE684E2C6E8793298290`). If it
reports an unknown public key, import the current key first:

```sh
curl -s https://openpgpkey.torproject.org/.well-known/openpgpkey/torproject.org/hu/kounek7zrdx745qydx6p59t9mqjpuhdf | gpg --import
```

## Updating

Mullvad Browser gets security releases often, and with the built-in updater off
this package is the only way users get them. For a new release:

1. Set `pkgver` and reset `pkgrel=1`.
2. Put the sha256 of `mullvad-browser-linux-x86_64-<version>.tar.xz` from
   `https://cdn.mullvad.net/browser/<version>/sha256sums-signed-build.txt` in the
   first `sha256sums` entry.
3. `makepkg -s` and publish.

## License

Mullvad Browser is MPL-2.0 with the licenses of its bundled components; they are
installed to `/usr/share/licenses/mullvad-browser/`. "Mullvad" is a trademark of
Mullvad VPN AB.
