# AgBioData GFF3 working group recommendations

**Members**
 - AgBioData: Ethalinda Cannon, Andrew Farmer, Zhiliang Hu, Rex Nelson, Monica Poelchau, Surya Saha
 - Alliance of Genome Resources: Scott Cain, Nathan Dunn, Sierra Moxon, Rob Buels
 - NCBI: Vamsi Kodali, Terence Murphy


The GFF3 format is a common, flexible tab-delimited format representing the structure and function of genes or other mapped features. However, with increasing re-use of annotation data, this flexibility has become an obstacle for standardized downstream processing. Common software packages that export annotations in GFF3 format model the same data and metadata in different notations, which puts the burden on end-users to interpret the data model.

The AgBioData consortium is a group of genomics, genetics and breeding databases and partners working towards shared practices and standards. Providing concrete guidelines for generating GFF3, and creating a standard representation of the most common biological data types would provide a major increase in efficiency for AgBioData databases and the genomics research community that use the GFF3 format in their daily operations.

The AgBioData GFF3 working group has developed recommendations to solve common problems in the GFF3 format. We suggest improvements for each of the GFF3 fields, as well as the special cases of modeling functional annotations, and standard protein-coding genes. We welcome discussion of these recommendations from the larger community.
 
**References**
1.  [Sequence Ontology gff3 specifications](https://github.com/The-Sequence-Ontology/Specifications/blob/master/gff3.md)
2.  [AgBiodata consortium](https://www.agbiodata.org/) ![](https://www.agbiodata.org/sites/default/files/styles/newtopimage/public/imageblock/AgBioData-websiteHeader_v2.png?itok=_Z2cIMbc)
3. [GFF3 recommendation google doc (comment access)](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#)
4.  [NCBI Genbank genomes gff3 specifications](https://www.ncbi.nlm.nih.gov/sites/genbank/genomes_gff/)
5.  [NCBI Genbank gff3 documentation](https://www.ncbi.nlm.nih.gov/datasets/docs/about-ncbi-gff3/)
*****

**GFF3 working group goals**

*Ultimate goal* - to use a GFF3 file from any software or any database in downstream processing tools or applications (e.g. VEP, Tripal, Apollo, ...) WITHOUT having to modify it

1.Databases and software export their GFF3 files in (a) standard way(s)

2.Databases and software know how to import standard information from a GFF3

![Goals](https://github.com/NAL-i5K/AgBioData_GFF3_recommendation/blob/a0b7e70577b27d7c2c15451ed90802b3bfb90a6d/docs/Goals.png)


*****
**Summary of Recommendations**

For each column and reserved attribute, we provide the following results from our discussions:
 - Change level: The level of change relative to the SO specification. Values are 'No change', 'Recommendation only','minor', 'moderate', 'major'
 - Summary: A summary of the GFF3 working group's findings.
 - Proposed changes to specification: A list of the proposed changes to the SO specification.
 - Rationale: The rationale behind these changes.
 - Best Practices: Recommended best practices for this field.
 - Validation: How software would validate whether the field is used correctly.  
 - Example: An example implementation of the field. 

Types of changes that we recommend:
 - No change: 5
 - *Recommendation only: 9*
 - Minor: 1
 - Moderate: 1
 - Major: 1
![Summary](https://github.com/NAL-i5K/AgBioData_GFF3_recommendation/blob/ea21c39f248592cf13731d439e3562c5e4ed7532/docs/Summaryofrecommendations_Jul17.png)

We primarily have recommendations on how to
 - Interpret the specification
 - Model standard data types

Recommendation google doc: [https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018)
Comments are welcome in suggesting mode


*****
**Acknowledgements**

We thank Margaret Woodhouse for the inspiration for this working group, and many other contributors from the larger research community who have provided feedback on earlier versions
