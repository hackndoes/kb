#append string to list of values
```yaml
- set_fact:
    file_list: []

- set_fact:
    file_list: "{{ file_list }} + [ '{{ item.path|basename|splitext|first }}' ]"
  with_items: "{{ cert_files.files }}"
```