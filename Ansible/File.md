# Operate on list of files from controller

## will run the template module where the source will be each time another file from the rules folder
```yaml
 - name: "Copy rules"
   with_fileglob:
     - "rules/*"
   template:
     src: "{{ item }}"
     dest: "{{ elastalert_home }}/rules/{{ item | basename | splitext | first }}"
```

# Remove a list of unwanted files (as var to script) from remote host
## In this example remove all the files not in the list of <i>wanted_file_base_names</i>
```python
- set_fact:
    wanted_file_base_names:
      - first.file.name
      - second.file.name

- name: find all exiting files in the host directory
  register: all_files
  find:
    paths: "{{ host_dir_path }}"
    
# each file in all_files.files has a path attribute with the full path
# and we filter by the basename of a file only
- name: "Remove all unwanted files from {{ host_dir_path }}"
  with_items: "{{ all_files.files }}"
  # take only the file name and strip the extention to match values from
  # wanted_file_base_names list exactly.
  when: item.path|basename|splitext|first not in wanted_file_base_names
  file:
    path: "{{ item.path }}"
    state: absent
```

[Delete Files](http://www.mydailytutorials.com/ansible-delete-multiple-files-directories-ansible/)