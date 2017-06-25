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