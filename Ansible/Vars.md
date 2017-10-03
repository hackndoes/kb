# Reference a variable in a task using a dynamically constructed name
## Use the global play bound vars object with brackets
```python
vars[var+'_'+name]
```
```yaml
- set_fact: {"{{ (item.name+'_instances')|replace('-', '_') }}": "{% for result in ec2_instances.results %}{% for tagged in result.tagged_instances %}{% if item.name in tagged.tags.roles.split(',') %}{{ tagged.id }} {% endif %}{% endfor %}{% endfor %}"}
  with_items: '{{ load_balanced_roles }}'
  
- name: do something
  with_items: '{{ load_balanced_roles }}'
  module_name:
    instance_ids: "{{ vars[item.name.replace('-', '_') +'_instances'].strip().split(' ') }}"
```

# Use item in when query nested variable expension
## Bad: the item.lb_subnet_name is surrounded with {{}}
```yaml
- debug: msg="{{ (vpc_subnet.results|selectattr('subnet.tags.net', 'equalto', {{ item.lb_subnet_name }})|first).subnet.id }}"
  with_items: '{{ load_balanced_roles }}'
```
## Bad: the item.lb_subnet_name is surrounded with ""
```yaml
- debug: msg="{{ (vpc_subnet.results|selectattr('subnet.tags.net', 'equalto', 'item.lb_subnet_name')|first).subnet.id }}"
  with_items: '{{ load_balanced_roles }}'
```
## Good: just use item.lb_subnet_name
```yaml
- debug: msg="{{ (vpc_subnet.results|selectattr('subnet.tags.net', 'equalto', item.lb_subnet_name)|first).subnet.id }}"
  with_items: '{{ load_balanced_roles }}'
```

# Build var from complex data

 ### I found myself in need to filter items on some more complex conditions a few times and started using the Jinja do extension to effectively turn variable assignments into arbitrary computations:

```yaml
my_list: |
  {%- set names = [] -%}
  {%- for item in my_dict if item.item_en -%}
    {%- do names.append(item.name) -%}
  {%- endfor -%}
  {{ names }}
```
It's important to do proper whitespace-control (the dashes in {%- ... -%}) because in the end the string assigned to my_list must be "['name1', 'name3']" and not something like "   ['name1', 'name3']". In the former case, Ansible converts it to a proper list (because the value starts with '['), in the latter my_list would be assigned the actual string "   [...]".

<span style="color:red">**__You need to enable the Jinja extension in your ansible.cfg:__**</span>
```ini
[defaults]
jinja2_extensions = jinja2.ext.do
```

As mentioned in the beginning, this isn't really necessary in this case and certainly isn't very much in Ansible's spirit of simplicity, but it has saved me from duplicating information across multiple variables several times.

# Variables Order of Precedance
1. role defaults [1]
2. inventory INI or script group vars [2]
3. inventory group_vars/all
4. playbook group_vars/all
5. inventory group_vars/*
6. playbook group_vars/*
7. inventory INI or script host vars [2]
8. inventory host_vars/*
9. playbook host_vars/*
10. host facts
11. play vars
12. play vars_prompt
13. play vars_files
14. role vars (defined in role/vars/main.yml)
15. block vars (only for tasks in block)
16. task vars (only for the task)
17. role (and include_role) params
18. include params
19. include_vars
20. set_facts / registered vars
21. extra vars (always win precedence)