# Filter on output from STDOUT structured with Yaml after returned as JSON

## structure cmd_out text into an Ansible object and filter on that

<span style="color:yellow">__**Tip: in when statement you can use all jinja filters only drop the {{}} its a given**__</span>
```yaml
# return all tags for specific elasticache instance/cluster
- name: List tags for elasticache Resource
  changed_when: False
  register: cmd_out
  shell: "aws elasticache list-tags-for-resource --resource-name {{ elasticache_arn }}"

# take command output as text and structure into tag_list object
- set_fact:
    tag_list: "{{ cmd_out.stdout|from_json }}"

- debug:
    var: tag_list
    verbosity: 1

# filter on structured data in when statement using filters
- name: Tag elasticache Resource
  register: elasticache_tags
  when: tag_list.TagList|selectattr('Key','equalto','environment')|list|length < 1
  shell: "aws elasticache add-tags-to-resource --resource-name {{ elasticache_arn }} --tags Key=environment,Value={{ envname }}"

```