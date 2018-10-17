---
title: JPSS Example
permalink: /docs/examples/jpss/
---

Joint Polar Satellite System ([JPSS](https://www.jpss.noaa.gov)) is the current generation of operational polar-orbiting U.S. satellites for weather and environmental monitoring. The data is disseminated in an [HDF5](https://portal.hdfgroup.org/display/HDF5/HDF5)-based file [format](https://jointmission.gsfc.nasa.gov/documents.html). The example here is for an SVM01 product file from the [VIIRS](https://www.jpss.noaa.gov/viirs.html) instrument aboard the [SNPP](https://www.wmo-sat.info/oscar/satellites/view/342) satellite.

The JPSS file format relies on several advanced HDF5 features so the example below is an excellent showcase of Ndarray Data Language capabilities, such as:

* Object (`/Data_Products/VIIRS-M1-SDR/VIIRS-M1-SDR_Aggr` ndarray) and region  (`/Data_Products/VIIRS-M1-SDR/VIIRS-M1-SDR_Gran_0` ndarray) reference datatypes with values.

* [Storage directives]({{ site.baseurl }}{% link _docs/syntax.md %}#storage-directives) for ndarrays and attributes.

* Non-scalar attribute values.

```yaml
/:
  attributes:
    Distributor:
      type: string
      shape: [1,1]
      value: [[arch]]

    Mission_Name:
      type: string
      shape: [1,1]
      value: [[NPP]]

    N_Dataset_Source:
      type: string
      shape: [1,1]
      value: [[noaa]]

    N_GEO_Ref:
      type: string
      shape: [1,1]
      value: [[GMODO_npp_d20131114_t0010303_e0011545_b10607_c20131114064119981830_noaa_ops.h5]]

    N_HDF_Creation_Date:
      type: string
      shape: [1,1]
      value: [["20131114"]]

    N_HDF_Creation_Time:
      type: string
      shape: [1,1]
      value: [[064119.981830Z]]

    Platform_Short_Name:
      type: string
      shape: [1,1]
      value: [[NPP]]


/All_Data/VIIRS-M1-SDR_All:

  ndarrays:

    ModeGran:
      type: uint8
      shape: [null]
      storage:
        endian: little
        shape: [1]
        chunk: [1]
        fillvalue: 249

    ModeScan:
      type: uint8
      shape: [null]
      storage:
        endian: little
        shape: [48]
        chunk: [48]
        fillvalue: 249

    NumberOfBadChecksums:
      type: int32
      shape: [null]
      storage:
        endian: big
        shape: [48]
        chunk: [48]
        fillvalue: -993

    NumberOfDiscardedPkts:
      type: int32
      shape: [null]
      storage:
        endian: big
        shape: [48]
        chunk: [48]
        fillvalue: -993

    NumberOfMissingPkts:
      type: int32
      shape: [null]
      storage:
        endian: big
        shape: [48]
        chunk: [48]
        fillvalue: -993

    NumberOfScans:
      type: int32
      shape: [null]
      storage:
        endian: big
        shape: [1]
        chunk: [1]
        fillvalue: -993

    PadByte1:
      type: uint8
      shape: [null]
      storage:
        endian: little
        shape: [3]
        chunk: [3]
        fillvalue: 249

    QF1_VIIRSMBANDSDR:
      type: uint8
      shape: [null, null]
      storage:
        endian: little
        shape: [768, 3200]
        chunk: [768, 3200]
        fillvalue: 249

    QF2_SCAN_SDR:
      type: uint8
      shape: [null]
      storage:
        endian: little
        shape: [48]
        chunk: [48]
        fillvalue: 249

    QF3_SCAN_RDR:
      type: uint8
      shape: [null]
      storage:
        endian: little
        shape: [48]
        chunk: [48]
        fillvalue: 249

    QF4_SCAN_SDR:
      type: uint8
      shape: [null]
      storage:
        endian: little
        shape: [768]
        chunk: [768]
        fillvalue: 249

    QF5_GRAN_BADDETECTOR:
      type: uint8
      shape: [null]
      storage:
        endian: little
        shape: [16]
        chunk: [16]
        fillvalue: 249

    Radiance:
      type: uint16
      shape: [null, null]
      storage:
        endian: big
        shape: [768, 3200]
        chunk: [768, 3200]
        fillvalue: 65529

    RadianceFactors:
      type: float32
      shape: [null]
      storage:
        endian: big
        shape: [2]
        chunk: [2]
        fillvalue: -999.2999877929688

    Reflectance:
      type: uint16
      shape: [null, null]
      storage:
        endian: big
        shape: [768, 3200]
        chunk: [768, 3200]
        fillvalue: 65529

    ReflectanceFactors:
      type: float32
      shape: [null]
      storage:
        endian: big
        shape: [2]
        chunk: [2]
        fillvalue: -999.2999877929688


/Data_Products/VIIRS-M1-SDR:

  attributes:

    Instrument_Short_Name:
      type: string
      shape: [1,1]
      value: [[VIIRS]]

    N_Collection_Short_Name:
      type: string
      shape: [1,1]
      value: [[VIIRS-M1-SDR]]

    N_Dataset_Type_Tag:
      type: string
      shape: [1,1]
      value: [[SDR]]

    N_Instrument_Flight_SW_Version:
      type: int32
      shape: [1,1]
      value: [[20]]
      storage:
        endian: big

    N_Processing_Domain:
      type: string
      shape: [1,1]
      value: [[ops]]

    Operational_Mode:
      type: string
      shape: [1,1]
      value: [[NPP Normal Operations, VIIRS Operational]]

  ndarrays:

    VIIRS-M1-SDR_Aggr:
      type: objref
      shape: [null]
      storage:
        shape: [16]
        chunk: [16]
      value:
        - /All_Data/VIIRS-M1-SDR_All/Radiance
        - /All_Data/VIIRS-M1-SDR_All/Reflectance
        - /All_Data/VIIRS-M1-SDR_All/ModeScan
        - /All_Data/VIIRS-M1-SDR_All/ModeGran
        - /All_Data/VIIRS-M1-SDR_All/PadByte1
        - /All_Data/VIIRS-M1-SDR_All/NumberOfScans
        - /All_Data/VIIRS-M1-SDR_All/NumberOfMissingPkts
        - /All_Data/VIIRS-M1-SDR_All/NumberOfBadChecksums
        - /All_Data/VIIRS-M1-SDR_All/NumberOfDiscardedPkts
        - /All_Data/VIIRS-M1-SDR_All/QF1_VIIRSMBANDSDR
        - /All_Data/VIIRS-M1-SDR_All/QF2_SCAN_SDR
        - /All_Data/VIIRS-M1-SDR_All/QF3_SCAN_RDR
        - /All_Data/VIIRS-M1-SDR_All/QF4_SCAN_SDR
        - /All_Data/VIIRS-M1-SDR_All/QF5_GRAN_BADDETECTOR
        - /All_Data/VIIRS-M1-SDR_All/RadianceFactors
        - /All_Data/VIIRS-M1-SDR_All/ReflectanceFactors
      attributes:
        AggregateBeginningDate:
          type: string
          shape: [1,1]
          value: [["20131114"]]
        AggregateBeginningGranuleID:
          type: string
          shape: [1,1]
          value: [[NPP000650598298]]
        AggregateBeginningOrbitNumber:
          type: uint64
          shape: [1,1]
          value: [[10607]]
          storage:
            endian: big
        AggregateBeginningTime:
          type: string
          shape: [1,1]
          value: [[001030.352482Z]]
        AggregateEndingDate:
          type: string
          shape: [1,1]
          value: [["20131114"]]
        AggregateEndingGranuleID:
          type: string
          shape: [1,1]
          value: [[NPP000650598298]]
        AggregateEndingOrbitNumber:
          type: uint64
          shape: [1,1]
          value: [[10607]]
          storage:
            endian: big
        AggregateEndingTime:
          type: string
          shape: [1,1]
          value: [[001154.528149Z]]
        AggregateNumberGranules:
          type: int32
          shape: [1,1]
          value: [[1]]
          storage:
            endian: big

    VIIRS-M1-SDR_Gran_0:
      type:
        regref:
          selection: block
      shape: [null]
      storage:
        shape: [16]
        chunk: [16]
      value:
        - target: /All_Data/VIIRS-M1-SDR_All/Radiance
          start: [0,0]
          opposite: [767,3199]
        - target: /All_Data/VIIRS-M1-SDR_All/Reflectance
          start: [0,0]
          opposite: [767,3199]
        - target: /All_Data/VIIRS-M1-SDR_All/ModeScan
          start: [0]
          opposite: [47]
        - target: /All_Data/VIIRS-M1-SDR_All/ModeGran
          start: [0]
          opposite: [0]
        - target: /All_Data/VIIRS-M1-SDR_All/PadByte1
          start: [0]
          opposite: [2]
        - target: /All_Data/VIIRS-M1-SDR_All/NumberOfScans
          start: [0]
          opposite: [0]
        - target: /All_Data/VIIRS-M1-SDR_All/NumberOfMissingPkts
          start: [0]
          opposite: [47]
        - target: /All_Data/VIIRS-M1-SDR_All/NumberOfBadChecksums
          start: [0]
          opposite: [47]
        - target: /All_Data/VIIRS-M1-SDR_All/NumberOfDiscardedPkts
          start: [0]
          opposite: [47]
        - target: /All_Data/VIIRS-M1-SDR_All/QF1_VIIRSMBANDSDR
          start: [0,0]
          opposite: [767,3199]
        - target: /All_Data/VIIRS-M1-SDR_All/QF2_SCAN_SDR
          start: [0]
          opposite: [47]
        - target: /All_Data/VIIRS-M1-SDR_All/QF3_SCAN_RDR
          start: [0]
          opposite: [47]
        - target: /All_Data/VIIRS-M1-SDR_All/QF4_SCAN_SDR
          start: [0]
          opposite: [767]
        - target: /All_Data/VIIRS-M1-SDR_All/QF5_GRAN_BADDETECTOR
          start: [0]
          opposite: [15]
        - target: /All_Data/VIIRS-M1-SDR_All/RadianceFactors
          start: [0]
          opposite: [1]
        - target: /All_Data/VIIRS-M1-SDR_All/ReflectanceFactors
          start: [0]
          opposite: [1]
      attributes:
        Ascending/Descending_Indicator:
          type: uint8
          shape: [1,1]
          storage:
            endian: big
          value: [[0]]
        Band_ID:
          type: string
          shape: [1,1]
          value: [[M1]]
        Beginning_Date:
          type: string
          shape: [1,1]
          value: [["20131114"]]
        Beginning_Time:
          type: string
          shape: [1,1]
          value: [[001030.352482Z]]
        East_Bounding_Coordinate:
          type: string
          shape: [1,1]
          value: [[180]]
        Ending_Date:
          type: string
          shape: [1,1]
          value: [["20131114"]]
        Ending_Time:
          type: string
          shape: [1,1]
          value: [[001154.528149Z]]
        G-Ring_Latitude:
          type: float32
          shape: [8,1]
          storage:
            endian: big
          value: [[84.7619], [84.3261], [82.9881], [80.1059], [66.8805],
                  [67.2599], [67.3605], [81.274]]
        G-Ring_Longitude:
          type: float32
          shape: [8,1]
          storage:
            endian: big
          value: [[-67.3091], [-94.0303], [-112.513], [137.407], [120.752],
                  [114.378], [107.545], [106.405]]
        N_Algorithm_Version:
          type: string
          shape: [1,1]
          value: [[1.O.000.008]]
        N_Anc_Filename:
          type: string
          shape: [11,1]
          value: [[Terrain-Eco-ANC-Tile_20030125000000Z_ee00000000000000Z_NA_NA_N0398_1.O.0.0],
                  [Terrain-Eco-ANC-Tile_20030125000000Z_ee00000000000000Z_NA_NA_N0399_1.O.0.0],
                  [Terrain-Eco-ANC-Tile_20030125000000Z_ee00000000000000Z_NA_NA_N0430_1.O.0.0],
                  [Terrain-Eco-ANC-Tile_20030125000000Z_ee00000000000000Z_NA_NA_N0431_1.O.0.0],
                  [Terrain-Eco-ANC-Tile_20030125000000Z_ee00000000000000Z_NA_NA_N0463_1.O.0.0],
                  [Terrain-Eco-ANC-Tile_20030125000000Z_ee00000000000000Z_NA_NA_N0495_1.O.0.0],
                  [Terrain-Eco-ANC-Tile_20030125000000Z_ee00000000000000Z_NA_NA_N0496_1.O.0.0],
                  [Terrain-Eco-ANC-Tile_20030125000000Z_ee00000000000000Z_NA_NA_N0527_1.O.0.0],
                  [Terrain-Eco-ANC-Tile_20030125000000Z_ee00000000000000Z_NA_NA_N0528_1.O.0.0],
                  [off_Planet-Eph-ANC_Static_JPL_000f_20000101_200001010000Z_20000101000000Z_ee00000000000000Z_np],
                  [off_USNO-PolarWander-UT1-ANC_Ser7_USNO_000f_20131108_201311080000Z_20131108000003Z_ee20131115120000Z_np]]
        N_Aux_Filename:
          type: string
          shape: [55, 1]
          value: [[CMNGEO-PARAM-LUT_npp_20111101010000Z_20111101010000Z_ee00000000000000Z_PS-1-N-CCR-11-216-NGAS-002-PE-_noaa_all_all-_all],
                  [CmnGeo-SAA-AC_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_1_devl_dev_noaa_ops],
                  [VIIRS-DNB-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-I1-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-I2-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-I3-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-I4-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-I5-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M1-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M10-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M11-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M12-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M13-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M14-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M15-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M16-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M2-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M3-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M4-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M5-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M6-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M7-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M8-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-M9-SDR-DQTT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_BASELINE-MON-1_devl_dev_noaa_ops],
                  [VIIRS-SDR-BB-TEMP-COEFFS-LUT_npp_20120301000000Z_20111109010000Z_ee00000000000000Z_PS-1-N-CCR-12-347-JPSS-DPA-001-PE-_noaa_all_all-_all],
                  [VIIRS-SDR-COEFF-A-LUT_npp_20110822010100Z_20111101010000Z_ee00000000000000Z_PS-1-N-CCR-11-117-JPSS-DPA-001-PE-_noaa_all_all-_all],
                  [VIIRS-SDR-COEFF-B-LUT_npp_20110822010100Z_20111101010000Z_ee00000000000000Z_PS-1-N-CCR-11-117-JPSS-DPA-001-PE-_noaa_all_all-_all],
                  [VIIRS-SDR-DELTA-C-LUT_npp_20120217000000Z_20120220000001Z_ee00000000000000Z_PS-1-N-CCR-12-330-JPSS-DPA-002-PE-_noaa_all_all-_all],
                  [VIIRS-SDR-DG-ANOMALY-DN-LIMITS-LUT_npp_20130708000000Z_20130729000000Z_ee00000000000000Z_PS-1-O-CCR-13-1150-JPSS-DPA-007-PE_noaa_all_all-_all],
                  [VIIRS-SDR-DNB-C-COEFFS-LUT_npp_20130905000000Z_20130905000000Z_ee00000000000000Z_PS-1-O-CCR-13-1062-JPSS-DPA-019-PE_noaa_all_all-_all],
                  [VIIRS-SDR-DNB-DN0-LUT_npp_20130905000000Z_20130905000000Z_ee00000000000000Z_PS-1-O-CCR-13-1062-JPSS-DPA-019-PE_noaa_all_all-_all],
                  [VIIRS-SDR-DNB-F-PREDICTED-LUT_npp_20131103143000Z_20131108003000Z_ee00000000000000Z_PS-1-0-CCR-13-1305-JPSS-DPA-063-PE_noaa_all_all-_all],
                  [VIIRS-SDR-DNB-FRAME-TO-ZONE-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-1-PE-_devl_dev_all-_all],
                  [VIIRS-SDR-DNB-RVF-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-2-PE-_devl_dev_all-_all],
                  [VIIRS-SDR-DNB-STRAY-LIGHT-CORRECTION-LUT_npp_20131004000000Z_20131004000000Z_ee00000000000000Z_PS-1-O-CCR-13-1198-JPSS-DPA-006-PE_noaa_all_all-_all],
                  [VIIRS-SDR-EBBT-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-2-PE-_devl_dev_all-_all],
                  [VIIRS-SDR-EMISSIVE-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-1-PE-_devl_dev_all-_all],
                  [VIIRS-SDR-F-PREDICTED-LUT_npp_20131103142900Z_20131108003000Z_ee00000000000000Z_PS-1-0-CCR-13-1305-JPSS-DPA-063-PE_noaa_all_all-_all],
                  [VIIRS-SDR-GAIN-LUT_npp_20120217000000Z_20120220000001Z_ee00000000000000Z_PS-1-N-CCR-12-330-JPSS-DPA-002-PE-_noaa_all_all-_all],
                  [VIIRS-SDR-GEO-DNB-PARAM-LUT_npp_20130601000000Z_20130601000000Z_ee00000000000000Z_PS-1-O-CCR-13-1171-JPSS-DPA-009-PE_noaa_all_all-_all],
                  [VIIRS-SDR-GEO-IMG-PARAM-LUT_npp_20130601000000Z_20130601000000Z_ee00000000000000Z_PS-1-O-CCR-13-1171-JPSS-DPA-009-PE_noaa_all_all-_all],
                  [VIIRS-SDR-GEO-MOD-PARAM-LUT_npp_20130601000000Z_20130601000000Z_ee00000000000000Z_PS-1-O-CCR-13-1171-JPSS-DPA-009-PE_noaa_all_all-_all],
                  [VIIRS-SDR-HAM-ER-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-2-PE-_devl_dev_all-_all],
                  [VIIRS-SDR-OBC-ER-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-2-PE-_devl_dev_all-_all],
                  [VIIRS-SDR-OBC-RR-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-2-PE-_devl_dev_all-_all],
                  [VIIRS-SDR-OBS-TO-PIXELS-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-1-PE-_devl_dev_all-_all],
                  [VIIRS-SDR-QA-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-1-PE-_devl_dev_all-_all],
                  [VIIRS-SDR-RADIOMETRIC-PARAM-LUT_npp_20130220000000Z_20130221000000Z_ee00000000000000Z_PS-1-O-CCR-13-885-JPSS-DPA-004-PE_noaa_all_all-_all],
                  [VIIRS-SDR-REFLECTIVE-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-1-PE-_devl_dev_all-_all],
                  [VIIRS-SDR-RSR-LUT_npp_20130320000000Z_20130405000400Z_ee00000000000000Z_PS-1-O-CCR-13-888-JPSS-DPA-003-PE_noaa_all_all-_all],
                  [VIIRS-SDR-RTA-ER-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-2-PE-_devl_dev_all-_all],
                  [VIIRS-SDR-RVF-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-2-PE-_devl_dev_all-_all],
                  [VIIRS-SDR-SOLAR-IRAD-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-1-PE-_devl_dev_all-_all],
                  [VIIRS-SDR-TELE-COEFFS-LUT_npp_20120229000000Z_20120301000000Z_ee00000000000000Z_PS-1-O-CCR-12-346-JPSS-DPA-002-PE_noaa_all_all-_all],
                  [VIIRS-SOLAR-DIFF-VOLT-LUT_npp_20020101010000Z_20020101010000Z_ee00000000000000Z_PS-1-D-NPP-1-PE-_devl_dev_all-_all]]
        N_Beginning_Orbit_Number:
          type: uint64
          storage:
            endian: big
          shape: [1,1]
          value: [[10607]]
        N_Beginning_Time_IET:
          type: uint64
          storage:
            endian: big
          shape: [1,1]
          value: [[1763079065352482]]
        N_Creation_Date:
          type: string
          shape: [1,1]
          value: [["20131114"]]
        N_Creation_Time:
          type: string
          shape: [1,1]
          value: [[015435.407508Z]]
        N_Day_Night_Flag:
          type: string
          shape: [1,1]
          value: [[Night]]
        N_Ending_Time_IET:
          type: uint64
          storage:
            endian: big
          shape: [1,1]
          value: [[1763079149528149]]
        N_Graceful_Degradation:
          type: string
          shape: [1,1]
          value: No
        N_Granule_ID:
          type: string
          shape: [1,1]
          value: [[NPP000650598298]]
        N_Granule_Status:
          type: string
          shape: [1,1]
          value: [[100% night for day only product]]
        N_Granule_Version:
          type: string
          shape: [1,1]
          value: [[A1]]
        N_Input_Prod:
          type: string
          shape: [11,1]
          value: [[52841a43-72729-0a18020b-5ca2ecc7],
                  [52842caa-6bd01-0a18020b-5ca29526],
                  [52842caa-e865c-0a18020b-5caa5e81],
                  [52842cab-d9065-0a18020b-5ca9686b],
                  [52842cad-b0140-0a18020b-5ca6d968],
                  [52842cb1-68047-0a18020b-5ca25873],
                  [52842cb4-0d195-0a18020b-5c9ca9c4],
                  [52842cb8-74736-0a18020b-5ca31f49],
                  [52842cf8-06261-0a180228-5c9c2981],
                  [52842cf8-11bcb-0a180228-5c9ce2eb],
                  [52842cf8-15785-0a180228-5c9d1ea5]]
        N_LEOA_Flag:
          type: string
          shape: [1,1]
          value: [[Off]]
        N_NPOESS_Document_Ref:
          type: string
          shape: [3,1]
          value: [[474-00001-03_JPSS-CDFCB-X-Vol-III_0123B_I1.5.07.01.pdf],
                  [474-00001-03_JPSS-CDFCB-X-Vol-III_0123B_VIIRS-M1-SDR-PP.xml],
                  [474-00090_OAD-VIIRS-CAL-GEO-SDR_D_I1.5.07.01.pdf]]
        N_Nadir_Latitude_Max:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[81.2793]]
        N_Nadir_Latitude_Min:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[80.1277]]
        N_Nadir_Longitude_Max:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[137.164]]
        N_Nadir_Longitude_Min:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[106.784]]
        N_Number_Of_Scans:
          type: int32
          storage:
            endian: big
          shape: [1,1]
          value: [[48]]
        N_Percent_Erroneous_Data:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[0]]
        N_Percent_Missing_Data:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[0]]
        N_Percent_Not-Applicable_Data:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[0]]
        N_Quality_Summary_Names:
          type: string
          shape: [2,1]
          value: [[Scan Quality Exclusion],
                  [Summary VIIRS SDR Quality]]
        N_Quality_Summary_Values:
          type: int32
          storage:
            endian: big
          shape: [2,1]
          value: [[48], [0]]
        N_Reference_ID:
          type: string
          shape: [1,1]
          value: [[52842d5b-636cd-0a180228-5ca1edf0]]
        N_Satellite/Local_Azimuth_Angle_Max:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[180]]
        N_Satellite/Local_Azimuth_Angle_Min:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[-180]]
        N_Satellite/Local_Zenith_Angle_Max:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[70.2555]]
        N_Satellite/Local_Zenith_Angle_Min:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[0.00470374]]
        N_Software_Version:
          type: string
          shape: [1,1]
          value: [[I1.5.07.02]]
        N_Solar_Azimuth_Angle_Max:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[180]]
        N_Solar_Azimuth_Angle_Min:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[-180]]
        N_Solar_Zenith_Angle_Max:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[110.733]]
        N_Solar_Zenith_Angle_Min:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[93.5212]]
        N_Spacecraft_Maneuver:
          type: string
          shape: [1,1]
          value: [[Normal Operations]]
        North_Bounding_Coordinate:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[90]]
        South_Bounding_Coordinate:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[66.8805]]
        West_Bounding_Coordinate:
          type: float32
          storage:
            endian: big
          shape: [1,1]
          value: [[-180]]
```
