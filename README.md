<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/agmmnn/tauri-controls/assets/16024979/8ac8cae0-9cde-4b27-af8b-02d949ede7a2">
  <img alt="Shows an illustrated sun in light color mode and a moon with stars in dark color mode." src="https://github.com/agmmnn/tauri-controls/assets/16024979/12f46aa7-72de-4c44-aa4a-0580c73fc73a">
</picture>

![](https://img.shields.io/bundlephobia/min/tauri-controls)
![](https://img.shields.io/bundlephobia/minzip/tauri-controls)
[![](https://img.shields.io/npm/dt/tauri-controls)](https://npmjs.com/package/tauri-controls)

**Tauri Controls** is a library that provides native-looking **window controls** for [Tauri](https://github.com/tauri-apps/tauri) 2 applications. You can enhance the user experience of your Tauri 2 applications with window controls that mimic the identical native controls on the current system.

`tauri-controls` uses Tauri's [js/ts APIs](https://next--tauri.netlify.app/next/api/js) to handle window events and just provides native-looking (designed according to official system design prototypes) html/react elements, not native, _it does not rely on the system's native APIs_.

The following designs are taken as reference:

- [Windows UI 3](https://www.figma.com/community/file/1159947337437047524) @microsoft
- [Apple Design Resources - macOS](https://www.figma.com/community/file/1251588934545918753) @apple

## How to use

### Install Dependencies

```bash
pnpm add tauri-controls

#install peer dependencies
pnpm add @tauri-apps/plugin-os @tauri-apps/plugin-window
pnpm add -D clsx tailwind-merge
```

Then, make sure to include the following tauri plugins in your `src-tauri` directory:

```bash
cargo add tauri-plugin-window tauri-plugin-os
```

Don't forget to register plugins in your main function.

```rust
fn main() {
    tauri::Builder::default()
        .plugin(tauri_plugin_os::init())
        .plugin(tauri_plugin_window::init())
        .run(tauri::generate_context!())
        .expect("error while running tauri application");
}
```

### Add to Your Code

And simply add the `WindowControls` component to your code:

```tsx
import { WindowControls } from "tauri-controls"

function MyTitlebar() {
  return <WindowControls />
}
```

![](https://github.com/agmmnn/tauri-controls/assets/16024979/7be3dde4-7953-4188-af12-abd4445c0bf9)

When no platform is specified, the current system will be detected and the matching element will be returned. It's a great solution for cross-platform releases.

WindowTitlebar component handles the window controls and dynamically adjusts the control buttons and titlebar content order based on the current operating system.

```tsx
import { WindowTitlebar } from "tauri-controls"

function MyTitlebar() {
  return (
    <WindowTitlebar>{/* Place your titlebar content here */}</WindowTitlebar>
  )
}
```

![](https://github.com/agmmnn/tauri-controls/assets/16024979/214677d4-dd70-4e6b-96c3-b9d1a1356f05)

> _If you get the message "Not allowed by scope" in the terminal after a production build, try [this](https://github.com/agmmnn/tauri-controls/issues/1#issuecomment-1653557673)._

### Options

```ts
// WindowTitlebar:
controlsOrder?: "right" | "left" | "platform" | "system"
windowControlsProps?: WindowControlsProps

// WindowControls:
platform?: "windows" | "macos" | "gnome"
hide?: boolean
hideMethod?: "display" | "visibility"
```

You can also pass additional `props` to elements like `data-tauri-drag-region` for further enhancements.

### Examples:

```tsx
<WindowControls platform="macos" className="my-4" />
```

![](https://i.imgur.com/OAO22HC.png)

```tsx
<WindowControls
  platform="windows"
  className="w-full justify-end"
  data-tauri-drag-region
/>
```

![](https://i.imgur.com/hq389kn.png)

[More examples](https://github.com/agmmnn/tauri-controls/blob/master/src/App.tsx).

# To-Do

If the library gets some interest, I can gradually add the following features:

- [x] If no platform is specified, the side of the controls will also be determined automatically. (e.g. MacOS left, others right)
- [x] Next.js/SSR support.
- [ ] Detect disabled window controls (is_maximizable, ...) and disable the buttons accordingly.
- [ ] Svelte/SvelteKit implementation.

## Figma

Check out the design implementation on Figma for a visual reference. [Desktop Native Window Controls - Figma](https://www.figma.com/file/ms2vbZx5lEGxHqHR8fAfQm/Desktop-Native-Window-Controls?type=design&node-id=4%3A6020&mode=design&t=PIbVTsr8zWmIFsNr-1).

These sources were utilized:

- [Windows UI 3](https://www.figma.com/community/file/1159947337437047524) @microsoft
- [Apple Design Resources - macOS](https://www.figma.com/community/file/1251588934545918753) @apple
- [macOS Monterey UI Kit for Figma](https://www.figma.com/community/file/1034539431656086181/macOS-Monterey-UI-Kit-for-Figma) @joey
- [Spotify Desktop App Clone](https://www.figma.com/community/file/1028665514709480268/Spotify-Desktop-App-Clone) @uidesignguide

## Further Reading

- [Feature request: Add setting for titlebar style with native window controls support - tauri-apps/tauri#2663](https://github.com/tauri-apps/tauri/issues/2663)
- [Window Controls Overlay for Installed Desktop Web Apps - WICG](https://github.com/WICG/window-controls-overlay/blob/main/explainer.md)
- [Window Controls Overlay - Electron](https://www.electronjs.org/docs/latest/tutorial/window-customization#window-controls-overlay-macos-windows)
- [Window Controls Overlay API - MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window_Controls_Overlay_API)

## License

MIT
