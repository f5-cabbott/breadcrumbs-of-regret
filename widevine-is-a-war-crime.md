# Is this why we have frigates on ourrr digital oceans again?

I just wanted to watch "The Office" while on holiday in France.... and since moving to arm64/aarch64... it's been an adventure. Not the hobbit kind.

## Phase A: The "Snap" Purge

Why: The default Firefox Snap is a locked-down "jerk" that can't see the custom files we need.

### 1. Uninstall the Snap
```bash
sudo snap remove firefox
```

### 2. Add the Mozilla PPA
```bash
sudo add-apt-repository ppa:mozillateam/ppa
```

### 3. Install Native Firefox
```bash
sudo apt update
sudo apt install firefox
```
<br>

## Phase B: The "ChromeOS" Graft

Why: This steals the secret ARM Widevine engine from Google's Chromebook files.

### 1. Run the Asahi Installer

This puts files in /var/lib/widevine/

```bash
curl -sL https://raw.githubusercontent.com/AsahiLinux/widevine-installer/main/widevine-installer.sh | sudo bash
```

### 2. The "Bolt-Down" (Symlinks)

Replace [YOUR_PROFILE] with your actual default release folder in `~/.mozilla/firefox/`
***Note** Look for a folder with .default-release*

```bash
mkdir -p ~/.mozilla/firefox/[YOUR_PROFILE]/gmp-widevinecdm/4.10.2662.3/
ln -s /var/lib/widevine/libwidevinecdm.so ~/.mozilla/firefox/[YOUR_PROFILE]/gmp-widevinecdm/4.10.2662.3/libwidevinecdm.so
ln -s /var/lib/widevine/manifest.json ~/.mozilla/firefox/[YOUR_PROFILE]/gmp-widevinecdm/4.10.2662.3/manifest.json
```
<br>

## Phase C: The "Internal" Wake-up Call

Why: Forces Firefox to stop looking for Intel (x86_64) code and acknowledge its ARM (AArch64) heart.

**In about:config set/create these:**

1. media.gmp-widevinecdm.visible -> true
2. media.gmp-widevinecdm.enabled -> true
3. media.eme.enabled -> true
4. media.gmp-widevinecdm.abi -> aarch64-gcc3
5. media.gmp-widevinecdm.version -> 4.10.2662.3

<br>

## Phase D: The Sandbox Bypass

Why: Tells Ubuntu 26.10 to lower its security shield so Firefox can actually "read" the stolen Widevine file.

### 1. Create the Config File

```bash
vi ~/.config/environment.d/99-firefox-drm.conf
```

### 2. Add these lines (No quotes!):

```ini
MOZ_GMP_PATH=/var/lib/widevine/
MOZ_DISABLE_GMP_SANDBOX=1
```

### 3. REBOOT THE SYSTEM.

## Phase E: The Identity Lie

Why: Tricks Netflix/Prime into thinking you're a "safe" Chromebook.

### 1. Install "User-Agent Switcher and Manager" extension.

### 2. Use the Chromebook Imposter String:

```ini
Mozilla/5.0 (X11; CrOS aarch64 15329.44.0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36
```
