# Ndarray Data Language

Files with multidimensional array data (also referred as [ndarray](https://docs.scipy.org/doc/numpy/reference/arrays.ndarray.html) or [datacube](https://en.wikipedia.org/wiki/Data_cube)) are commonplace in science, engineering, and machine learning. They are typically in various binary formats because that is the most efficient storage method for array data. One important aspect of the workflow is describing file content in some textual form that is easy for users to understand, create, or share. This requires special software tools and many of these file formats have at least one. Their output text formats are not standardized and vary in the file content detail but usually are directly related to the underlying storage format.

The Ndarray Data Language aims to provide a common text format for describing the content of multidimensional array data files. The language's main features are:

* Modelled as a simple abstraction of the [HDF5](https://portal.hdfgroup.org/display/HDF5/HDF5) and [netCDF](https://www.unidata.ucar.edu/software/netcdf/) formats.

* Simple and clean syntax based on [YAML](http://yaml.org/spec/1.2/spec.html). Parseable by all major programming languages, yet easy for users to understand and create in their favorite text editor.

* Easily extendable to accomodate additional information for a particular file format.

## License

Released under [the MIT license](LICENSE).
