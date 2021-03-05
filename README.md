# StructConverter
Python 2/3 &amp; C# data buffer converter , Network programming and programming language translation will use. And the implementation below C#.
## 在Python3中存在这两个方法，但是在Python2中并没有这两个方法，于是我按照自己的理解去在Python2下编写了这两个方法，可以替代Python3的这两个方法在Python2下使用。只写了小端序的，大端序未写。以及在C#下面的实现。
## There are these two methods in Python3, but there are no two methods in Python2, so I wrote these two methods in Python2 according to my own understanding, which can replace these two methods in Python3 and use them in Python2. Use all in little-endian order.
### classmethod int.from_bytes(bytes, byteorder, *, signed=False)
Return the integer represented by the given array of bytes.
```
>>>
int.from_bytes(b'\x00\x10', byteorder='big')
16
int.from_bytes(b'\x00\x10', byteorder='little')
4096
int.from_bytes(b'\xfc\x00', byteorder='big', signed=True)
-1024
int.from_bytes(b'\xfc\x00', byteorder='big', signed=False)
64512
int.from_bytes([255, 0, 0], byteorder='big')
16711680
```
The argument bytes must either be a bytes-like object or an iterable producing bytes.

The byteorder argument determines the byte order used to represent the integer. If byteorder is "big", the most significant byte is at the beginning of the byte array. If byteorder is "little", the most significant byte is at the end of the byte array. To request the native byte order of the host system, use sys.byteorder as the byte order value.

The signed argument indicates whether two’s complement is used to represent the integer.

New in version 3.2.


### int.to_bytes(length, byteorder, *, signed=False)
Return an array of bytes representing an integer.
```
>>>
(1024).to_bytes(2, byteorder='big')
b'\x04\x00'
(1024).to_bytes(10, byteorder='big')
b'\x00\x00\x00\x00\x00\x00\x00\x00\x04\x00'
(-1024).to_bytes(10, byteorder='big', signed=True)
b'\xff\xff\xff\xff\xff\xff\xff\xff\xfc\x00'
x = 1000
x.to_bytes((x.bit_length() + 7) // 8, byteorder='little')
b'\xe8\x03'
```
The integer is represented using length bytes. An OverflowError is raised if the integer is not representable with the given number of bytes.

The byteorder argument determines the byte order used to represent the integer. If byteorder is "big", the most significant byte is at the beginning of the byte array. If byteorder is "little", the most significant byte is at the end of the byte array. To request the native byte order of the host system, use sys.byteorder as the byte order value.

The signed argument determines whether two’s complement is used to represent the integer. If signed is False and a negative integer is given, an OverflowError is raised. The default value for signed is False.

New in version 3.2.

## Python2 Method :
```
def to_bytes(n, length):
    if type(n) == str:
        h = '%x' % ord(n)
    else:
        h = '%x' % n
    # h = '%x' % n
    s = ('0' * (len(h) % 2) + h).zfill(length * 2).decode('hex')
    return s[::-1]


def from_bytes(n):
    return int(n[::-1].encode("hex"), 16)
```
## C# Method :
int.from_bytes(bytes, byteorder, *, signed=False) => StructConverter.cs (From_Bytes fuction)

int.to_bytes(length, byteorder, *, signed=False) => StructConverter.cs (To_Bytes fuction)
## Example
Python3:
```
Python 3.7.5 (tags/v3.7.5:5c02a39a0b, Oct 15 2019, 00:11:34) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> c = 9999999
>>> c.to_bytes(4, byteorder='little')
b'\x7f\x96\x98\x00'
>>>
```
![image](https://user-images.githubusercontent.com/37355028/110065243-b08e6880-7da9-11eb-8d56-0dc7130e11fd.png)

Python2:
```
Python 2.7.18 (v2.7.18:8d21aa21f2, Apr 20 2020, 13:25:05) [MSC v.1500 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> def to_bytes(n, length):
...     if type(n) == str:
...         h = '%x' % ord(n)
...     else:
...         h = '%x' % n
...     # h = '%x' % n
...     s = ('0' * (len(h) % 2) + h).zfill(length * 2).decode('hex')
...     return s[::-1]
...
>>> to_bytes(9999999,4)
'\x7f\x96\x98\x00'
>>>
```
![image](https://user-images.githubusercontent.com/37355028/110065260-bbe19400-7da9-11eb-8691-e308b61ec5b8.png)

C#:

![image](https://user-images.githubusercontent.com/37355028/110065996-73c37100-7dab-11eb-984d-81565b640ec2.png)





