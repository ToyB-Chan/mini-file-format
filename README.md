# mini-file-format

Minimalized ini format with concrete format rules. Meant to be easily read and easily parsed.
Ends with `.mini` i.e. `config.mini`.

## Sections and Keys

- Sections are denoted by square brackets (`[Section]`).
- Subsections are nested within sections using dot notation (`[Section.Subsection]`).
- All (sub-) sections must be explicitly defined before used elsewhere.
- All (sub-) sections may only be defined once.
- (Sub-) sections can be empty.
- Keys within (sub-) sections use `key = value` syntax and are seperated by new-lines.
- Key and (sub-) section names may only contain `a-z`, `A-Z`, `0-9`, and `_`.
- Values may never reach over multiple lines.
- Empty keys (i.e., keys without values) are not allowed.
- Whitespaces are ignored as long as they don't split up names or values.
- Whitespaces are not allowed during (sub-) section declaration.
- Empty lines are ignored.
- Comments are denoted by `#` and may only appear on separate lines.
- Cases are sensitive (for both names and values), except where otherwise noted.
- The underlying size of any type is application specific.
- The parsing behavior of an illformed file is undefined.

## Supported Data Types

The format supports the following data types:

### Integer
- Defined as a sequence of digits (`0-9`).
- May be suffixed with `h` to be interpreted as hexadecimal (`0-9`, `a-f`, `A-F`).
- May be suffixed with `b` to be interpreted as binary (`0-1`).
- May contain underscores (`_`) for better readability.
- Example: `value = 5`
- Example: `hexValue = FA8h`
- Example: `binValue = 0010010b`
- Example: `anotherDec = 1_000_375`

### String
- Strings are enclosed in double quotes (`""`).
- Strings support escape sequences using backslashes (`\`).
- Supported escape sequences:
  - `\"` for a double quote (`"`)
  - `\n` for a newline
  - `\t` for a tab
  - `\r` for carriage return
  - `\\` for a backslash
- Strings may be empty.
- Example: `string = "My string"`
- Example: `string = "Line 1\nLine 2"`
- Example: `string = "Tab\tSeparated"`
- Example: `string = "My \"escaped\" String"`
- Example: `string = "My string with a \\ <- backslash"`
- Example: `emptyString = ""`

### Array
- Arrays are enclosed in square brackets (`[]`) and contain comma-separated values.
- Arrays may contain whitespaces, as long as they don't split up values.
- Arrays may not end on a comma.
- Arrays may only contain one datatype at a time.
- Arrays may be of any positive dimension.
- Arrays may be of any positive length.
- Arrays may be empty.
- Example: `array = [5, 6, 10]`
- Example: `array2d = [[5, 8], [9, 7], [23, 47]]`
- Example: `array2d = [[9], [50, 3]]`
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

### Array of Subsections
Arrays of subsections are implicty realizable by naming subsections and do not need specific parsing rules.
We recommend using something alike the following structure:
```text
[Database]
Version = 1

[Database.Persons]
[Database.Persons.0]
Name = John Smith
Age = 36

[Database.Persons.1]
Name = Emily Johnson
Age = 24
```
As mentioned above, it is to be handled like any other subsection - that means not starting at index 0 or having discontinuous indicies do _not_ make the file illformed.

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
hexValue = FA8h

[MySection.MySubsection.AnotherSubsection]
binValue = 0010010b

# Another comment
anotherDec = 1_000_375
```

## What not to do
```text
[My-Section]                 | Invalid character in section name.
myFloat = 1.5                | Should always end with f.
myBool = True                | Should not be capitalized.
myArray = [5, "Hi"]          | Arrays may only contain one datatype at a time.
myArray2 = [[5, 6], 1]       | Dimensions must match across all arrays (technically same as above).
myString = 'Hello'           | Strings may only be encapsulated by ".
my-value = 5                 | Invalid character in key.

[MyOtherSection.Subsection]  | "MyOtherSection" has not been defined before.
myValue = 16 # a comment     | Inline comments are not allowed.
myArray = [5, 9,]            | Arrays may not end on a comma.
abc =                        | Keys may not contain empty values-

[ MyOtherSection]            | Whitespaces are not allowed during section declaration.
myArray = [                  | Values may not span over multiple lines.
            [5, 7],
            [9, 2]
          ]
```
