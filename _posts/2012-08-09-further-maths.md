---
layout: post
category: help
tags: [ manual, gertrude, maths ]
---
{% include JB/setup %}

Want to know more about the maths gertrude can do?

## Representing numbers

Gertrude understands integer literals, like `1234`, and also floating point literals like `123.456` or scientific-notation floating point literals like `1.234e3`. 
She'll have a reasonable stab at parsing English number names like `one hudred and thirty eight` or even `one nine eight four` or `twenty twelve`, but there's an element of 
ambiguity in these representations so she may not always understand you.

Gertrude even understands quite a lot of slang terms, so if you've ever wondered what `a bernie, two archers, three monkeys and a pony??` amounts to, then gertrude can tell you.

### Is a billion 1,000,000,000 or 1,000,000,000,000?

Gertrude can handle numbers in either the former (known as the _short scale_) or the latter (known as the _long scale_), but the system she uses is decided on a per-channel basis (for #ant.org, it's the Short Scale). There's currently no method to change the calculation scale on the fly.

## Representing operations

Operations can be represented using their C-style names, e.g. `+`, `-`, `sqrt()` etc. A few operations can also
be invoked by writing their names long-hand:

*   *plus*, *and* 
*   *minus*, *less*
*   *times*
*   *divided (by)*, *over*
*   *to the power of*
*   *to the first|second|third|fourth power *
*   *squared*
*   *cubed*
*   *square root of*
*   *cubed root of*
*   *% of*, *percent of*, *percent*
*   *+VAT*, *-VAT* (VAT is currently fixed at 20%)

## Functions

A number of C-style functions are available:

*    *sqrt()*
*    *cbrt()*
*    *sin()*, *asin()*, *sinh()*, *asinh()*
*    *cos()*, *acos()*, *cosh()*, *acosh()*
*    *tan()*, *atan()*, *tanh()*, *atanh()*
*    *exp()*, *log()*, *log2()*, *log10()*

Some functions have a more Rubyish representation (for obvious reasons):

*   *abs* eg `-123.abs`
*   *floor* eg `123.456.floor`
*   *ceil* eg `123.456.ceil`
*   etc

pretty much all the functions from Ruby's Numeric class are available.

## Angle units

Gertrude works internally in radians, but you can convert between radians and degrees with:

*    *rad2deg()*
*    *deg2rad()*

## Working in different bases

Gertrude understands decimal literals, octal literals that start with a zero (eg `0640`), hexadecimal literals
that start with '0x' or '0X' (eg `0xbeef`) or binary literals that start with '0b' (eg `0b1010101010`). 

If you want to work in another base, you can use the 'base()' function; eg to represent the base 3 number '12211' use `base(3, 12211)`.

If you want to convert the result of your entire expression to a different base, enclose the whole thing in a 
'base' function call, eg `base(2, 0xc8 & 0x55)`.

Hex, octal and binary outputs are so common that they have their own functions, `hex()`, `bin()` and `oct()` respectively, so the above example could be replaced with `bin(0cx8 & 0x55)`.

## Physical and mathematical constants

Gertrude has a number of built-in physical and mathematical constants; these always have an upper-case name:

*   *PI*
*   *E*
*   *C* (speed of light in a vaccuum)
*   *G* (gravitational constant)
*   *H* (Planck constant)
*   *L* (Avogadro's number)
*   *F* (Faraday's constant)
*   *R* (gas constant)
*   *GEE* (acceleration under earth gravity)

## Getting the results of previous evaluations

Every time gertrude successfully carries out a calculation, she stores the result in a special variable called
`@ans`. You can use this in subsequent expressions to substitute the previous result. Some other gertrude
plugins (eg the [currency convertor](/currency_convertor) plugin) can also write to `@ans`. Its value is shared
amongst all users of the channel, so it's possible to get very odd results if two people are using Gertrude's
maths plugin simultaneously.

## Storing results in variables

You can store intermediate results in variables, and start a new expression using a semicolon, 
eg `a = 4; b = 3; a*b`. 

Variables must start with a lower-case letter, and note that unlike `@ans`, variables are cleared out
between each expression, so you can't for exaple use variables from another user's expressions (although
this may change in the future).

## Programmable calculator

If you want to do more elaborate calculations, you can use full Ruby syntax, provided it all fits in one
line. For example to sum the numbers 1 to 10 `1.upto(10).reduce(:+)`.

If your program takes too long to execute, gertrude will abort it.

