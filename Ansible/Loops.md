# Loops
## matching values from two lists
### with_together
If you want to copy different config files to to different directories for example you could
```yaml
- name: copy config files
  with_together:
    - [ "file1", "file2" ]
    - [ "/usr/share/myapp/config1", "/usr/share/myapp/config2"]
  template:
    name: "{{ item.0 }}.j2"
    dest: "{{ item.1}}/{{ item.0 }}"
```
this will take template file from first list in each iteration and reference it as item.0
and save it on the managed host in the location designated by value from second list
which is the correct config directory and the matching config file. 
so the result will be:

/usr/share/myapp/config1/file1
/usr/share/myapp/config2/file2