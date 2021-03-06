# elastic-searcher

Searches via EQL (Event Query Language) and KQL in an elasticsearch instance to find documents with an advanced syntax.

Elastic Syntax: https://www.elastic.co/guide/en/elasticsearch/reference/current/eql-syntax.html

Syntax: https://readthedocs.org/projects/eql/downloads/pdf/latest/

Definition of EQL
> EQL is a language that can match events, generate sequences, stack data, build aggregations, and perform analysis.
> EQL is schemaless and supports multiple database backends. It supports field lookups, boolean logic, comparisons,
> wildcard matching, and function calls. EQL also has a preprocessor that can perform parse and translation time
> evaluation, allowing for easily sharable components between queries.

# Prerequisites

```
pip install elasticsearch 
```

# Usage

## Columns Output

```bash
python elastic-searcher.py --size 3 --columns 'process.name, process.command_line, process.parent.name, host.name' eql '
process where process.name == "sh"
'
```

```
process.name,process.command_line,process.parent.name,host.name
sh,/bin/sh -c iptables -w -I f2b-sshd 1 -s 127.0.0.1 -j REJECT --reject-with icmp-port-unreachable,python3.8,localhost
sh,/bin/sh -c iptables -w -I f2b-sshd 1 -s 127.0.0.1 -j REJECT --reject-with icmp-port-unreachable,sh,localhost
sh,/bin/sh -c iptables -w -I f2b-sshd 1 -s 127.0.0.1 -j REJECT --reject-with icmp-port-unreachable,python3.8,localhost
```

## Json Output

To get the raw json output omit the `--columns` parameter.

```bash
python elastic-searcher.py eql '
process where process.name == "sh"
'
```

## Searching with KQL

To use Kibanas KQL search you can use the subroutine "kql" like this:

```bash
python eql_search.py --columns "destination.ip" kql 'destination.ip:*'
```

This query would generate a list of destination ips for you.
