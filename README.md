# cstream-desktop

The cstream desktop metalayer for OpenCharly images — a streamed Hyprland
desktop, as one name.

`cstream-desktop` composes two members as one candy:

- **`pod-cstream`** — the transport spine: the Wayland parent, gateway, PAM
  stack, and the streaming gates.
- **`pod-hyprland`** — the nested compositor: Hyprland, its Lua config, and the
  file-capability strip.

It installs nothing itself. Composition **order** is the reason it exists:
Hyprland has no headless mode, and Aquamarine's DRM backend needs a KMS card
node a rootless pod never gets, so Hyprland can only run nested in a parent that
advertises `zwp_linux_dmabuf_v1` and binds `xdg_wm_base` at version 6.
`pod-cstream` is that parent. Composing `pod-hyprland` alone builds cleanly and
dies at start with `CBackend::create() failed!` — an error that names the
compositor rather than the missing parent.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `cstream-desktop` (metalayer) |
| Composes | `pod-cstream`, `pod-hyprland` (pinned `@github` refs) |
| Install content | none — a composition assertion only |
| Service / port | none of its own (the members own theirs) |

## How to use it

Compose the metalayer by pinning this repo in a box's `candy:` list:

```yaml
my-streamed-desktop:
  candy:
    base: cachyos
    candy:
      - '@github.com/opencharly/layer-cstream-desktop:v2026.243.1831'
```

The candy's `plan:` asserts both halves are present after composition — one
binary from each member (`/usr/local/bin/cstream-parent` from `pod-cstream`,
`/usr/bin/Hyprland` from `pod-hyprland`), so a missing parent or compositor
fails the check rather than failing at container start.

## Layout

- `charly.yml` — the `cstream-desktop:` candy entity: the two member `candy:`
  refs and the composition `check:`. It declares **no `skill:` entity**.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: none — this candy declares no `skill:` entity; the gap is tracked
  in [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
  Closest family skill: `/charly-distros:omarchy-cstream` (the streamed Omarchy
  desktop, owned by `distro-omarchy`).
- Members: `pod-cstream`, `pod-hyprland`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
