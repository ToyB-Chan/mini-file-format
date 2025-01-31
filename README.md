

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
- Can be prefixed with `0x` to be interpreted as hexadecimal (`0-9`, `a-f`, `A-F`).
- Can be prefixed with `0b` to be interpreted as binary (`0-1`).
- Can contain underscores (`_`) for better readability.
- Example: `value = 5`
- Example: `hexValue = 0xFA8`
- Example: `binValue = 0b0010010`
- Example: `anotherDec = 1_000_375`

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
- Example: `string = "My string with a \\ <- backslash"`
- Example: `emptyString = ""`

### Array
- Arrays are enclosed in square brackets (`[]`) and contain comma-separated values.
- Arrays may only contain one datatype at a time.
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
- Floating-point numbers use `.` for decimal notation or `e`/`E` for scientific notation.
- Floats always end with `f`.
- Floats may also be whole numbers without a decimal point.
- Example: `anotherValue = 1.065f`
- Example: `thisToo = 1e18f`
- Example: `scientificFloat = 1.534E3f`
- Example: `wholeFloat = 1f`
- Example: `wholeFloat = 5.f`

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

## What not to do
```text
[My-Section]                 | Invalid character in section name.
myFloat = 1.5                | Does not end with f.
myBool = True                | Should not be capitalized.
myArray = [5, "Hi"]          | Arrays can only contain one datatype at a time.
myArray2 = [[5, 6], 1]       | Technically same as above, dimensions must match across all arrays.
myString = 'Hello'           | Strings may only be encapsulated by ".
my-value = 5                 | Invalid character in key.

[MyOtherSection.Subsection]  | "MyOtherSection" is not defined
myValue = 16 # comment       | Inline comments are not allowed.
```
