

# mini-file-format

Minimalized ini format with concrete format rules. Meant to be easily read and easily parsed.
Ends with `.mini` i.e. `config.mini`.

## Sections and Keys

- Sections are denoted by square brackets (`[Section]`).
- Subsections are nested within sections using dot notation (`[Section.Subsection]`).
- All (sub-) sections must be explicitly defined before used elsewhere.
- All (sub-) sections may only be defined once.
- (Sub-) sections can be empty.
- Keys within (sub-) sections use `key = value` syntax.
- Key and (sub-) section names may only contain `a-z`, `A-Z`, `0-9`, and `_`.
- Empty keys (i.e., keys without values) are not allowed.
- Spaces outside of strings are ignored.
- Empty lines are ignored
- Comments are denoted by `#` and may only appear on separate lines.

## Supported Data Types

The format supports the following data types:

### Integer
- Defined as a sequence of digits (`0-9`).
- Example: `value = 5`

### String
- Strings are enclosed in double quotes (`""`).
- Strings support escape sequences using backslashes (`\`).
- Supported escape sequences:
  - `\"` for a double quote (`"`)
  - `\n` for a newline
  - `\t` for a tab
  - `\\` for a backslash
- Strings can be empty.
- Example: `string = "My string`
- Example: `string = "Line 1\nLine 2"`
- Example: `string = "Tab\tSeparated"`
- Example: `string = "My \"escaped\" String"`
- Example: `emptyString = ""`

### Array
- Arrays are enclosed in square brackets (`[]`) and contain comma-separated values.
- Arrays may only contain one datatype.
- Arrays may be of any positive dimension.
- Arrays may be of any positive length.
- Arrays can be empty.
- Example: `array = [5, 6, 10]`
- Example: `array2d = [[5, 8], [9, 7], [23, 47]]`
- Example: `array2d = [[9], [50, 3], [54, 12, 46, 37]]`
- Example: `emptyArray = []`

### Boolean
- Boolean values are represented as either `true` or `false`.
- Example: `myBool = false`

### Floating-Point Numbers
- Floating-point numbers use `.` for decimal notation or `e` for scientific notation.
- Floats always end with `f`.
- Floats may also be whole numbers without a decimal point.
- Example: `anotherValue = 1.065f`
- Example: `thisToo = 1e18f`
- Example: `scientificFloat = 1.534e3f`
- Example: `wholeFloat = 1f`

### Hexadecimal Numbers
- Hexadecimal numbers are prefixed with `0x`.
- Example: `hexValue = 0xFA8`

### Binary Numbers
- Binary numbers are prefixed with `0b`.
- Example: `binValue = 0b0010010`

### Underscore in Numbers
- Numbers can contain underscores (`_`) for readability.
- Example: `anotherDec = 1_000_375`

## Example Structure
```text
[MySection]
myInteger = 5
myString = "My String"
myArray = [5, 6, 10]
myBool = false

# A comment
[MySection.MySubsection]
myFloat = 1.065f
myFloat2 = 1e18f
hexValue = 0xFA8

[MySection.MySubsection.AnotherSubsection]
binValue = 0b0010010

# Another comment
anotherDec = 1_000_375
```

