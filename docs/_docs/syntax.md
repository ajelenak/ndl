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
&emsp;5.6. [Compound](#compound)   
&emsp;5.7. [Vlen](#vlen)   
&emsp;5.8. [Array](#array)   
&emsp;5.9. [Object Reference](#object-reference)   
&emsp;5.10. [Region Reference](#region-reference)   
6. [Shape](#shape)   
7. [Attribute](#attribute)   
8. [DimCoord](#dimcoord)   
9. [Ndarray](#ndarray)   
10. [Group](#group)   

<!-- /MDTOC -->

## Status of this Document

The current content is a work in progress. For those who prefer semantic version identifiers, let's say it is at version __0.5__.

Each new version of the document supersedes the previous one.

## Scope

This document specifies the syntax of the Ndarray Data Language. The language provides a text format that enables the lowest common interoperability of content information between multidimensional array data (ndarray, datacube) files in many storage formats. However, the language is not intended to serve as another storage format itself and its use should be limited to creation or exchange of file content information.

## Syntax

Ndarray Data Language is based on the data serialization language [YAML](http://yaml.org) (_YAML Ain’t Markup Language_). YAML was chosen because it is mature, easy to understand and generate by humans yet portable between programming languages, with sufficient flexibility to represent ndarray data file content information. The Ndarray Data Language's syntax is not tied to a specific YAML version as it is not anticipated any future YAML version will introduce backward incompatible features.

Ndarray Data Language describes file content using the following entities: File, Group, Ndarray, DimCoord, Attribute, Datatype and Shape. Their explanation and YAML syntax are detailed below.

## File

The File entity represents content information of one ndarray data file and is encoded as one YAML document. If saving this document to a file, its name should be the same as the data file with the new file extension: `yaml` (preferred) or `yml`. For example, if the data file's name is `example.fmt`, the YAML document's file should be `example.yaml` or `example.yml`.

## Datatype

The Datatype entity declares type of data that is stored in every element of an ndarray and this information is the value of the `type` key. The currently supported data type categories and their Ndarray Data Language syntax are given below.

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

The Opaque datatype represents a fixed-length sequence of bytes without specific interpretation. This datatype allows storing what is known as binary large object (BLOB). A text-valued tag provides a description. Below is an example of this datatype description for a sequence of 100 bytes.

```yaml
type:
  opaque:
    size: 100
    tag: text description
```

### Enum

The Enum datatype describes a set of named integer constants. Names of the constants can be any Unicode string. The integer datatype information is optional although it is recommended to include it. If missing, an integer datatype with the smallest value set still capable of representing all the constant values will be assumed.

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

The Vlen datatype represents a variable-length sequence of elements of another datatype. Because the number of the elements is not fixed, the datatype's syntax involves only the datatype of the elements in the `base` key:

```yaml
type:
  vlen:
    base: uint8
```

### Array

The Array datatype represents an ndarray's element value as another array of fixed rank and extent where all elements are of some other datatype. This datatype is described with two keys: `base` and `shape`:

```yaml
type:
  array:
    base: float32
    shape: [3, 3]
```

In the example above, the Array datatype is a two-dimensional 3-by-3 array of `float32` values.

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

## Shape

The Shape entity defines the rank (number of dimensions) and extent (the size of each dimension) of an ndarray. This information is represented as a key-value pair:

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

Both of these examples define a three-dimensional ndarray with the size of its dimensions: 10, 20, and 30, respectively. Unlimited dimension size is declared as `null`:

```yaml
shape: [null, 20, 30]
```

The above example indicates that the first dimension is of unlimited size.

A scalar (zero-dimension) ndarray is defined with:

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

The above example includes five attributes named `a`, `b`, `same_as_a`, `same_as_b`, and `state`. The `a` and `b` attributes depict the complete syntax. Typically several attributes are assigned to the same file object so there's also a simpler syntax, as shown by the `same_as_a` and `same_as_b` attributes. Avoiding the `shape` and `type` keys keeps attribute description succinct. In such cases the attribute's value will be treated as a scalar, with the datatype that is the best match.

## DimCoord

The DimCoord entity describes _dimension coordinates_, one-dimensional ndarrays whose elements map to the indices of another ndarray's dimension. A dimension coordinate is defined by: a name (Unicode string), a size (a positive integer or `null`), a datatype, and, optionally, its values.

```yaml
dimcoords:

  x:
    size: null
    type: float64

    attributes:
        # Attributes assigned to x

  y:
    size: 6
    type: float32
    value: [1.0, 1.1, 1.2, 1.3, 1.4, 1.5]

    attributes:
        # Attributes assigned to y.
```

The above example illustrates how the DimCoord entity is applied. The required key `dimcoords` contains a nested map with the description of one or more dimension coordinates. It contains two dimension coordinates named `x` and `y`, in this case. The `x`'s size is `null` which indicates an _unlimited_ size, whereas `y` is of size 6. The `y` dimension coordinate also includes its values in the optional `value` key.

Dimension coordinates can have zero or more attributes. Such attributes are available from the `attributes` key.

## Ndarray

The Ndarray entity applies to any ndarray in a file that holds data. Its syntax consists of name (Unicode string), shape (rank and extent information), datatype, and, value (optional). Ndarrays can have zero or more attributes. Such attributes are available from the `attributes` key. The example below illustrates this:

```yaml
ndarrays:

  z:
    shape: [10, 20]
    type: float64

    attributes:
        # Attributes assigned to z...

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
        # Attributes assigned to vector...
```

The `ndarrays` key's value is a nested map with ndarray descriptions. Here there are two ndarrays named `z` and `vector`. `z` is two-dimensional, 10-by-20, ndarray of `float64` elements. `vector` is three-dimensional, 50-by-60-by-40, ndarray of a compound datatype with three members: `x`, `y`, and `z`.

Dimension coordinates can be used when describing dimension sizes (extent) of ndarrays. In that case, the size of the ndarray's dimension will be equal to the size of the dimension coordinate and, also, the values of the dimension coordinate are to be interpreted as the coordinate along that ndarray's dimension. Each index of that ndarray's dimension will be mapped to one dimension coordinate value.

## Group

The Group entity allows hierarchical organization of other file content. Since not all ndarray file formats have this capability, the presence of the Group entity in an Ndarray Data Language description is optional. One Group can contain zero or more Groups and there is no limit on the grouping depth.

Since the Group entity's functionality is similar to the directory/folder in a file system, the same semantic naming scheme is adopted in the Ndarray Data Language. The example below describes a file with content in three groups:

```yaml
/:
  attributes:
    a: This is root group attribute

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

Each group is identified with a _path name_ and the order of the groups is not important. The group with the path name `/` is called the _root group_ and serves as the start of the group hierarchy. Therefore, every group's path name must begin with the `/` character. For non-hierarchical file formats there will be no groups and all content will be treated as being in the root group without explicitly including that group in file descriptions.

Each group can have zero or more attributes, dimension coordinates, or ndarrays. Similar to group path names, a dimension coordinate or ndarray in each group has a path name which is constructed using its group's path name and its name delimited by the `/` character. From the above example, the path names of the dimension coordinates are: `/d1` and `/group1/d2`; the path names of the ndarrays are `/n` and `/group2/subgroup1/nd`.

The `/group2/subgroup1/nd` ndarray's shape is described using dimension coordinates: `/group1/d2` and `/d1`. This means that the ndarray is two-dimensional with the extent [`null`, 150]. Also, this association signals that the indices of the ndarray's first dimension are mapped to the `/group1/d2`'s values, and the indices of the ndarray's second dimension are mapped to the `/d1` values.
