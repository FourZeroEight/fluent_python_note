# 4. Text versus Bytes

In this chapter we will visit the following topics:
* **characters**, **code points** and **byte** representations;
* unique features of binary sequences:  **bytes**,  **bytearray** and  **memoryview**;
* codecs for full Unicode and legacy character sets;
* avoiding and dealing with **encoding errors**;
* best practices when handling text files;
* the default encoding trap and standard I/O issues;
* safe Unicode text comparisons with normalization;
* utility functions for normalization, case folding and brute-force diacritic removal;
* proper sorting of Unicode text with  locale and the PyUCA library;
* character metadata in the Unicode database;
* dual-mode APIs that handle  str and  bytes;

## Character issues

> The concept of “string” is simple enough: a string is a sequence of characters. The problem lies in the definition of “character”.

* character 的識別是透過他的 code points(0 ~ 1,114,111),
* 在 Unicode 6.3 中（这是 Python 3.4 使用的标准），约 10% 的有效码位有对应的字符。

*  The actual bytes that represent a character depend on the encoding in use. An en‐
coding is an algorithm that converts code points to byte sequences and vice-versa.

``` python
# examples of encoding and decoding
>>> s = 'café'
>>> len(s)  # 
4
>>> b = s.encode('utf8')  # 
>>> b
b'caf\xc3\xa9'  # 
>1>> len(b)  # 
5
```

* encoding 就是, 從 str 轉換成 byte, 為了儲存或傳輸
* decoding 就是, 從 byte 轉換成 str, 為了人類可讀性

## Byte essentials

...

目前工作上用不到字串處理, 這章集節先擱著zzz