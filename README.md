# Take-Ownership
Take ownership of a file in Fedora- Nobara, with right clik option menu.

Here’s a ready-to-use setup for Nobara OS (Fedora-based). Nobara’s Official edition uses KDE Plasma + Dolphin, while the GNOME edition uses Nautilus. I’ll give both.

1. KDE / Dolphin (Official Nobara – recommended)
Run these commands in a terminal:
-------------------------------------------------------------------
mkdir -p ~/.local/share/kio/servicemenus

cat > ~/.local/share/kio/servicemenus/take-ownership.desktop << 'EOF'
[Desktop Entry]
Type=Service
ServiceTypes=KonqPopupMenu/Plugin
MimeType=all/all;
Actions=TakeOwnership;
X-KDE-Priority=TopLevel
X-KDE-StartupNotify=false
Icon=user-identity

[Desktop Action TakeOwnership]
Name=Take Ownership
Name[en]=Take Ownership
Icon=user-identity
Exec=pkexec chown -R $USER:$USER %F
EOF

chmod +x ~/.local/share/kio/servicemenus/take-ownership.desktop
-------------------------------------------------------------------

Restart Dolphin (or log out/in): run in terminal:
---------------------
killall dolphin
---------------------

Result: Right-click any file or folder → Take Ownership.

It will ask for your password (via pkexec) and change ownership to your user recursively.
















2. GNOME / Nautilus (GNOME edition of Nobara)

Bash
----------------------------------------------------------------
mkdir -p ~/.local/share/nautilus/scripts

cat > ~/.local/share/nautilus/scripts/"Take Ownership" << 'EOF'
#!/bin/bash
# Take ownership of selected files/folders

if [ -z "$NAUTILUS_SCRIPT_SELECTED_FILE_PATHS" ]; then
    exit 1
fi

# Ask for confirmation
zenity --question --title="Take Ownership" --text="Change ownership of selected items to $USER?" || exit 0

# Run with elevated privileges
pkexec chown -R "$USER:$USER" $NAUTILUS_SCRIPT_SELECTED_FILE_PATHS

zenity --info --title="Done" --text="Ownership changed successfully." 2>/dev/null || true
EOF

chmod +x ~/.local/share/nautilus/scripts/"Take Ownership"
-----------------------------------------------------------------

Restart:
Run in terminal:
-----------------------------------------------------------------

nautilus -q
-----------------------------------------------------------------

Result: Right-click file(s)/folder(s) → Scripts → Take Ownership.

Notes:

I’m not a programmer; I needed a "Take Ownership" feature like the one in Windows, so I turned to AI.

I hope you find this little program useful. Live long and prosper.

20.09.2026 Cluj-Napoca, Romania

Alex B.













