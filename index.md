---
title: jq Command Line Tool Cheatsheet
---

# jq Command Line Tool Cheatsheet

## Basic Syntax
```bash
jq 'filter' input.json
```

## Basic Filters

### Identity Filter
```bash
. # Returns input unchanged
```

### Object Identifier-Index
```bash
.foo       # Access property "foo"
.foo.bar   # Access nested property
.["foo"]   # Access property with special characters
```

### Array Index
```bash
.[0]       # First element
.[-1]      # Last element
.[2:4]     # Array slice (elements 2 and 3)
```

## Array Operations

### Array Construction
```bash
[.foo,.bar]     # Create array from elements
[ .[] | .name ] # Transform array elements
```

### Array Iteration
```bash
.[]        # Output array elements one by one
.items[]   # Iterate over array in "items" property
```

## Combinators

### Pipe
```bash
. | .foo   # Pipe output of one filter to another
```

### Comma
```bash
.foo, .bar # Output both "foo" and "bar" properties
```

### Alternative
```bash
.foo // .bar  # Output foo if present, otherwise bar
```

## Built-in Operators & Functions

### Type Checking
```bash
type       # Output type of value
has("foo") # Check if object has property
```

### Numbers
```bash
length     # Length of string/array/object
add        # Sum numbers in array
max        # Maximum value in array
min        # Minimum value in array
```

### Strings
```bash
split(".")      # Split string by delimiter
join(" ")       # Join array with delimiter
startswith("x") # Check string prefix
endswith("x")   # Check string suffix
```

## Advanced Features

### Object Construction
```bash
{foo: .bar}     # Create new object
{(.key): .value} # Dynamic property names
```

### Conditionals
```bash
if . == 0 then "zero" else "nonzero" end
select(. > 2)   # Keep elements matching predicate
```

### Error Handling
```bash
try .a catch "error"  # Handle missing property
```

## Common Use Cases

### Format Output
```bash
jq -r        # Raw output (no quotes)
jq -c        # Compact output (one line)
jq --sort-keys # Sort object keys
```

### Transform Data
```bash
# Convert array of objects to object keyed by id
map({key: .id, value: .}) | from_entries

# Group array elements
group_by(.category)

# Flatten nested arrays
flatten

# Remove null values from objects
with_entries(select(.value != null))
```

### Complex Queries
```bash
# Find all unique values of a field
.[] | .field | unique

# Count occurrences of each value
group_by(.) | map({key: .[0], value: length}) | from_entries

# Filter nested data
.items[] | select(.type == "book" and .price < 10)
```

## Tips
- Use `-r` for raw output when dealing with strings
- Use `--arg name value` to pass variables from shell
- Use `-e` to set exit code on filter error
- Use `jq '.'` to pretty-print JSON
- Pipe multiple jq commands for complex transformations

Remember to quote your filter expressions when using jq on the command line!
