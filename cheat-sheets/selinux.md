ansible: sefcontext

Execute restorecon for existing files folders: restorecon -Rv /path/to/folder
If folders are created after the label is set restorecon is not needed

# http
## http content folder
Parent folders need x permissions
```
label: httpd_sys_content_t
```
## http cache folder
```
label: httpd_cache_t
```
