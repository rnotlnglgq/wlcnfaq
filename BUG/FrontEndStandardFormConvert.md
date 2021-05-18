# FrontEnd BUG Report

## Description

`InputForm` code

```
"a;(**)
b"
```
will be converted to following `StandardForm` boxes incorrectly
```
RowBox[{"a", ";", RowBox[{"(*", "*)"}], "\n", "b"}]]
```
instead of following expected boxes:
```
BoxData[{RowBox[{"a", ";", RowBox[{"(*", "*)"}]}], "\n", "b"}]
```
by the `FrontEndToken` `"SelectionDisplayAs"->StandardForm`.

The same behavior can be produced by the `FrontEndToken` `Paste`.

## Importance

When `b` is an expression includes `Out`, the result won't be acceptable.

## Tested

rnotlnglgq: Wolfram Mathematica 12.2 Windows

Other discussers: Wolfram Mathematica 4 11.3 12.0