# EDAM
**EDAM** - **E**nvironmental **D**econtamination of **A**ncient **M**etagenomes
***
A pipeline to remove environmental contaminants from ancient metagenomic samples using an OTU classification approach. Full details of the pipeline can be found in [Dahlquist-Axe *et al*. (2024)]().

[Basic usage](https://github.com/DrATedder/EDAM/blob/main/README.md#basic-usage)

## Basic usage
`EDAM` assumes you have used `centrifuge` (see [here](https://github.com/DaehwanKimLab/centrifuge) for details) to assign OTUs to your reads, and that subsequent processes will be be based on `centrifugeReport.txt` type output files.
>[!NOTE]
>Basic usage assumes that you have specific 'contaminant' reads that have been through the `centrifuge` OTU assignment protocol. If you do **NOT** have environmental 'blanks' associated with your sampling, it is recommended to use one of the packaged 'standard' lists (see below).

```bash
python3 centrifuge_env_decontam.py [sample_folder] [contaminent_folder] [metadata_file] [tax_level]
```

**Requirements:**
1.  directory containing sample files (`centrifugeReport.txt` format; see below for naming protocols)
2.  directory containing either contaminents (`centrifugeReport.txt` format; can be the same folder as the samples are given in)
3.  metadata file (CSV format, see below for details)
4.  taxonomic level (either 'total', 'genus' or 'species'; see below for explanation)

**File naming protocol:** Centrifuge output files should be named in the following manner:
> shortname_anything_centrifugeReport.txt

1.    shortname: used to link files to the metadata
2.    anything: not used, but can be anything
3.    centrifugeReport.txt: used by the programme to identify the correct files within the given directory
4.    underscores ('_') must be used between file name elements as these are used for splitting file names

**Metadata format:** Metadata should be in two column CSV format as shown below (example can be downloaded [here](https://github.com/DrATedder/ancient_metagenomics/blob/42e6d56453cc1c63e0ee8885aeb0acfc4acc42d1/decontamination_metadata_example.csv "Decontaminant metadata example file")). The first column should contain the sequence 'shortname' for each file you want to process, and the second column should contain the sequence 'shortname' for the contaminant file. 

>[!Note]
>If either file (sample or contaminent) is in the metadata but not in the directories given, they will be ignored. 

|sample|contaminent|
|---|---|
|ERR9638263|ERR9638259|
|ERR9638253|ERR9638262|
|ERR9638254|ERR9638262|
|ERR9638255|ERR9638262|
|ERR9638256|ERR9638262|

**Taxonomic level:** Taxonomic level explains what OTUs from the contaminant sample will be reomved from the 'real' sample. Brief explanations for these are given below:

*total* - This will remove **any** OTUs which overlap at any taxRank level. This is likely to be super conservative, and may only be useful in certain circumstance.

*genus* - This will remove overlapping OTUs from the genus level down (inc. 'genus', 'species', 'subspecies' & 'leaf').

*species* - This will remove overlapping OTUs from the species level down (inc. 'species', 'subspecies' & 'leaf').

**Output files:** Output file, still in 'centrifugeReport.txt' format will be output into the directory containing the samples. File names will have been appended in the following way:
> shortname_anything_<tax_level>_decontam_centrifugeReoprt.txt

>[!NOTE]
>Some text
