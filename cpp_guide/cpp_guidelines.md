<h1>C++ Programming and Style Guidelines</h1>

# 1 Preface

## 1.1 References

These guidelines are loosely based on the [C++ Core Guidelines](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md), [C++ FAQ](https://isocpp.org/wiki/faq/), [Bjarne Stroustrup's C++ Style and Technique FAQ](http://www.stroustrup.com/bs_faq2.html) and the [PPP Style Guide](http://www.stroustrup.com/Programming/PPP-style.pdf) (PDF). These documents are recommended reading and should be considered for anything not specifically mentioned here.

# 2 Naming conventions

## 2.1 General

* Don't use `camelCase` or `PascalCase`, use `snake_case` as done in Standard C++ and the Standard Library
* Don't add type information to names, e.g. Hungarian notation `f_velocity` [<sub><sup>*(NL.5)*</sup></sub>](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#nl5-avoid-encoding-type-information-in-names)
* Don't use a leading underscore `_abc` or double underscores `ab__c`. These are generally reserved for library implementers or compiler vendors. <sub><sup>*(N4659 5.10.3)*</sup></sub>
* Avoid using unnecessarily or excessively long names

## 2.2 Variables

* Variable names should be written using only lower case characters, numbers and underscores to separate words

```C++
int received_bytes;  // Yes
int receivedBytes;  // No
int ReCe1ved_ByT3s;  // You're fired!

std::string html_header;  // This also applies to acronyms
std::string HTML_header;  // No
```

* Variable names should clearly reflect the content of the variable
* Don't remove vowels or needlessly shorten words, e.g. `rcvd_bytes`
* Class member variables should be suffixed with an underscore

```C++
class Vehicle
{
  ...
  float velocity_;
  float heading_;
};
```

* The length of a name should be roughly proportional to the size of its scope, i.e. use `n` instead of `received_bytes` for short-lived stack variables, especially if content can be inferred by context [<sub><sup>*(NL.7)*</sup></sub>](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#nl7-make-the-length-of-a-name-roughly-proportional-to-the-length-of-its-scope)

```C++
if (receiver.wait_for_data()) {
  int n = receiver.bytes_available();
  if (n > 512)
    process_bytes(buffer, n);
}
```

## 2.3 Functions

* Function names should be written using only lower case characters, numbers and underscores to separate words

```C++
void send_message();  // Yes
void sendmessage();  // No
void sendMessage();  // No
```

## 2.4 Types

* User defined types should start with a single capital letter followed by only lower case characters, numbers and underscores to separate words (Stroustrup style)

```C++
// Yes
class Fusion_object;
{
  ...
};

// No
class FusionObject;
{
  ...
};
```

## 2.5 Constants

* Don't capitalize all constants, e.g. `MAX_ITERATIONS` (exceptions are global constants, plain enumerations and reprocessor defines)
* Prefer using `enum class` since these don't leak names into the surrounding scope

```C++
enum COLOR { RED, GREEN, BLUE };  // OK
enum class Color { Red, Green, Blue };  // Better
```

## 2.6 Library usage

* The rules in this guide should be followed independent of the frameworks and libraries used, i.e. do not adapt, or partly adapt, to the conventions of a specific library

```C++
class TreeModel : public QAbstractItemModel  // No
class Tree_model : public QAbstractItemModel  // Yes

class file_error : public std::runtime_error  // No
class File_error : public std::runtime_error  // Yes
```

## 2.7 Files

* Header files should use `.h` and source files `.cpp`
* File names should be written only using lower case characters, numbers and underscores to separate words
* Files definining types should have the same name as the type

Example: `fusion_object.h`

```C++
class Fusion_object
{
  ...
};
```

# 3 Style conventions

## 3.1 Indentation

* Tabs should be automatically replaced with spaces
* One tab should be two spaces wide

```C++
// Yes
if (x < 0) {
  x = 0;
}

// No
if (x < 0) {
    x = 0;
}
```

## 3.2 Line length

* Lines should not exceed 120 characters
* Long lines should be broken up and continued on a new line with 4 spaces indentation with respect to the first line, i.e. any additional lines should not be indented further

```C++
// Yes
auto time = to_timestamp(int hours, int minutes, int seconds, int milliseconds,
    int microseconds, int nanoseconds, int picoseconds);

// Yes
auto time = to_timestamp(int hours, int minutes, int seconds, int milliseconds,
    int microseconds, int nanoseconds, int picoseconds, int femtoseconds,
    int attoseconds, int zeptoseconds, int yoctoseconds);

// No
auto time = to_timestamp(int hours, int minutes, int seconds, int milliseconds,
                         int microseconds, int nanoseconds, int picoseconds);  
```

## 3.3 Functions

* Don't put a space before or after the parentheses
* The opening brace should be put on a new line
* Parameters should be separated by a single space after the comma

```C++
void run(int mode, float delta_time)
{
  int x;
}
```

## 3.4 Types

* The opening brace should be put on a new line
* Access specifiers should not be indented

```C++
class Vehicle
{
public:
  void start();

private:
  float velocity_;
};
```

## 3.5 Conditionals

* The opening brace should be put at the end of the line
* Put a space between the keyword and the opening parenthesis
* Don't put spaces after the opening parenthesis or before the closing parenthesis

```C++
// Yes
if (i < 10) {
  ...
}
else {
  ...
}

// No
if( i < 10 )
{
  ...
}
```

* Don't write Yoda contitions

```C++
if (5 == i) {  // No
  ...
}
```

* Code within cases should be indented
* Prefer using braces for complex cases
* Denote intentional fallthrough with a comment or the C++17 `[[fallthrough]]` attribute

```C++
switch (condition)
{
  case 0:
    parse_signal(s);
    break;
  case 1:
    ...
    // Fallthrough or...
    [[fallthrough]];  // C++17
  case 2: {
    ...
  } break;
  default:
    break;
}
```

## 3.6 Exception handling

* The opening brace should be put at the end of the line

```C++
try {
  ...
}
catch (const std::excetion& e) {
  ...
}
```

## 3.7 Pointer declaration

* The * should be put close to the type

```C++
int* p;  // Yes
int *p;  // No
const int* const p;  // Yes
```

## 3.8 Const notation

* The const qualifier should be put before the type [<sub><sup>*(NL.26)*</sup></sub>](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#nl26-use-conventional-const-notation)

```C++
const int i = 3;  // Yes
int const i = 3;  // No
```

## 3.9 Getter & setter

* Getter should not be prefixed and setter should be prefixed with `set_`

```C++
class Vehicle
{
public:
  float velocity() const { return velocity_; };
  void set_velocity(float v) { velocity_ = v; };

private:
  float velocity_;
};
```

## 3.10 Comments

* Comments must be written in English
* Prefer succinct single-line comments in the imperative mood
* Capitalize the first letter and do not end the comment with a period (but use proper punctuation in long multi-line comments)
* Comments should focus on why, not what or how you are doing it

```C++
// Defer update of tree items to reduce CPU load
...
```

* Comments following code should be separated by 2 spaces

```C++
int apples;  // Number of bananas
```

* Keep comments sparse; prefer writing clear and expressive code
* Don't use C-style comments `/* Comment */`

## 3.11 Whitespace

* Don't paint "pretty" pictures with whitespace

```C++
// No
int i         = 0;
std::string s = "Maintaining pointless alignment isn't fun";
double d      = 13.37;

// Yes
int i = 0;
std::string s = "Each line is an individual, yay";
double d = 13.37;

// No, don't build staircases
auto time = to_timestamp(hours,
                         minutes,
                         seconds,
                         milliseconds,
                         to_microseconds(microseconds,
                                         nanoseconds,
                                         picoseconds));

// Yes
auto time = to_timestamp(hours, minutes, seconds, milliseconds,
    to_microseconds(microseconds, nanoseconds, picoseconds));
```

* Exception: Do properly align vector and matrix initialization

```C++
Matrix3f m;
m << 1,  2,  3,
     4,  5,  6,
     9, 10, 11;
```

# 4 Programming conventions

## 4.1 Header includes

* Explicitly include all headers you use
* Prefer including headers in the source file instead of the header file
* Prefer forward declaration whenever possible
* Always include the C++ version, e.g. include `<cmath>` and not `<math.h>`
* Include headers in the following order:
1. Current source file's header
2. C++ Standard libraries
3. C++ libraries
4. Project header files

Example: `vehicle.cpp`

```C++
#include "vehicle.h"

#include <tuple>
#include <vector>

#include "openssl/ssl.h"
#include <fmt/format.h>
#include "pugixml.hpp"

#include "header_from_your_project.h"
```

## 4.2 Include guards

* Add include guards to each header file
* The name must be the same as the file name and be written using only capital letters

```C++
#ifndef CAN_SOCKET_H
#define CAN_SOCKET_H
   ...
#endif  // CAN_SOCKET_H
```

## 4.3 Preprocessor directives

* Avoid macro definitions and prefer C++ constants and constexpr whenever possible

```C++
#define TAU 6.28318f;  // No
const float TAU = 6.28318f;  // Yes
```

* Macros should be written using only capital letters, numbers and underscores

## 4.4 Namespaces

* Never import a namespace in a header file or at global scope
* Always explicitly qualifiy the std namespace, e.g. `std::vector` (This applies to all types and functions included from C++ headers, e.g. `std::memcpy`, `std::abs` or `std::uint64_t`)
* Prefer declaring namespaces aliases in source files

```C++
using namespace std;  // No, never
using namespace asio::ip::udp;  // Allowed at function scope
namespace fsys = std::experimental::filesystem;  // Allowed in source files
```

* Put related classes into a common namespace

## 4.5 Const correctness

* Pointers and references should be const whenever possible
* Always mark a member function `const` when it doesn't modify any member variables

```C++
class Vehicle
{
public:
  float velocity() const { return velocity_; };

private:
  float velocity_;
};
```

## 4.6 Fixed-width integers

* Use the integer types defined in `<cstdint>` when a fixed-width integer is necessary

```C++
std::uint8_t crc;  // Yes
unsigned char crc;  // No
```

## 4.7 Structs

* Only use structs for [aggregates and POD types](https://stackoverflow.com/questions/4178175/what-are-aggregates-and-pods-and-how-why-are-they-special), in short, don't do anything fancy with structs
  * No user-defined constructor
  * No virtual functions

## 4.8 Classes

* A class should follow the single responsibility principle
* All resources acquired by a class must be released by the class's destructor [<sub><sup>*(C.31)*</sup></sub>](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#c31-all-resources-acquired-by-a-class-must-be-released-by-the-classs-destructor)
  * Prefer wrapping resources in their own [RAII](#412-resource-management) class, e.g. `std::unique_ptr`, rather than defining a destructor in the owning class
* A class with either a user-defined destructor, copy/move constructors or assignment operators must always define all five ([rule of five](http://en.cppreference.com/w/cpp/language/rule_of_three))

```C++
class Vehicle
{
public:
  ~Vehicle() { /* Resource cleanup */ }
  Vehicle(const Vehicle&) { /* Copy implementation */ }
  Vehicle(Vehicle&&) noexcept { /* Move implementation */ }
  Vehicle& operator=(const Vehicle&) = delete;  // Explicitly deleting counts as definition here
  Vehicle& operator=(Vehicle&&) noexcept = default;  // Same for requesting the default implementation

private:
  // Some resource, e.g. buffer, socket, file handle, etc.
};
```
* Declare move constructors `noexcept` to allow standard containers to internally move objects
* Declare single-argument constructors `explicit` unless implicit conversion is intended [<sub><sup>*(C.46)*</sup></sub>](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#c46-by-default-declare-single-argument-constructors-explicit)
* A class without resource responsibility should not define those member functions ([rule of zero](http://en.cppreference.com/w/cpp/language/rule_of_three))

## 4.9 Inheritance

* Always explicitly specify the inheritance type
* Always mark classes not intended to be derived from as `final`
* Always mark functions overwriting virtual functions with `overwrite`

```C++
class Shape
{
public:
  virtual void render() = 0;
};
 
class Cube final : public Shape
{
public:
  void render() override;
};
```

* Avoid resource by polymorphic base class pointers, when necessary make sure the base class's destructor is declared virtual

## 4.10 Functions

* Functions should serve a single purpose
* A single function should not exceed 100 lines; aim to write concise functions below 30 lines
* Inputs should be passed by constant references or pointers, or by value for primitive types [<sub><sup>*(F.16)*</sup></sub>](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#f16-for-in-parameters-pass-cheaply-copied-types-by-value-and-others-by-reference-to-const)
* Outputs should be returned [<sub><sup>*(F.20)*</sup></sub>](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#f20-for-out-output-values-prefer-return-values-to-output-parameters) [<sub><sup>*(F.21)*</sup></sub>](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#f21-to-return-multiple-out-values-prefer-returning-a-tuple-or-struct)

## 4.11 Resource management

* A resources should be tied to an object lifetime, i.e. resource acquisition is initialization [(RAII)](http://en.cppreference.com/w/cpp/language/raii)
* Follow the the Core Guidelines on [resource management](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#r-resource-management)
* In short:
  * Raw pointers should never have ownership responsibility
  * Prefer `std::unqiue_ptr` over `std::shared_ptr`
  * Wrap owning raw pointers from a C-style API in a `std::unique_ptr` with a custom deleter

## 4.12 Error handling

* Exceptions should be used [<sub><sup>*(C++ FAQ)*</sup></sub>](https://isocpp.org/wiki/faq/exceptions)
* Exceptions should be derived from `std::runtime_error` or `std::logic_error`

```C++
class Parse_error : public std::runtime_error
{
public:
  explicit Parse_error(const std::string& s) : std::runtime_error{s} {}
  explicit Parse_error(const char* s) : std::runtime_error{s} {}
};
```
