# gpui-aros

The AROS platform backend for [GPUI](https://github.com/zed-industries/zed),
the UI framework of the Zed editor: GPUI applications run as native AROS
programs on AArch64, in Intuition windows drawn by a CPU rasterizer.
It is part of [PomAROS](https://pomaros.com).

| Crate | What |
|---|---|
| [`crates/gpui_aros`](crates/gpui_aros/README.md) | the backend: Intuition windows and input, tiny-skia rendering blitted through CyberGraphics, cosmic-text, clipboard, GadTools menus, ASL requesters, the run loop and dispatcher |
| `crates/gpui_platform_aros` | the `gpui-pre-platform` package with an AROS arm that selects the backend |
| `crates/gpui_aros_smoke` | the smallest AROS consumer: one window, every renderer primitive, keyboard echo |

## Using it

GPUI comes from the `gpui-pre` crates.io snapshots; this repository follows
the snapshot that [gpui-kit](https://github.com/pomaros/gpui-kit) pins
(0.3.8 today). An application keeps `gpui-pre`, `gpui-pre-platform` and
gpui-kit on that version and, for its AROS build only, swaps the platform
package:

```toml
[patch.crates-io]
raw-window-handle = { git = "https://github.com/pomaros/raw-window-handle", rev = "39372f3" }
gpui-pre-platform = { git = "https://github.com/pomaros/gpui-aros", rev = "..." }
```

Building for AROS needs the native AROS Rust SDK and C SDK; the backend's C
glue is compiled by `crates/gpui_aros/build.rs` against the SDK named by
`AROS_SDK`. On any other host the crate builds without its AROS shell, and

```sh
cargo test -p gpui_aros
```

runs the renderer and input conformance suite.

## Status

[Ferail](https://github.com/jonx/Ferail) runs on it on AROS. The Zed port
carries the same backend files through its own platform layer.

## License

Apache-2.0, like GPUI; see [LICENSE](LICENSE) and [NOTICE](NOTICE). The
bundled fonts are under the SIL Open Font License 1.1.
