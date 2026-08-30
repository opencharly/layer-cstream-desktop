# layer-cstream-desktop

The cstream desktop metalayer: `pod-cstream` (the transport spine — parent, gateway, PAM stack and
the streaming gates) plus `pod-hyprland` (the nested compositor), composed as one name.

It installs nothing itself. Composition **order** is the reason it exists: Hyprland has no headless
mode and Aquamarine's DRM backend needs a KMS card node a rootless pod never gets, so it only runs
nested in a parent advertising `zwp_linux_dmabuf_v1` and binding `xdg_wm_base` at version 6.
`pod-cstream` is that parent. Composing `pod-hyprland` alone builds cleanly and dies at start with
`CBackend::create() failed!` — an error that names the compositor rather than the missing parent.
