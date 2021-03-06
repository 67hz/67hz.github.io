---
layout: post
title: fiddlin' bits
category: software
tags: [c++, c, bitwise ops]
---

Bits of bitwise magic:


1. Set the kth bit in n
```cpp
n = n | 1 << k;
```

1. Clear the kth bit in n
```cpp
n = n & 0 << k;
n = n & ~(1 << k); // same as above
n &= ~(1 << k);
```

1. Clear all bits in n
```cpp
n ^= n;
```

1. Toggle the kth bit in n
```cpp
n = n ^ 1 << k;
```

1. Erase the lowest set bit:
```cpp
x = x & (x - 1);
```

1. Isolate the lowest bit that is 1 in x
```cpp
x & ~(x - 1);
```
1. Identity - useful for extracting least significant bit
```cpp
x &= 1;
```
1. Base 2 exponentiation. - prevents ambiguity errors that might come up using `<cmath>` and `pow()`
```cpp
std::array<int>(pow(2, 64)); // ambiguous, first param expects a double
// avoid this and just use >>
std::array<int>(1 << 64);
```

1. Toggle last bit - useful in keeping a binary sum (e.g., parity count - even/odd count of 1s in binary)
```cpp
short result = 0;
while(x) {
    if(bitIsOne(x))
	result ^= 1;
	x &= (x - 1); // erase lowest set bit (see #4)
}
return result;
```
1. Modular division with power of 2 base (e.g. 77 mod 64)
```cpp
// x       = 77:01001101
// a       = 64:01000001
// subtract 1 from a - creates identity mask
// a - 1   = 76:00111111
// x & a-1 =   :00001101
//  result = 13 == 77 mod 64
///...
result = x & (x-1);
```
1. Right propogate after lowest set bit.
* Isolate the lowest set bit.
* Subtract 1 from lowest bit to get a mask (all 1s) of the right side of the lowest set bit.
* OR the original number against the mask to fill the right-side of the lowest set bit with 1's.
```cpp
// Example: 12
// x                 : 00011000
// (x & ~(x-1))      : 00001000
// (x & ~(x-1)) - 1  : 00000111
// x | (x & ~(x-1) -1: 00011111 
short rightPropogate(unsigned long long x)
{
    unsigned long long lowestBit = x & ~(x-1);
    lowestBit -= 1;
    x |= lowestBit;
    return x;
}
```
