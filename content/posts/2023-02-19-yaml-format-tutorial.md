---
title: YAML format tutorial
date: 2023-02-19
---



# YAML

YAML (short for "YAML Ain't Markup Language") is a human-readable data serialization format that is commonly used for configuration file and data exchange between programming languages.

## Preface

If you learned about  `JSON`, reading `YAML versus JSON` part is recommended to understand about its usage quickly.

## YAML syntax

REF: https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html

All members of a list are lines beginning at the same indentation level starting with a `"- "` (a dash and a space):

```yaml
# A list of tasty fruits
- Apple
- Orange
- Strawberry
- Mango
...
```

A dictionary is represented in a simple `key: value` form (the colon must be followed by a space):

```yaml
# An employee record
martin:
  name: Martin D'vloper
  job: Developer
  skill: Elite
```

Ansible doesn’t really use these too much, but you can also specify a boolean value (true/false) in several forms:

```yaml
create_key: yes
needs_agent: no
knows_oop: True
likes_emacs: TRUE
uses_cvs: false
```

Values can span multiple lines using `|` or `>`. Spanning multiple lines using a “Literal Block Scalar” `|` will include the newlines and any trailing spaces. Using a “Folded Block Scalar” `>` will fold newlines to spaces; it’s used to make what would otherwise be a very long line easier to read and edit. In either case the indentation will be ignored. Examples are:

```yaml
include_newlines: |
            exactly as you see
            will appear these three
            lines of poetry

fold_newlines: >
            this is really a
            single line of text
            despite appearances
```

While in the above `>` example all newlines are folded into spaces, there are two ways to enforce a newline to be kept:

```yaml
fold_some_newlines: >
    a
    b

    c
    d
      e
    f
```

Alternatively, it can be enforced by including newline `\n` characters:

```yaml
fold_same_newlines: "a b\nc d\n  e\nf\n"
```

In addition to `'` and `"` there are a number of characters that are special (or reserved) and cannot be used as the first character of an unquoted scalar: `[] {} > | * & ! % # ` @ ,` .

You should also be aware of `? : -`. In YAML, they are allowed at the beginning of a string if a non-space character follows, but YAML processor implementations differ, so it’s better to use quotes.

## YAML versus JSON

[Convert YAML to JSON](http://nodeca.github.io/js-yaml/)

JSON:

```json
{
  models:[
    { 
    	model: "a"
    	type: "x"
    	#bunch of properties...
  	},
  	{
    	model: "b"
    	type: "y"
    	#bunch of properties...
  	}
 ]
}
```

Convert to YAML:

```yaml
models:
- model: "a"
	type: "x"
  #bunch of properties...
- model: "b"
  type: "y"
  #bunch of properties...
```

