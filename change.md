# Mouse Device Configuration Changes

## Summary

Added support for configurable mouse device path through the configuration file. The `find_mouse_device` function now accepts an optional device path from the configuration and uses it if provided, falling back to automatic detection if not specified or if the configured path fails.

## Changes Made:

### 1. Config struct (src/config.rs:24)
- Added `mouse_device_path: Option<String>` field with a default value of `None`

### 2. find_mouse_device function (src/mouse_listener.rs:20)
- Modified to accept `configured_path: Option<&str>` parameter
- Now checks the configured path first before falling back to automatic detection
- If the configured path fails to open, it prints a warning and falls back to auto-detection

### 3. run_listener function (src/mouse_listener.rs:98)
- Updated signature to accept `mouse_device_path: Option<String>`
- Passes the path to `find_mouse_device`

### 4. spawn method (src/mouse_listener.rs:86)
- Clones `mouse_device_path` from config and passes it to `run_listener`

## Usage

Users can now add the following to their config file (`~/.config/nicotine/config.toml`):

```toml
mouse_device_path = "/dev/input/event3"
```

If the path is not specified or is `None`, the application will continue to auto-detect the mouse device as before.
