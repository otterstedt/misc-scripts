## WSL
- Enable WSL in Windows
- Install XServer
- Install ```terminator, dbus-x11```
- Autostart XServer
- Shortcut to terminator from Win

## Windows
- WinSCP
- Docker for Windows (alt. Docker in VM)

## Docker vs. Virtualization
 - Use Virtualbox - Disable Hyper-V
   ```
   # Run from elevated prompt (admin privileges)
   bcdedit /set hypervisorlaunchtype off
   ```
- Use Docker - Enable Hyper-V 
  ```
  # Run from elevated prompt (admin privileges)
  bcdedit /set hypervisorlaunchtype auto
  ```
