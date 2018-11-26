---
title: CF Files
permalink: /docs/examples/cf/
sectionid: examples
---

In [geosciences](https://en.wikipedia.org/wiki/Earth_science), specifically, the climate and weather modeling communities, ndarray data is often stored in files according to the [Climate and Forecast metadata convention](http://cfconventions.org) (CF). This page showcases examples of how various types of CF-encoded geoscience data are described using the Ndarray Data Language. The content in these examples should not be construed as complete or representing best practices.

## Grid

The most common type is so-called _gridded data_ which are represented as ndarrays where each of their dimensions is an independent physical quantity. In the example below, the gridded data is in a four-dimensional ndarray named `geoparam`. The `geoparam`'s dimension coordinates are `t`, `z`, `lat`, and `lon`, in that order.

```yaml
attributes:
  Conventions: CF-1.7

dimcoords:

  lat:
    type: float32
    size: 180

    attributes:
      long_name: grid cell latitude
      standard_name: latitude
      units: degrees_north
      axis: Y
      valid_min:
          value: -90
          type: float32
          shape: []
      valid_max:
          value: 90
          type: float32
          shape: []

  lon:
    type: float32
    size: 360

    attributes:
      long_name: grid cell longitude
      standard_name: longitude
      units: degrees_east
      axis: X
      valid_min:
          value: -180
          type: float32
          shape: []
      valid_max:
          value: 180
          type: float32
          shape: []

  z:
    type: float32
    size: 25

    attributes:
      long_name: grid cell altitude
      standard_name: altitude
      units: km
      axis: Z
      positive: up
      valid_min:
          value: 0
          type: float32
          shape: []
      valid_max:
          value: 25
          type: float32
          shape: []

  t:
    type: float64
    size: 72

    attributes:
      long_name: grid cell time
      standard_name: time
      units: hours since 1970-01-01T00:00:00Z
      calendar: gregorian
      axis: T
      valid_min:
          value: 0
          type: float64
          shape: []
      valid_max:
          value: 72
          type: float64
          shape: []

ndarrays:

  geoparam:
    type: float32
    shape: [t, z, lat, lon]

    attributes:
      long_name: air temperature
      standard_name: air_temperature
      _FillValue:
        type: float32
        shape: []
        value: -9999
      units: K
      valid_min:
          value: 200
          type: float32
          shape: []
      valid_max:
          value: 320
          type: float32
          shape: []
```
