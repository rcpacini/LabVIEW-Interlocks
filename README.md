# LabVIEW-Interlocks

LabVIEW library to enable or disable controls based on one or more expressions.

## Getting Started

Clone the repository and run the `/Examples/Interlocks Example.vi` to demostrate how interlocks enable or disable controls according to the expressions defined in an `Interlocks.ini` configuration file.

![Example](/Images/Example.png)

![Example](/Images/Example-Diagram.png)

## Usage

Interlock expressions are used to enable or disable controls based on one or more conditions, useful for operator interfaces that need to limit operator actions. The interlock expressions for each control reference is loaded from an `Interlocks.ini` configuration file (loaded once at initialization) formatted as:

```
[<variable_name>]
<expressions>
```

Where `<variable_name>` is the section name (i.e. control label without special characters: `[A-Za-z0-9_][A-Za-z0-9_]*`) and the `<expression>` is python-like statements which can span multiple lines.

If the `<expression` evaluates to `True` (i.e. != 0 or is empty), the control is `Enabled`.

Otherwise, the control is `Disabled and Grayed Out`.

Prior to expression evaluation, all control values are read to be used as identifiers within other expressions (useful for cascading conditions), eliminating a lot of value change event checks.

Expressions can be nested using parentheses `(a or b) and (c or d)`.

Expressions can use pythonic nomenclature `(a and b or not c)` or C++ nomenclature `(a && b || !c)`.

### Expressions supported:

```
AND:          and | && | &
OR:           or | '||' | '|'
NOT:          not | !
EQUAL:        ==
NOT EQUAL:    !=
LESS THAN:    <
GREATER THAN: >
LTE:          <=
GTE:          >=
PARENTHESES:  (..)
LITERAL:      Boolean | DBL | I32 | U32
IDENTIFIER:   [A-Za-z_][A-Za-z0-9_]*
```

_Note: Bit-wise and assignment operations are not supported._

### Example Interlock.ini

```
[V1]
not V2 and (V4 or V5)

[V2]
not V1 and (V4 or V5)

[V3]
V1 or V2

[MFC1]
V3 and (V1 or V2)

[V4]
not (V1 or V2) and not V5 or MFC1 > 0.5

[V5]
not (V1 or V2) and not V4 or MFC1 > 0.5
```

## Support

Open a ticket to bug fixes or feature requests.
