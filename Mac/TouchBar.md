# Sudo With Touch ID
Pro MacBook Pro Tip: have a Touch Bar with Touch ID? If you edit /etc/pam.d/sudo and add the following line to the top…
```s
auth sufficient pam_tid.so
```
you can now use your fingerprint to sudo!