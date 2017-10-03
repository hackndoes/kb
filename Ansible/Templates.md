# Render same template with list of items generating multiple files each per item values
### variable file
```yaml
rules:
  - name: example_rule
    type: frequency
    index: logstash-*
    num_events: 1000
    timeframe:
      hours: 4
    filters:
      - type: term
        content:
          key: containerName
          value: "logstash"
```
### template file
```yaml
es_host: "{{ elasticsearch_host }}"
es_port: "{{ elasticsearch_port }}"
type: frequency
name: "{{ item.name }}"
index: "{{ item.index }}"
num_events: "{{ item.num_events }}"
timeframe:
    "{{ item.timeframe | to_nice_yaml }}"
{% for fltr in item.filters %}
filter:
  - {{ fltr.type }}:
        {{ fltr.content.key }}:{{ fltr.content.value }}
{% endfor %}
```
### Tip
To get a dictionary from var to template use the to_nice_yaml filter!!!