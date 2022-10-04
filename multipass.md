# Multipass Setup
```cmd
PS> multipass set client.primary-name=jimny
PS> multipass set client.gui.autostart=false
PS> multipass set local.driver=hyperv
```

## Multipass Configuration
```bash
PATH="$PATH:/home/user/snap/multipass/common/bin"
```
```cmd
PS> set PATH="%PATH%:C:\Users\<user>\AppData\Local\Multipass\bin”
```

## Multipass Settings
```cmd
PS> multipass get --keys
client.apps.windows-terminal.profiles
client.gui.autostart
client.gui.hotkey
client.primary-name
local.bridged-network
local.driver
local.passphrase
local.privileged-mounts
```

## Multipass Default folders
```cmd
C:\Program Files\Multipass\
C:\ProgramData\Multipass\
C:\Users\<user>\AppData\Local\Multipass\
```
