---
layout: "post"
title: "Excel and averages"
date: "2021-05-20 09:35"
---
More of a mental note than anything else, while using built in functions (which I normally avoid) I noticed something I had not considered earlier. Depending on how one notes absence of values will influence statistical evaluation.

| #1 | #2 | #3 | #4 | SUM |  AVG   |
| :-:| :-:| :-:| :-:| :-: |  :-:   |
| 16 | 20 |  2 | 18 | 56  |   14   |
| 16 | 20 |  2 |  0 | 38  |   9,5  |
| 16 | 20 |  2 |    | 38  |  12,67 |
| 16 | 20 |  2 | -  | 38  |  12,67 |

Consider that using `=AVERAGE(AX:DX)` differentiates between "0", " " and "-". If any other notation than "0" is used to indicate absence of a value though for which an entry was still expected and should be included in subsequent calculations, then a modified calculation of "average" is needed:

`=(SUM(AX:DX)/SUM(COUNTA(AX:DX);COUNTBLANK(AX:DX)))`

Summing all values, ignoring empty or NAN, then counting the number of cells which contains numbers as well as cells which does not contain any numbers and divides the calculated sum of all numbers by this number of counted cells for a "new average". This works regardless if what is used for "expected but not present" values.

| #1 | #2 | #3 | #4 | SUM |  AVG   | NEW AVERAGE |
| :-:| :-:| :-:| :-:| :-: |  :-:   |     :-:     |
| 16 | 20 |  2 | 18 | 56  |   14   |     14      |
| 16 | 20 |  2 |  0 | 38  |   9,5  |     9,5     |
| 16 | 20 |  2 |    | 38  |  12,67 |     9,5     |
| 16 | 20 |  2 | -  | 38  |  12,67 |     9,5     |

This is a small problem and an obvious edge case as "not detected" could probably almost always be noted as "0". Still, worth remembering.
