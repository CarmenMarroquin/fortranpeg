[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](https://github.com/ECYS-FIUSAC/fortranpeg/actions)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue)](https://opensource.org/licenses/MIT)
![Language: PEG](https://img.shields.io/badge/Language-PEG-blue)
![Language: Fortran](https://img.shields.io/badge/Language-Fortran-blue)
![Contributors](https://img.shields.io/github/contributors/ECYS-FIUSAC/fortranpeg)

# FortranPEG

FortranPEG is an online parser generator that uses a PEG grammar and generates a Fortran module with a recursive descent parser.

### [Try FortranPEG online!](https://ecys-fiusac.github.io/fortranpeg/)


## Usage guide


First, you have to write the grammar of expressions, for example, a simple calculator:

```
s = e

e = t "+" e
    / t "-" e
    / t

t = f "*" t
    / f "/" t
    /f

f = "(" e ")"
    / _ [0..9]+ _

_ = [ \t\n\r]*
```

![Alt text](https://github.com/ECYS-FIUSAC/fortranpeg/blob/main/img/screenshot1.png?raw=true "execute")

Next, semantic actions can be added to the end of each rule in Fortran code within brackets.

To access the values ​​in the body of the rule, a label can be defined, for example, in the rule: 

```
f = _ [0..9]+ _ 
```

It could be: 

```
f = _ num:[0..9]+ _
```

Where "num" will be automatically declared in the rule function.

By default, these labels are declared as strings, it is the user's responsibility to return a desired type.

>[!IMPORTANT]
>By convention, in each rule function a value and return type can be defined with the name "res".

For example, to return an integer from the rule:

```
f = _ num:[0..9]+ _
```

it would be: 

```
f = _ num:[0-9]+ _ {
    integer :: res
    read(num, *) res
}
```

The complete grammar can be found at this link:

[grammar.peg](https://github.com/ECYS-FIUSAC/fortranpeg/blob/main/test/test1/grammar.peg)


Now that you have the grammar, use the online generator to download the parser. It will be downloaded as parser.f90.

![Alt text](https://github.com/ECYS-FIUSAC/fortranpeg/blob/main/img/screenshot2.png?raw=true "execute")


To use the parser, you will need a Fortran program that will make calls to the parse(input) function with the string to be evaluated.

Here you can find an example program for testing:

[test.f90](https://github.com/ECYS-FIUSAC/fortranpeg/blob/main/test/test1/test.f90)

To start using the program, it must be compiled as follows:

```
gfortran -c parser.f90
gfortran -c test.f90
gfortran parser.o test.o -o test
./test
```

>[!TIP]
>Alternatively, you can use a bash with these lines: \# bash [exec](https://github.com/ECYS-FIUSAC/fortranpeg/blob/main/test/test1/exec)

Congratulations :clap:, you can now use the built program:

![Alt text](https://github.com/ECYS-FIUSAC/fortranpeg/blob/main/test/test1/screenshot.png?raw=true "execute")




