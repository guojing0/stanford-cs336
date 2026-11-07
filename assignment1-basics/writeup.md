## Problem (unicode1): Understanding Unicode (1 point)

> (a) What Unicode character does chr(0) return?

U+0000, null character

> (b) How does this character’s string representation (__repr__()) differ from its printed representation?

The string representation is  '\x00', whereas the printed representation is nothing, since it's the null character.

> (c) What happens when this character occurs in text?

When it's in `repr()` form, it reads "\x00", but when it's printed with `print` function, it has no visible glyph.

## Problem (unicode2): Unicode Encodings (3 points)

> (a) What are some reasons to prefer training our tokenizer on UTF-8 encoded bytes, rather than UTF-16 or UTF-32? It may be helpful to compare the output of these encodings for various input strings.

Consider the following example:

```python
In [34]: love_you = "I love you 我爱你"

In [35]: love_you_8 = love_you.encode("utf-8")
In [36]: love_you_16 = love_you.encode("utf-16")
In [37]: love_you_32 = love_you.encode("utf-32")

In [38]: love_you_8
Out[38]: b'I love you \xe6\x88\x91\xe7\x88\xb1\xe4\xbd\xa0'
In [39]: love_you_16
Out[39]: b'\xff\xfeI\x00 \x00l\x00o\x00v\x00e\x00 \x00y\x00o\x00u\x00 \x00\x11b1r`O'
In [40]: love_you_32
Out[40]: b'\xff\xfe\x00\x00I\x00\x00\x00 \x00\x00\x00l\x00\x00\x00o\x00\x00\x00v\x00\x00\x00e\x00\x00\x00 \x00\x00\x00y\x00\x00\x00o\x00\x00\x00u\x00\x00\x00 \x00\x00\x00\x11b\x00\x001r\x00\x00`O\x00\x00'
```

When it's encoded with UTF-8, the English/Latin text is still recognizable, and much shorter (even compared to UTF-16 encoding). Also, in the case of `love_you_32`, there are multiple occurences of `\x00`, even if they should represent different texts or characters.

> (b) Consider the following (incorrect) function, which is intended to decode a UTF-8 byte string into a Unicode string. Why is this function incorrect? Provide an example of an input byte string that yields incorrect results.

```python
In [48]: "爱".encode("utf-8")
Out[48]: b'\xe7\x88\xb1'

In [49]: decode_utf8_bytes_to_str_wrong(b'\xe7\x88\xb1')
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xe7 in position 0: unexpected end of data
```

The function is incorrect because it does not consider that for non-English/Latex characters (e.g., Chinese), one Unicode code point may be multiple bytes, like in this case of love / "爱".

(by GPT: It decodes each byte separately, even though the three bytes in your example encode a single code point.)

> (c) Give a two-byte sequence that does not decode to any Unicode character(s).

```python
In [67]: b'\xc0\xc1'.decode("utf-8")
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xc0 in position 0: invalid start byte
```

Because the bytes `0xC0` and `0xC1` are not defined in UTF-8 encoding.
