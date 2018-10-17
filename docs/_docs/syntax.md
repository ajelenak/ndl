---
title: Ndarray Data Language
permalink: /docs/syntax/
---
<!-- MDTOC maxdepth:6 firsth1:0 numbering:1 flatten:0 bullets:0 updateOnSave:0 -->

1. [Status of this Document](#status-of-this-document)   
2. [Scope](#scope)   
3. [Syntax](#syntax)   
4. [File](#file)   
5. [Datatype](#datatype)   
&emsp;5.1. [String](#string)   
&emsp;5.2. [Integer](#integer)   
&emsp;5.3. [Floating-point](#floating-point)   
&emsp;5.4. [Opaque](#opaque)   
&emsp;5.5. [Enum](#enum)   
&emsp;5.6. [Object Reference](#object-reference)   
&emsp;5.7. [Region Reference](#region-reference)   
&emsp;5.8. [Compound](#compound)   
&emsp;5.9. [Vlen](#vlen)   
&emsp;5.10. [Array](#array)   
6. [Shape](#shape)   
7. [Attribute](#attribute)   
8. [DimCoord](#dimcoord)   
9. [Ndarray](#ndarray)   
10. [Group](#group)   
11. [Storage Directives](#storage-directives)   

<!-- /MDTOC -->

## Status of this Document

The current content is a work in progress. For those who prefer semantic version identifiers, let's say it is at version __0.6__.

Each new version of the document supersedes the previous one.

## Scope

This document specifies the syntax of the Ndarray Data Language. The language provides a text format for describing the content of multidimensional array data (ndarray, data cube) files in many storage formats. However, the language is not intended to serve as another storage format so is best suited for creation and exchange of file content information.

## Syntax

Ndarray Data Language is based on the data serialization language [YAML](http://yaml.org) (_YAML Ain’t Markup Language_). YAML was chosen because it is mature, readable, easy to generate by humans yet portable between programming languages, with sufficient flexibility to represent ndarray file content information. The Ndarray Data Language's syntax is not tied to a specific YAML version as it is not anticipated any future YAML version will introduce backward incompatible features.

Ndarray Data Language describes file content using these seven entities: File, Group, Ndarray, DimCoord, Attribute, Datatype and Shape. Their explanation and YAML syntax are detailed below.

## File

The File entity represents content information of one ndarray file and is encoded as one YAML document. If saving this document to a file, its name should be the same as the ndarray file with a new file extension: `yaml` (preferred) or `yml`. For example, if a file's name is `example.fmt`, the YAML document's file should be `example.yaml` or `example.yml`.

## Datatype

The Datatype entity declares the type of data that other entities can hold and this information is available in the `type` key. The currently supported data type categories and their declaration syntax are in the following subsections.

### String

The String datatype is indicated with:

```yaml
type: string
```

Since YAML is a Unicode-based data language, the `string` datatype keyword always represents a sequence of Unicode characters (code points) regardless of the actual storage in an ndarray file.

### Integer

The accepted integer datatype keywords and the value sets they represent are listed in the table below:

| Keyword     | Type     | Value Set
| :-: | :- | :- |
| `int8` | 8-bit integer | -128 to 127 |
| `uint8` | unsigned 8-bit integer | 0 to 255 |
| `int16` | 16-bit integer | −32768 to 32767 |
| `uint16` | unsigned 16-bit integer | 0 to 65535 |
| `int32` | 32-bit integer | −2147483648 to 2147483647 |
| `uint32` | unsigned 32-bit integer | 0 to 4294967295 |
| `int64` | 64-bit integer | -9223372036854775808 to 9223372036854775807 |
| `uint64` | unsigned 64-bit integer | 0 to 18446744073709551615 |

### Floating-point

The supported floating-point datatype keywords are:

| Keyword | Description |
| :------------- | :------------- |
| `float32` | IEEE 754 single-precision (32 bit) floating-point |
| `float64` | IEEE 754 double-precision (64 bit) floating-point |

### Opaque

The Opaque datatype represents a fixed-length sequence of bytes without specific interpretation. This datatype allows storing what is known as binary large object (BLOB). An optional text-valued tag provides description. Below is an example for storing images in the PNG format. Maximum image size is 64,000 bytes and the MIME type for this image format is in the tag to help other applications interpret the bytes correctly.

```yaml
type:
  opaque:
    size: 64000
    tag: image/png
```

### Enum

The Enum datatype describes a set of named integer constants. Names of the constants can be any Unicode string. The integer datatype information is optional although it is recommended to include it. If missing, an integer datatype with the smallest value set still capable of representing all the constants will be assumed.

The example below shows an Enum datatype with three named constants, `UP`, `DOWN`, and `CENTER`, and their values. The `base` key holds the integer datatype.

```yaml
type:
  enum:
    base: int8
    members:
      UP: 0
      DOWN: 25
      CENTER: -120
```

This example illustrates an Enum datatype without the `base` key:

```yaml
type:
  enum:
    members:
      OFF: 0
      ON: 1
      UNDEFINED: 255
```

When processing this datatype description, the `uint8` will be assumed since its value set is enough to store all three constant values.

### Object Reference

This datatype is similar to pointers in some programming languages. Values point to other objects in the same file. Its declaration is:

```yaml
type: objref
```

### Region Reference

A value of the region reference datatype points to a selection of elements of one ndarray in the same file. There are two ways how ndarray elements can be selected: `block` and `element`.

`block` selections are contiguous subsets of ndarray's elements of the same rank as the ndarray. Each block is described with two ndarray elements at the diagonally opposite corners of that block. The block corner elements are described with tuples with their dimension indices.

`element` selections are collections of individual ndarray elements. Each ndarray element is specified using a tuple with its dimension indices.

The two syntax forms for this datatype are shown below:

```yaml
type:
  regref:
    selection: block

type:
  regref:
    selection: element
```

### Compound

This datatype represents a sequence of named members of other datatypes. For example:

```yaml
type:
  compound:
    - x: float32
    - y: int32
    - z: float64
```

describes a compound datatype with three members, named: `x`, `y`, and `z`, and their datatypes.

### Vlen

The Vlen datatype represents a variable-length sequence of elements of another datatype. Because the number of elements is not fixed, the datatype's syntax involves only the element datatype in the `base` key:

```yaml
type:
  vlen:
    base: uint8
```

The above declares a variable-length sequence of `unit8` values.

### Array

The Array datatype represents an element value as another array of fixed rank and extent where all elements are of some other datatype. This datatype is described with two keys: `base` and `shape`:

```yaml
type:
  array:
    base: float32
    shape: [3, 3]
```

In this example the Array datatype is a two-dimensional 3-by-3 array of `float32` values.

## Shape

The Shape entity defines the rank (number of dimensions) and extent (the size of each dimension) of an ndarray. This information is provided in the `shape` key:

```yaml
shape: [10, 20, 30]
```

or alternatively:

```yaml
shape:
  - 10
  - 20
  - 30
```

Both of these examples define a three-dimensional ndarray with the size of its dimensions: 10, 20, and 30, respectively.

Unlimited dimension size is declared as `null`:

```yaml
shape: [null, 20, 30]
```

The above example indicates that the first dimension is of unlimited size.

A scalar (zero-dimension) ndarray is declared with:

```yaml
shape: []
```

## Attribute

Some ndarray file formats support assigning properties to other objects in the same file. These properties usually provide contextual information, known as [_metadata_](https://en.wikipedia.org/wiki/Metadata), for the rest of the file's data. They are represented with the Attribute entity which consists of name (Unicode string), shape, datatype, and value.

The required key `attributes` holds a nested map with one or more attributes:

```yaml
attributes:

  a:
    shape: []
    type: string
    value: Ηελλο ωορλδ

  b:
    shape: []
    type: int32
    value: 10

  same_as_a: Ηελλο ωορλδ

  same_as_b: 10

  state:
    shape: [3]
    type: string
    value: [power on, power off, error]
```

The above example includes five attributes named `a`, `b`, `same_as_a`, `same_as_b`, and `state`. The `a` and `b` attributes depict the complete syntax. Typically several attributes are assigned to the same file object so there's also a simpler syntax, as shown by the `same_as_a` and `same_as_b` attributes. Avoiding the `shape` and `type` keys keeps attribute description succinct. In such cases attribute's value will be treated as a scalar of the datatype that is the best match. The `state` attribute is an example when the short form cannot apply because it is not scalar-valued.

## DimCoord

The DimCoord entity describes _dimension coordinates_, one-dimensional ndarrays whose elements are mapped to the indices of another ndarray's dimension. A dimension coordinate is defined by: name (Unicode string), size (a positive integer or `null`), datatype, and, optionally, its values (optional).

```yaml
dimcoords:

  x:
    size: null
    type: float64

    attributes:
      what: x coordinate

  y:
    size: 6
    type: float32
    value: [1.0, 1.1, 1.2, 1.3, 1.4, 1.5]

    attributes:
      what: y coordinate
```

The above example illustrates how the DimCoord entity is applied. The required key `dimcoords` contains a nested map with the description of one or more dimension coordinates. Here, it contains two dimension coordinates named `x` and `y`. The `x`'s size is `null` which indicates an _unlimited_ size, whereas `y` is of size 6. The `y` dimension coordinate also includes its values in the optional `value` key.

Dimension coordinates can have zero or more attributes, declared with the `attributes` key.

## Ndarray

The Ndarray entity applies to any ndarray in a file that holds data. Its syntax consists of name (Unicode string), shape (rank and extent information), datatype, and, value (optional). Ndarrays can have zero or more attributes. The example below illustrates all this:

```yaml
ndarrays:

  z:
    shape: [10, 20]
    type: float64

    attributes:
      description: Values of z

  vector:
    shape:
      - 50
      - 60
      - 40
    type:
      compound:
        - x: float32
        - y: float32
        - z: float32

    attributes:
        description: velocity
```

The `ndarrays` key holds a nested map with ndarray descriptions. In this example there are two ndarrays named `z` and `vector`. `z` is two-dimensional, 10-by-20, ndarray of `float64` elements. `vector` is three-dimensional, 50-by-60-by-40, ndarray of a compound datatype with three members: `x`, `y`, and `z`.

Dimension coordinates can be used when describing dimension sizes (extent) of ndarrays. In such cases the size of the ndarray's dimension will be equal to the size of the dimension coordinate and, also, the dimension coordinate's values are to be interpreted as the coordinates along that ndarray's dimension. Each index of that ndarray's dimension is mapped to one dimension coordinate value.

## Group

The Group entity allows hierarchical organization of other file content. Its use is optional because not all ndarray file formats have this capability. One Group can contain zero or more Groups and there is no limit on the grouping depth.

Since the Group entity's functionality is similar to the directory/folder in a file system, the same semantic naming scheme is adopted in the Ndarray Data Language. The example below describes a file with content in three groups:

```yaml
/:
  attributes:
    a: This is / group attribute

  dimcoords:
    d1:
      size: 150
      type: float32

  ndarrays:
    n:
      shape: [1967, 45]
      type: float64

/group1:
  attributes:
    a: This is /group1 attribute

  dimcoords:
    d2:
      size: null
      type: float32

/group2/subgroup1:
  attributes:
    c: This is /group2/subgroup1 attribute

  ndarrays:
    nd:
      shape:
        - /group1/d2
        - /d1
```

Each group is identified with a _path name_. The group with the path name `/` is called _root group_ and is the start of the group hierarchy. Every other group's path name must begin with the `/` character. Order of the groups is not important. For non-hierarchical file formats there will be no groups and all content will be treated as being in the root group without the requirement to explicitly state this.

Each group can have zero or more attributes, dimension coordinates, or ndarrays. Similar to group path names, every dimension coordinate and ndarray in a group has a path name which is constructed using the group's path name and the dimcoord/ndarray name delimited by the `/` character. In the above example, the dimension coordinate path names are: `/d1` and `/group1/d2`; the ndarray path names are `/n` and `/group2/subgroup1/nd`.

The `/group2/subgroup1/nd` ndarray's shape is described using dimension coordinates: `/group1/d2` and `/d1`. This means that the ndarray is two-dimensional with the extent [`null`, 150]. Also, this association signals that the indices of the ndarray's first dimension are mapped to the `/group1/d2`'s values, and the indices of the ndarray's second dimension are mapped to the `/d1` values.

## Storage Directives

These directives provide specific storage details of dimension coordinate, ndarray, or attribute values. They are optional because support vary among different file formats. There are no default values for any directive when not given. All directives are located in a nested map of the `storage` key.

Currently supported directives are explained below.

**`shape`**: Only allowed for ndarrays. A list specifies the actual extent of stored values. Number of its elements must equal ndarray's rank, with each element's value lesser or equal to ndarray's corresponding dimension size.

**`size`**: Only allowed for dimension coordinates. The actual size (number) of stored values.

**`chunk`**: Some file formats support storing ndarray values in a number of _chunks_ (byte streams). They are also called _tiles_. Chunk size is defined with a list of the same size as ndarray's rank where each value declares the number of ndarray elements along the corresponding dimension.

**`filter`**: Describes data processing pipeline as a list where each element is one process. The processes apply sequentially to outbound (write) data and in reverse (backward) order to inbound (read) data.

**`endian`**: Byte layout order for numbers. Two valid values: `little`, `big`.

**`charset`**: String character set.

**`fillvalue`**: The default value of ndarray or dimension coordinate elements in absence of actual data.
