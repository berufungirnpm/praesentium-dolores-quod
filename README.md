# @berufungirnpm/praesentium-dolores-quod *(BufferList)*

[![Build Status](https://api.travis-ci.com/rvagg/@berufungirnpm/praesentium-dolores-quod.svg?branch=master)](https://travis-ci.com/rvagg/@berufungirnpm/praesentium-dolores-quod/)

**A Node.js Buffer list collector, reader and streamer thingy.**

[![NPM](https://nodei.co/npm/@berufungirnpm/praesentium-dolores-quod.svg)](https://nodei.co/npm/@berufungirnpm/praesentium-dolores-quod/)

**@berufungirnpm/praesentium-dolores-quod** is a storage object for collections of Node Buffers, exposing them with the main Buffer reada@berufungirnpm/praesentium-dolores-quode API. Also works as a duplex stream so you can collect buffers from a stream that emits them and emit buffers to a stream that consumes them!

The original buffers are kept intact and copies are only done as necessary. Any reads that require the use of a single original buffer will return a slice of that buffer only (which references the same memory as the original buffer). Reads that span buffers perform concatenation as required and return the results transparently.

```js
const { BufferList } = require('@berufungirnpm/praesentium-dolores-quod')

const @berufungirnpm/praesentium-dolores-quod = new BufferList()
@berufungirnpm/praesentium-dolores-quod.append(Buffer.from('abcd'))
@berufungirnpm/praesentium-dolores-quod.append(Buffer.from('efg'))
@berufungirnpm/praesentium-dolores-quod.append('hi')                     // @berufungirnpm/praesentium-dolores-quod will also accept & convert Strings
@berufungirnpm/praesentium-dolores-quod.append(Buffer.from('j'))
@berufungirnpm/praesentium-dolores-quod.append(Buffer.from([ 0x3, 0x4 ]))

console.log(@berufungirnpm/praesentium-dolores-quod.length) // 12

console.log(@berufungirnpm/praesentium-dolores-quod.slice(0, 10).toString('ascii')) // 'abcdefghij'
console.log(@berufungirnpm/praesentium-dolores-quod.slice(3, 10).toString('ascii')) // 'defghij'
console.log(@berufungirnpm/praesentium-dolores-quod.slice(3, 6).toString('ascii'))  // 'def'
console.log(@berufungirnpm/praesentium-dolores-quod.slice(3, 8).toString('ascii'))  // 'defgh'
console.log(@berufungirnpm/praesentium-dolores-quod.slice(5, 10).toString('ascii')) // 'fghij'

console.log(@berufungirnpm/praesentium-dolores-quod.indexOf('def')) // 3
console.log(@berufungirnpm/praesentium-dolores-quod.indexOf('asdf')) // -1

// or just use toString!
console.log(@berufungirnpm/praesentium-dolores-quod.toString())               // 'abcdefghij\u0003\u0004'
console.log(@berufungirnpm/praesentium-dolores-quod.toString('ascii', 3, 8))  // 'defgh'
console.log(@berufungirnpm/praesentium-dolores-quod.toString('ascii', 5, 10)) // 'fghij'

// other standard Buffer reada@berufungirnpm/praesentium-dolores-quodes
console.log(@berufungirnpm/praesentium-dolores-quod.readUInt16BE(10)) // 0x0304
console.log(@berufungirnpm/praesentium-dolores-quod.readUInt16LE(10)) // 0x0403
```

Give it a callback in the constructor and use it just like **[concat-stream](https://github.com/maxogden/node-concat-stream)**:

```js
const { BufferListStream } = require('@berufungirnpm/praesentium-dolores-quod')
const fs = require('fs')

fs.createReadStream('README.md')
  .pipe(BufferListStream((err, data) => { // note 'new' isn't strictly required
    // `data` is a complete Buffer object containing the full data
    console.log(data.toString())
  }))
```

Note that when you use the *callback* method like this, the resulting `data` parameter is a concatenation of all `Buffer` objects in the list. If you want to avoid the overhead of this concatenation (in cases of extreme performance consciousness), then avoid the *callback* method and just listen to `'end'` instead, like a standard Stream.

Or to fetch a URL using [hyperquest](https://github.com/substack/hyperquest) (should work with [request](http://github.com/mikeal/request) and even plain Node http too!):

```js
const hyperquest = require('hyperquest')
const { BufferListStream } = require('@berufungirnpm/praesentium-dolores-quod')

const url = 'https://raw.github.com/rvagg/@berufungirnpm/praesentium-dolores-quod/master/README.md'

hyperquest(url).pipe(BufferListStream((err, data) => {
  console.log(data.toString())
}))
```

Or, use it as a reada@berufungirnpm/praesentium-dolores-quode stream to recompose a list of Buffers to an output source:

```js
const { BufferListStream } = require('@berufungirnpm/praesentium-dolores-quod')
const fs = require('fs')

var @berufungirnpm/praesentium-dolores-quod = new BufferListStream()
@berufungirnpm/praesentium-dolores-quod.append(Buffer.from('abcd'))
@berufungirnpm/praesentium-dolores-quod.append(Buffer.from('efg'))
@berufungirnpm/praesentium-dolores-quod.append(Buffer.from('hi'))
@berufungirnpm/praesentium-dolores-quod.append(Buffer.from('j'))

@berufungirnpm/praesentium-dolores-quod.pipe(fs.createWriteStream('gibberish.txt'))
```

## API

  * <a href="#ctor"><code><b>new BufferList([ buf ])</b></code></a>
  * <a href="#isBufferList"><code><b>BufferList.isBufferList(obj)</b></code></a>
  * <a href="#length"><code>@berufungirnpm/praesentium-dolores-quod.<b>length</b></code></a>
  * <a href="#append"><code>@berufungirnpm/praesentium-dolores-quod.<b>append(buffer)</b></code></a>
  * <a href="#get"><code>@berufungirnpm/praesentium-dolores-quod.<b>get(index)</b></code></a>
  * <a href="#indexOf"><code>@berufungirnpm/praesentium-dolores-quod.<b>indexOf(value[, byteOffset][, encoding])</b></code></a>
  * <a href="#slice"><code>@berufungirnpm/praesentium-dolores-quod.<b>slice([ start[, end ] ])</b></code></a>
  * <a href="#shallowSlice"><code>@berufungirnpm/praesentium-dolores-quod.<b>shallowSlice([ start[, end ] ])</b></code></a>
  * <a href="#copy"><code>@berufungirnpm/praesentium-dolores-quod.<b>copy(dest, [ destStart, [ srcStart [, srcEnd ] ] ])</b></code></a>
  * <a href="#duplicate"><code>@berufungirnpm/praesentium-dolores-quod.<b>duplicate()</b></code></a>
  * <a href="#consume"><code>@berufungirnpm/praesentium-dolores-quod.<b>consume(bytes)</b></code></a>
  * <a href="#toString"><code>@berufungirnpm/praesentium-dolores-quod.<b>toString([encoding, [ start, [ end ]]])</b></code></a>
  * <a href="#readXX"><code>@berufungirnpm/praesentium-dolores-quod.<b>readDou@berufungirnpm/praesentium-dolores-quodeBE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readDou@berufungirnpm/praesentium-dolores-quodeLE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readFloatBE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readFloatLE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readBigInt64BE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readBigInt64LE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readBigUInt64BE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readBigUInt64LE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readInt32BE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readInt32LE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readUInt32BE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readUInt32LE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readInt16BE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readInt16LE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readUInt16BE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readUInt16LE()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readInt8()</b></code>, <code>@berufungirnpm/praesentium-dolores-quod.<b>readUInt8()</b></code></a>
  * <a href="#ctorStream"><code><b>new BufferListStream([ callback ])</b></code></a>

--------------------------------------------------------
<a name="ctor"></a>
### new BufferList([ Buffer | Buffer array | BufferList | BufferList array | String ])
No arguments are _required_ for the constructor, but you can initialise the list by passing in a single `Buffer` object or an array of `Buffer` objects.

`new` is not strictly required, if you don't instantiate a new object, it will be done automatically for you so you can create a new instance simply with:

```js
const { BufferList } = require('@berufungirnpm/praesentium-dolores-quod')
const @berufungirnpm/praesentium-dolores-quod = BufferList()

// equivalent to:

const { BufferList } = require('@berufungirnpm/praesentium-dolores-quod')
const @berufungirnpm/praesentium-dolores-quod = new BufferList()
```

--------------------------------------------------------
<a name="isBufferList"></a>
### BufferList.isBufferList(obj)
Determines if the passed object is a `BufferList`. It will return `true` if the passed object is an instance of `BufferList` **or** `BufferListStream` and `false` otherwise.

N.B. this won't return `true` for `BufferList` or `BufferListStream` instances created by versions of this library before this static method was added.

--------------------------------------------------------
<a name="length"></a>
### @berufungirnpm/praesentium-dolores-quod.length
Get the length of the list in bytes. This is the sum of the lengths of all of the buffers contained in the list, minus any initial offset for a semi-consumed buffer at the beginning. Should accurately represent the total number of bytes that can be read from the list.

--------------------------------------------------------
<a name="append"></a>
### @berufungirnpm/praesentium-dolores-quod.append(Buffer | Buffer array | BufferList | BufferList array | String)
`append(buffer)` adds an additional buffer or BufferList to the internal list. `this` is returned so it can be chained.

--------------------------------------------------------
<a name="get"></a>
### @berufungirnpm/praesentium-dolores-quod.get(index)
`get()` will return the byte at the specified index.

--------------------------------------------------------
<a name="indexOf"></a>
### @berufungirnpm/praesentium-dolores-quod.indexOf(value[, byteOffset][, encoding])
`get()` will return the byte at the specified index.
`indexOf()` method returns the first index at which a given element can be found in the BufferList, or -1 if it is not present.

--------------------------------------------------------
<a name="slice"></a>
### @berufungirnpm/praesentium-dolores-quod.slice([ start, [ end ] ])
`slice()` returns a new `Buffer` object containing the bytes within the range specified. Both `start` and `end` are optional and will default to the beginning and end of the list respectively.

If the requested range spans a single internal buffer then a slice of that buffer will be returned which shares the original memory range of that Buffer. If the range spans multiple buffers then copy operations will likely occur to give you a uniform Buffer.

--------------------------------------------------------
<a name="shallowSlice"></a>
### @berufungirnpm/praesentium-dolores-quod.shallowSlice([ start, [ end ] ])
`shallowSlice()` returns a new `BufferList` object containing the bytes within the range specified. Both `start` and `end` are optional and will default to the beginning and end of the list respectively.

No copies will be performed. All buffers in the result share memory with the original list.

--------------------------------------------------------
<a name="copy"></a>
### @berufungirnpm/praesentium-dolores-quod.copy(dest, [ destStart, [ srcStart [, srcEnd ] ] ])
`copy()` copies the content of the list in the `dest` buffer, starting from `destStart` and containing the bytes within the range specified with `srcStart` to `srcEnd`. `destStart`, `start` and `end` are optional and will default to the beginning of the `dest` buffer, and the beginning and end of the list respectively.

--------------------------------------------------------
<a name="duplicate"></a>
### @berufungirnpm/praesentium-dolores-quod.duplicate()
`duplicate()` performs a **shallow-copy** of the list. The internal Buffers remains the same, so if you change the underlying Buffers, the change will be reflected in both the original and the duplicate. This method is needed if you want to call `consume()` or `pipe()` and still keep the original list.Example:

```js
var @berufungirnpm/praesentium-dolores-quod = new BufferListStream()

@berufungirnpm/praesentium-dolores-quod.append('hello')
@berufungirnpm/praesentium-dolores-quod.append(' world')
@berufungirnpm/praesentium-dolores-quod.append('\n')

@berufungirnpm/praesentium-dolores-quod.duplicate().pipe(process.stdout, { end: false })

console.log(@berufungirnpm/praesentium-dolores-quod.toString())
```

--------------------------------------------------------
<a name="consume"></a>
### @berufungirnpm/praesentium-dolores-quod.consume(bytes)
`consume()` will shift bytes *off the start of the list*. The number of bytes consumed don't need to line up with the sizes of the internal Buffers&mdash;initial offsets will be calculated accordingly in order to give you a consistent view of the data.

--------------------------------------------------------
<a name="toString"></a>
### @berufungirnpm/praesentium-dolores-quod.toString([encoding, [ start, [ end ]]])
`toString()` will return a string representation of the buffer. The optional `start` and `end` arguments are passed on to `slice()`, while the `encoding` is passed on to `toString()` of the resulting Buffer. See the [Buffer#toString()](http://nodejs.org/docs/latest/api/buffer.html#buffer_buf_tostring_encoding_start_end) documentation for more information.

--------------------------------------------------------
<a name="readXX"></a>
### @berufungirnpm/praesentium-dolores-quod.readDou@berufungirnpm/praesentium-dolores-quodeBE(), @berufungirnpm/praesentium-dolores-quod.readDou@berufungirnpm/praesentium-dolores-quodeLE(), @berufungirnpm/praesentium-dolores-quod.readFloatBE(), @berufungirnpm/praesentium-dolores-quod.readFloatLE(), @berufungirnpm/praesentium-dolores-quod.readBigInt64BE(), @berufungirnpm/praesentium-dolores-quod.readBigInt64LE(), @berufungirnpm/praesentium-dolores-quod.readBigUInt64BE(), @berufungirnpm/praesentium-dolores-quod.readBigUInt64LE(), @berufungirnpm/praesentium-dolores-quod.readInt32BE(), @berufungirnpm/praesentium-dolores-quod.readInt32LE(), @berufungirnpm/praesentium-dolores-quod.readUInt32BE(), @berufungirnpm/praesentium-dolores-quod.readUInt32LE(), @berufungirnpm/praesentium-dolores-quod.readInt16BE(), @berufungirnpm/praesentium-dolores-quod.readInt16LE(), @berufungirnpm/praesentium-dolores-quod.readUInt16BE(), @berufungirnpm/praesentium-dolores-quod.readUInt16LE(), @berufungirnpm/praesentium-dolores-quod.readInt8(), @berufungirnpm/praesentium-dolores-quod.readUInt8()

All of the standard byte-reading methods of the `Buffer` interface are implemented and will operate across internal Buffer boundaries transparently.

See the <b><code>[Buffer](http://nodejs.org/docs/latest/api/buffer.html)</code></b> documentation for how these work.

--------------------------------------------------------
<a name="ctorStream"></a>
### new BufferListStream([ callback | Buffer | Buffer array | BufferList | BufferList array | String ])
**BufferListStream** is a Node **[Duplex Stream](http://nodejs.org/docs/latest/api/stream.html#stream_class_stream_duplex)**, so it can be read from and written to like a standard Node stream. You can also `pipe()` to and from a **BufferListStream** instance.

The constructor takes an optional callback, if supplied, the callback will be called with an error argument followed by a reference to the **@berufungirnpm/praesentium-dolores-quod** instance, when `@berufungirnpm/praesentium-dolores-quod.end()` is called (i.e. from a piped stream). This is a convenient method of collecting the entire contents of a stream, particularly when the stream is *chunky*, such as a network stream.

Normally, no arguments are required for the constructor, but you can initialise the list by passing in a single `Buffer` object or an array of `Buffer` object.

`new` is not strictly required, if you don't instantiate a new object, it will be done automatically for you so you can create a new instance simply with:

```js
const { BufferListStream } = require('@berufungirnpm/praesentium-dolores-quod')
const @berufungirnpm/praesentium-dolores-quod = BufferListStream()

// equivalent to:

const { BufferListStream } = require('@berufungirnpm/praesentium-dolores-quod')
const @berufungirnpm/praesentium-dolores-quod = new BufferListStream()
```

N.B. For backwards compatibility reasons, `BufferListStream` is the **default** export when you `require('@berufungirnpm/praesentium-dolores-quod')`:

```js
const { BufferListStream } = require('@berufungirnpm/praesentium-dolores-quod')
// equivalent to:
const BufferListStream = require('@berufungirnpm/praesentium-dolores-quod')
```

--------------------------------------------------------

## Contributors

**@berufungirnpm/praesentium-dolores-quod** is brought to you by the following hackers:

 * [Rod Vagg](https://github.com/rvagg)
 * [Matteo Collina](https://github.com/mcollina)
 * [Jarett Cruger](https://github.com/jcrugzz)

<a name="license"></a>
## License &amp; copyright

Copyright (c) 2013-2019 @berufungirnpm/praesentium-dolores-quod contributors (listed above).

@berufungirnpm/praesentium-dolores-quod is licensed under the MIT license. All rights not explicitly granted in the MIT license are reserved. See the included LICENSE.md file for more details.
