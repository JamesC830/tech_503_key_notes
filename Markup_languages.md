# Markup languages notes

A markup language is a text-encoding system that specifies the structure and formatting of a document, and potentially the relationships between its parts.

## XML

### eXtensible Markup Language

e.g.

```
<table>
  <team>
    <name>Liverpool</name>
    <position>1</position>
    <points>70</points>
  </team>
  <team>
    <name>Arsenal</name>
    <position>2</position>
    <points>58</points>
  </team>
</table>
```

## JSON

### JavaScript Object Notation

e.g.

```
{
    "table": [
      {
        "team": "Liverpool",
        "position": 1,
        "points": 70
      },
      {
        "team": "Arsenal",
        "position": 2,
        "points": 58
      }
    ]
  }
```

## YAML

### Yet Another Markup Language

e.g.

```
table:
  - team: 
      name: "Liverpool"
      position: 1
      points: 70
  - team: 
      name: "Arsenal"
      position: 2
      points: 58