---
# This title is used for search results
title: arista.avd.yaml_templates_to_facts
---
<!--
  ~ Copyright (c) 2023-2024 Arista Networks, Inc.
  ~ Use of this source code is governed by the Apache License 2.0
  ~ that can be found in the LICENSE file.
  -->

# yaml_templates_to_facts

!!! note
    Always use the FQCN (Fully Qualified Collection Name) `arista.avd.yaml_templates_to_facts` when using this plugin.

Set facts from YAML via Jinja2 templates

## Synopsis

Set facts from YAML produced by Jinja2 templates

## Parameters

| Argument | Type | Required | Default | Value Restrictions | Description |
| -------- | ---- | -------- | ------- | ------------------ | ----------- |
| <samp>root_key</samp> | str | False | None |  | Root key under which the facts will be defined. If not set the facts well be set directly on root level. |
| <samp>schema</samp> | dict | False | None |  | Schema conforming to \"AVD Meta Schema\". Either schema or schema\_id must be set. |
| <samp>schema_id</samp> | str | False | None | Valid values:<br>- <code>eos_cli_config_gen</code><br>- <code>eos_designs</code> | ID of Schema conforming to \"AVD Meta Schema\".  Either schema or schema\_id must be set. |
| <samp>templates</samp> | list | True | None |  | List of dicts for Jinja templates to be run. |
| <samp>&nbsp;&nbsp;&nbsp;&nbsp;template</samp> | str | False | None |  | Template file. Either template or python\_module must be set. |
| <samp>&nbsp;&nbsp;&nbsp;&nbsp;python_module</samp> | str | False | None |  | Python module to import. Either template or python\_module must be set. |
| <samp>&nbsp;&nbsp;&nbsp;&nbsp;python_class_name</samp> | str | False | AvdStructuredConfig |  | Name of Python Class to import. |
| <samp>&nbsp;&nbsp;&nbsp;&nbsp;options</samp> | dict | False | None |  | Template options. |
| <samp>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;list_merge</samp> | str | False | append |  | Merge strategy for lists |
| <samp>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;strip_empty_keys</samp> | bool | False | True |  | Filter out keys from the generated output if value is null/none/undefined. Only applies to templates. |
| <samp>debug</samp> | bool | False | None |  | Output list \'avd\_yaml\_templates\_to\_facts\_debug\' with timestamps of each performed action. |
| <samp>dest</samp> | str | False | None |  | Destination path. If set, the output facts will also be written to this path.<br>Autodetects data format based on file suffix. \'.yml\', \'.yaml\' \-\> YAML, default \-\> JSON |
| <samp>mode</samp> | str | False | None |  | File mode \(ex. 0664\) for dest file. See \'ansible.builtin.copy\' module for details. |
| <samp>template_output</samp> | bool | False | None |  | If true, the output data will be run through another jinja2 rendering before returning.<br>This is to resolve any input values with inline jinja using variables/facts set by the input templates. |
| <samp>conversion_mode</samp> | str | False | debug | Valid values:<br>- <code>error</code><br>- <code>warning</code><br>- <code>info</code><br>- <code>debug</code><br>- <code>quiet</code><br>- <code>disabled</code> | Run data conversion in either \"error\", \"warning\", \"info\", \"debug\", \"quiet\" or \"disabled\" mode.<br>Conversion will perform type conversion of input variables as defined in the schema.<br>Conversion is intended to help the user to identify minor issues with the input data, while still allowing the data to be validated.<br>During conversion, messages will be generated with information about the host\(s\) and key\(s\) which required conversion.<br>conversion\_mode\:disabled means that conversion will not run.<br>conversion\_mode\:error will produce error messages and fail the task.<br>conversion\_mode\:warning will produce warning messages.<br>conversion\_mode\:info will produce regular log messages.<br>conversion\_mode\:debug will produce hidden messages viewable with \-v.<br>conversion\_mode\:quiet will not produce any messages. |
| <samp>validation_mode</samp> | str | False | warning | Valid values:<br>- <code>error</code><br>- <code>warning</code><br>- <code>info</code><br>- <code>debug</code><br>- <code>disabled</code> | Run validation in either \"error\", \"warning\", \"info\", \"debug\" or \"disabled\" mode.<br>Validation will validate the input variables according to the schema.<br>During validation, messages will be generated with information about the host\(s\) and key\(s\) which failed validation.<br>validation\_mode\:disabled means that validation will not run.<br>validation\_mode\:error will produce error messages and fail the task.<br>validation\_mode\:warning will produce warning messages.<br>validation\_mode\:info will produce regular log messages.<br>validation\_mode\:debug will produce hidden messages viewable with \-v. |
| <samp>output_schema</samp> | dict | False | None |  | AVD Schema for output data. Used for automatic merge of data. |
| <samp>output_schema_id</samp> | str | False | None | Valid values:<br>- <code>eos_cli_config_gen</code><br>- <code>eos_designs</code> | ID of AVD Schema for output data. Used for automatic merge of data. |
| <samp>set_switch_fact</samp> | bool | False | True |  | Set \"switch\" fact from on \"avd\_switch\_facts.\<inventory\_hostname\>.switch\" |

## Examples

```yaml
---
- name: Generate device configuration in structured format
  arista.avd.yaml_templates_to_facts:
    root_key: structured_config
    templates:
      - python_module: "ansible_collections.arista.avd.roles.eos_designs.python_modules.base"
        python_class_name: "AvdStructuredConfig"
      - template: "mlag/main.j2"
      - template: "designs/underlay/main.j2"
      - template: "designs/overlay/main.j2"
      - template: "l3_edge/main.j2"
      - template: "designs/network_services/main.j2"
      - template: "connected_endpoints/main.j2"
      - template: "custom-structured-configuration-from-var.j2"
        options:
          list_merge: "{{ custom_structured_configuration_list_merge }}"
          strip_empty_keys: false
    schema_id: eos_designs
    output_schema_id: eos_cli_config_gen
  check_mode: no
  changed_when: False
```

## Authors

- EMEA AS Team (@aristanetworks)
