# SYNOPSIS
A Buffer class.


# USAGE
This module is designed to work with the [`datcxx`][0] build tool. To add this
module to your project us the following command...

```bash
build add datcxx/cxx-buffer
```


# TEST

```bash
build test
```


# EXAMPLE

```c++
Buffer<uint8_t> a(128);
Buffer<uint8_t> b = { 0x00, 0xFF };

auto c = a.concat(b);
```

# API


## CONSTRUCTOR
The `Buffer` constructor overloads `operator<<` so that it can be easily
piped to `std` streams ie `std::cout`.

```c++
Buffer<T> b;
```

```c++
Buffer<T> b(size_t size);
```

```c++
Buffer<T> b = { 0x00, 0x00 };
```

```c++
Buffer<T> b({ 0x00, 0x00 });
```

```c++
Buffer<T> b(anotherBuffer);
```


## METHODS


### length()
Returns an `integer`, the amount of memory allocated for buf in bytes. Note that
this does not necessarily reflect the amount of "usable" data within buf.

#### EXAMPLE
```c++
Buffer<uint8_t> b = { 0x00, 0x00 };
b.length(); // 2
```

### copy(target[, targetStart[, sourceStart[, sourceEnd]]]
Returns an `integer`, the number of bytes copied. Ba

- `Buffer<T>` _target_ A Buffer to copy into.

- `size_t` _targetStart = 0_ The offset within target at which to begin writing.

- `size_t` _sourceStart = 0_ The offset within buf from which to begin copying.

- `size_t` _sourceEnd = buf.length()_ The offset within buf at which to stop
copying (not inclusive).


#### EXAMPLE
```c++
Buffer<uint8_t> a({ 01, 02, 03, 04, 05, 06, 07, 0xFF });
Buffer<uint8_t> b({ 00, 00, 00, 00, 00, 00 });
a.copy(b, 3, 2);

// a == <Buffer 01 02 03 04 05 06 07 ff>
// b == <Buffer 00 00 00 03 04 05>
```

### concat([buffer[, ...]])
Returns a new Buffer, the result of concatenating a list of Buffer instances.


#### EXAMPLE
```c++
Buffer<uint8_t> a({ 0x01, 0x02 });
Buffer<uint8_t> b({ 0x03, 0x04 });
Buffer<uint8_t> c({ 0x05, 0x06 });

auto d = a.concat({ b, c });

// d == <Buffer 01 02 03 04 05 06>
```

### equals(buffer)
To compare two buffers you can just use the `==` and `!=` operators.

#### EXAMPLE
```c++
Buffer<uint8_t> b({ 0x03, 0x04 });
Buffer<uint8_t> c({ 0x05, 0x06 });

b != c; // true
```


### slice(start[, end])
Returns a new instance of Buffer that references the same memory as the
original, but offset and cropped by the start and end indices.

- `size_t` _start = 0_ Where the new Buffer will start.
- `size_t` _end = buf.length()_ <integer> Where the new Buffer will end (not 
inclusive).

#### EXAMPLE
```c++
Buffer<uint8_t> b({ 0x01, 0x02, 0x03, 0x04 });

auto slice = b.slice(2, 4); // <Buffer 03 04>
```

[0]:https://github.com/datcxx/build
