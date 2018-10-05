---
title: HDF-EOS5 Example
permalink: /docs/examples/hdf-eos5/
---

[HDF-EOS5](https://earthdata.nasa.gov/user-resources/standards-and-references/hdf-eos5) is the standard for storing NASA Earth Observing System data. The example below is for the file [`GSSTF_NCEP.3.2008.12.31.he5`](https://measures.gesdisc.eosdis.nasa.gov/data/GSSTF/GSSTF_NCEP.3/2008/GSSTF_NCEP.3.2008.12.31.he5) from the [Goddard Satellite-based Surface Turbulent Fluxes Version 3](https://disc.gsfc.nasa.gov/datasets/GSSTF_NCEP_V3/summary?keywords=Atmospheric%20Composition) dataset.

HDF-EOS5 files can have many groups but in Ndarray Data Language only the groups with content need to be included. Another characteristic of HDF-EOS5 files are text-valued HDF5 datasets with metadata. The `/HDFEOS INFORMATION/StructMetadata.0` ndarray shows how the value of such HDF5 datasets is easy to represent using YAML's [block scalar](http://yaml.org/spec/1.2/spec.html#id2793652) literal style.

```yaml
/HDFEOS INFORMATION:
  attributes:
    HDFEOSVersion: HDFEOS_5.1.11

  ndarrays:
    StructMetadata.0:
      type: string
      shape: []
      value: |
        GROUP=SwathStructure
        END_GROUP=SwathStructure
        GROUP=GridStructure
            GROUP=GRID_1
                GridName="NCEP"
                XDim=1440
                YDim=720
                UpperLeftPointMtrs=(-180000000.000000,-90000000.000000)
                LowerRightMtrs=(180000000.000000,90000000.000000)
                Projection=HE5_GCTP_GEO
                GROUP=Dimension
                    OBJECT=Dimension_1
                        DimensionName="XDim"
                        Size=1440
                    END_OBJECT=Dimension_1
                    OBJECT=Dimension_2
                        DimensionName="YDim"
                        Size=720
                    END_OBJECT=Dimension_2
                END_GROUP=Dimension
                GROUP=DataField
                    OBJECT=DataField_1
                        DataFieldName="SST"
                        DataType=H5T_NATIVE_FLOAT
                        DimList=("YDim","XDim")
                        MaxdimList=("YDim","XDim")
                    END_OBJECT=DataField_1
                    OBJECT=DataField_2
                        DataFieldName="Psea_level"
                        DataType=H5T_NATIVE_FLOAT
                        DimList=("YDim","XDim")
                        MaxdimList=("YDim","XDim")
                    END_OBJECT=DataField_2
                    OBJECT=DataField_3
                        DataFieldName="Tair_2m"
                        DataType=H5T_NATIVE_FLOAT
                        DimList=("YDim","XDim")
                        MaxdimList=("YDim","XDim")
                    END_OBJECT=DataField_3
                    OBJECT=DataField_4
                        DataFieldName="Qsat"
                        DataType=H5T_NATIVE_FLOAT
                        DimList=("YDim","XDim")
                        MaxdimList=("YDim","XDim")
                    END_OBJECT=DataField_4
                END_GROUP=DataField
                GROUP=MergedFields
                END_GROUP=MergedFields
            END_GROUP=GRID_1
        END_GROUP=GridStructure
        GROUP=PointStructure
        END_GROUP=PointStructure
        GROUP=ZaStructure
        END_GROUP=ZaStructure
        END

/HDFEOS/ADDITIONAL/FILE_ATTRIBUTES:
  attributes:
    BeginDate: 2008-12-31
    CollectionDescription: NCEP/DOE Reanalysis II in HDF-EOS5, relevant to
                           GSSTF 0.25x0.25 degree Daily Grid, V3, (GSSTF_NCEP)
                           at GES DISC
    DOI: 10.5067/MEASURES/GSSTF/DATA302
    EndDate: 2009-01-01
    LongName: NCEP/DOE Reanalysis II, for GSSTF, Daily Grid
    ShortName: GSSTF_NCEP
    VersionID: 3

/HDFEOS/GRIDS/NCEP/Data Fields:
  ndarrays:
    Psea_level:
      type: float32
      shape: [720, 1440]

      attributes:
        _FillValue:
          type: float32
          shape: []
          value: -999
        long_name: sea level pressure
        units: hPa

    Qsat:
      type: float32
      shape: [720, 1440]

      attributes:
        _FillValue:
          type: float32
          shape: []
          value: -999
        long_name: sea surface saturation humidity
        units: g/kg

    SST:
      type: float32
      shape: [720, 1440]

      attributes:
        _FillValue:
          type: float32
          shape: []
          value: -999
        long_name: sea surface skin temperature
        units: C

    Tair_2m:
      type: float32
      shape: [720, 1440]

      attributes:
        _FillValue:
          type: float32
          shape: []
          value: -999
        long_name: 2m air temperature
        units: C
```
