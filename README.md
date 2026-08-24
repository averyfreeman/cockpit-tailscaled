# Cockpit application to manage Tailscale

A [Cockpit](https://cockpit-project.org) application to manage
[Tailscale](https://tailscale.com) on the local machine: connection state,
login flow and client settings, plus an overview of the reachable peers.

![Screenshot](./docs/screenshot.png)

## Features

* View the connection state of this machine and its tailnet
* Connect / disconnect from the tailnet, including the login flow
* Manage client settings (`tailscale set`):
  Tailscale SSH, Accept DNS, Accept subnet routes, Shields up,
  exit node selection (use and advertise), subnet routes advertisement,
  hostname override and automatic updates
* Inspect reachable peers with search, sorting and expandable details
  (addresses, connection type, traffic)
* Capability badges per peer: SSH, exit node offers, advertised subnets
  and key-expiry state

Requires **Tailscale >= 1.34** (for `tailscale set`) and Cockpit >= 137.

## Development

### Requirements

* Node.js >= 20 and npm >= 9
* A running `tailscaled` for the page to show live data

```bash
$ npm install
```

### Build

Development build with watch mode:

```bash
$ npm run dev        # or: npm run watch
```

Production build (writes to `dist/`, including `bundle.js.LEGAL.txt`):

```bash
$ npm run build
```

### Quality checks

```bash
$ npm run lint       # ESLint
$ npm run typecheck  # TypeScript strict mode
$ npm run test:unit  # node:test unit tests
```

The service layer (`src/services/tailscale.ts`) wraps the `tailscale`
CLI; all spawns use argument arrays and elevated writes request
superuser through Cockpit's permission system.

### Install into Cockpit

Copy `dist/` to `/usr/local/share/cockpit/tailscale` (root) or
`~/.local/share/cockpit/tailscale` (user session), or create a symlink
for development:

```bash
$ npm run link      # symlink ~/.local/share/cockpit/tailscale -> dist/
$ npm run unlink    # remove it again
```

`linkusr` / `unlinkusr` do the same under `/usr/local/share/cockpit`
using `sudo`. Cockpit caches page assets, so log out and back in after
linking for the first time.

### RPM package

```bash
$ npm run rpm
```

## License

LGPL-2.1-or-later, see [LICENSE](./LICENSE).

Based on the original cockpit-tailscale work by
[Gerard Braad (@gbraad)](https://github.com/gbraad).
