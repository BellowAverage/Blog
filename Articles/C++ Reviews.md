# C++ Reviews

Created: September 15, 2024 12:26 PM

### Introduction

C++ is the very first language that I was exposed to when I was a second-year student in college. Later on, very few do I get chance to use this language, however. As a part of reviewing data structures and algorithm, I feel the necessity of recap basics of C++, as this particular language touches the essence of data structures.

### Learning Sources

https://www.youtube.com/watch?v=-TkoO8Z07hI

### English Naming Map

| Operator | English name | Description |
| --- | --- | --- |
| << | insertion operator |  |
| >> | extraction operator |  |
| :: | scope resolution operator |  |
| (a > b) ? a : b | ternary operator | a replacement of if statement |
|  | argument | goes into a function |
|  | parameter | preset by the function (type needs to be pre-defined) |
| 0x1A3F | hexadecimal |  |
| 6719 | decimal |  |
| & | address-of operator |  |
| * | dereference operator |  |
| . | class member access operator |  |

### Math Functions

https://cplusplus.com/reference/cmath/

### String Functions

https://cplusplus.com/reference/string/string/

### Pass Array Argument to Functions

- Array decays into a pointer.

### Passing Argument to Functions

- Normally the argument is passed in a way of copying value.
- Parameters set to receive address (`int &argument`) to handle argument in the level of actual storage.