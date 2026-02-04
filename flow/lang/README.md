Definitions

? Model
## Operation

## Syntax

## Quoting

- avoid at all cost.


3.1.2.4 ANSI-C Quoting

Character sequences of the form $'string' are treated as a special kind of single quotes. The sequence expands to string, with backslash-escaped characters in string replaced as specified by the ANSI C standard. Backslash escape sequences, if present, are decoded as follows:

| \a | alert |
| \b | backspace |
| \e \E | An escape character (not in ANSI C). 
| \f | form feed |
| \n | newline |
| \r | carriage return 
\t

    horizontal tab 
\v

    vertical tab 
\\

    backslash 
\'

    single quote 
\"

    double quote 
\?

    question mark 
\nnn

    The eight-bit character whose value is the octal value nnn (one to three octal digits). 
\xHH

    The eight-bit character whose value is the hexadecimal value HH (one or two hex digits). 
\uHHHH

    The Unicode (ISO/IEC 10646) character whose value is the hexadecimal value HHHH (one to four hex digits). 
\UHHHHHHHH

    The Unicode (ISO/IEC 10646) character whose value is the hexadecimal value HHHHHHHH (one to eight hex digits). 
\cx

    A control-x character. 

The expanded result is single-quoted, as if the dollar sign had not been present. 

