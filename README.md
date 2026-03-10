# plymouth-theme-generator

A Bash script that generates a **Plymouth theme from a GIF animation.

The script extracts the frames from a GIF, optionally resizes them to match the target screen resolution, and generates the required `.script` and `.plymouth` files automatically.

---

# Requirements

* **ImageMagick** (`magick` or `convert`)
* **Plymouth**

Most Linux distributions already provide these packages.

---

# Usage

Basic usage:

```bash
./generator.sh animation.gif
```

The theme name will be the same as the GIF file name.

Example:

```bash
./plymouth-theme-generator boot.gif
```

This will generate and install:

```
/usr/share/plymouth/themes/boot/
```

---

# Modes

The script supports two modes.

## cover (default)

```bash
./plymouth-theme-generator -m cover animation.gif
```

Frames are resized and cropped to match the detected screen resolution.

This ensures the animation fills the screen.

---

## centre

```bash
./plymouth-theme-generator -m centre animation.gif
```

Frames are extracted without scaling.

The animation will be centered on the screen.

---

# Screen resolution detection

The script automatically detects the target resolution using the Linux **Direct Rendering Manager (DRM)** subsystem.

Detection logic:

1. Prefer internal laptop displays (`eDP`, `LVDS`, `DSI`)
2. Otherwise select the connected monitor with the highest resolution
3. Fallback to `1920x1080` if detection fails

This allows the generator to work without relying on **X.Org Server** or **Wayland**.

---

# Installing the theme

The script installs the theme to:

```
/usr/share/plymouth/themes/
```

At the end it prints the activation command for your distribution.

Examples:

Arch / Artix:

```bash
sudo plymouth-set-default-theme -R theme-name
```

Debian / Ubuntu / Mint:

```bash
sudo update-alternatives --config default.plymouth
sudo update-initramfs -u
```

After that, reboot to see the new boot animation.

---

# Notes

* Very large GIFs can generate hundreds of PNG frames and significantly increase boot time.
* High resolution animations (4K) may produce very large theme sizes.
* For best results use short animations and moderate resolutions.
