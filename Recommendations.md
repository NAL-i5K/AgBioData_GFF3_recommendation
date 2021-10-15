AgBioData GFF3 working group recommendations
==============================================


- [Motivation](#motivation)
- [Specific recommendations](#specific-recommendations)

Maintainer | Github handle or E mail | Column
---------- | ----------------------- | ------
Rex | @maxglycine | [seqid](#seqid) (column 1)
Surya | @suryasaha | [source](#source) (column 2)	
Vamsi/Terence | @vkkodali | [type](#type) (column 3)	
Ethy | ekcannon@iastate.edu | [start, end](#start-end) (column 4,5) 	
Ethy | ekcannon@iastate.edu | [score](#score) (column 6)	
Monica | @mpoelchau | [phase](#phase) (column 8)
Monica | mpoelchau | [attributes : ID](#attributes--id)	(column 9)
Andrew | @adf-ncgr | [attributes : Name](#attributes--name)	(column 9)
Rob | @rbuels | [attributes : Alias](#attributes--alias)	(column 9)
Zhiliang| zhu@iastate.edu | [attributes : Dbxref](#attributes--dbxref) (column 9) 
NA | NA| [attributes : Derives_from](#attributes--derivesfrom) (column 9)
Rob | @rbuels | [attributes : Note](#attributes--note) (column 9) 
Rob | @rbuels | [attributes : Ontology_term](#attributes--ontologyterm) (column 9) 
Rob | @rbuels | [attributes : Target](#attributes--target) (column 9) 
Andrew | @adf-ncgr | [attributes : Gap](#attributes--gap) (column 9) 
Rob | @rbuels | Modeling hierarchical relationships of a protein-coding gene
Rob/Surya | @rbuels @suryasaha |  Functional annotations

Please tag the associated maintainer when you file an issue.

- [Pragmas](#pragmas)
- [Other caveats and unresolved issues](#other-caveats-and-unresolved-issues)
- [Examples](#examples)

------------------------------------------

Motivation 
==========

The [GFF3](https://github.com/The-Sequence-Ontology/Specifications/blob/master/gff3.md) format  is a commonly used tab-delimited format representing the structure and function of genes or other mapped features. The format's flexibility allows scientists to easily manipulate GFF3 files, and it helps accurately represent the complex biological information being captured. However, with increasing re-use of annotation data, in particular from different sources (software output from custom datasets, and/or reference datasets provided by databases), this flexibility has become an obstacle for downstream processing. Common software packages that export annotations in GFF3 format model the same data and metadata in different notations, which puts the burden on end-users to understand possibly undocumented assumptions about the data model, then to convert the data for downstream applications. For example, the CDS phase field is commonly misinterpreted by both dataset generators and consumers, which can lead to vastly different and erroneous amino acid sequences derived from the same GFF3 file.


The [AgBioData consortium](https://www.agbiodata.org/) is a group of genomics, genetics and breeding databases and partners working towards shared practices and standards. Almost every AgBioData database uses the GFF3 format in some capacity, either for content ingest (into the database or associated tools, such as JBrowse), analysis, distribution, or all of the above. Many AgBioData members report that much of their data wrangling time is spent reformatting and correcting GFF3 files that model the same data types in different ways. Providing concrete guidelines for generating GFF3, and creating a standard representation of the most common biological data types in GFF3 that would be compatible with the most commonly used tools, would provide a major increase in efficiency for all AgBioData databases.

The AgBioData GFF3 working group has developed new recommendations to solve common problems in the GFF3 format. We have referred to and in some cases adopted guidelines developed by the Alliance of Genome Resources ([https://docs.google.com/document/d/1yjQ7lozyETeoGkPfSMTAT8IN3ZIAuy5YkbsBdjGeLww/edit#heading=h.vz8hm961nr5b](https://docs.google.com/document/d/1yjQ7lozyETeoGkPfSMTAT8IN3ZIAuy5YkbsBdjGeLww/edit#heading=h.vz8hm961nr5b)), and NCBI ([https://www.ncbi.nlm.nih.gov/sites/genbank/genomes\_gff/](https://www.ncbi.nlm.nih.gov/sites/genbank/genomes_gff/) and [https://www.ncbi.nlm.nih.gov/datasets/docs/about-ncbi-gff3/](https://www.ncbi.nlm.nih.gov/datasets/docs/about-ncbi-gff3/)). Below, we suggest improvements for each of the GFF3 fields, as well as the special cases of modeling functional annotations, and standard protein-coding genes. We welcome debate and discussion of these recommendations from the larger community - these recommendations will only be helpful if they are refined and then adopted by many. Our goal is to clarify the GFF3 specification and limit ambiguity for AgBioData and other databases and resources.

Specific recommendations
========================

We recommend that developers and databases follow the Sequence Ontology specifications ([https://github.com/The-Sequence-Ontology/Specifications/blob/master/gff3.md](https://github.com/The-Sequence-Ontology/Specifications/blob/master/gff3.md)) with emphases and additions below. Each field contains information in the following categories:

- Change level: The level of change relative to the SO specification. Values are &#39;No change&#39;, &#39;Recommendation only&#39;,&#39;minor&#39;, &#39;moderate&#39;, &#39;major&#39;
- Summary: A summary of the GFF3 working group&#39;s findings.
- Proposed changes to specification: A list of the proposed changes to the SO specification.
- Rationale: The rationale behind these changes.
- Best Practices: Recommended best practices for this field.
- Validation: How software would validate whether the field is used correctly.
- Example: An example implementation of the field.


Seqid 
=====
**Column 1**
  - **Change Level**: Recommendation only
  - **Summary**: Optionally, provide an Alias table to specify alternate identifiers/aliases for the seqid.
  - **Proposed Changes to Specifications**: Institute a new pragma for alias table link.
  - **Rationale**: Sequences often have aliases (multiple identifiers, human-readable names that are not globally unique), and users prefer human-readable display names when viewing sequences in browsers.
  - **Recommendation**: Optionally provide a machine- and human- readable &#39;alias&#39; table to specify identifiers and their aliases, which is provided by GenBank, and requires INSDC submission of the genome.
  - **Validation**
    - A pragma line in the GFF3 header should provide a resolvable URL to the GenBank Alias table. Validator should only verify that the link is active.
    - If the GenBank Alias table is not available, then a separate alias table can be provided, where all identifiers in column 1 of the GFF3 file must be uniquely present in column 1 of the alias table. The columns in the alias file should be tab delimited and all lines beginning with a hash (#) will be ignored. There will be NO validation for an alternate alias table.
  - **Example** [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.tr1vioe7doqb](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.tr1vioe7doqb)


Source
======
**Column 2**

  - **Change level:** no change
  - **Summary**: There are no major changes from the previous SO specification. We recommend that the source field is used to define the source of the sequence feature concisely. Source is used to extend the feature ontology by adding a qualifier to the type field.
  - **Proposed changes to specification:** none
  - **Rationale**: The values used for this field vary widely as it's a free text field which can lead to parsing and interpretation issues for downstream software and data loading.
  - **Best Practices**: We recommend that programs generating and consuming GFF3 follow the constraints outlined below and account for the fact that the feature can be a result of multiple tools in a pipeline.
  - **Validation**
    - It is not necessary to specify a source. If there is no source, put a `.` (a period) in this field.
    - Note that only spaces are allowed to represent whitespace. In general, follow the formatting requirements of the specification. From the GFF3 specification: "Literal use of tab, newline, carriage return, the percent (%) sign, and control characters must be encoded using [RFC 3986 Percent-Encoding](https://tools.ietf.org/html/rfc3986#section-2.1); no other characters may be encoded."
    - If there are multiple sources, use a literal comma to separate them (NOT `%2C`). Source names should not include literal commas.
    - Specify the tool, method or pipeline used to generate this annotation or the database it was acquired from. Mention the version number if available.
    - The feature should be a well defined output of the tool or database specified. If there is any ambiguity or post-processing, it should be clearly explained in the pragma stanza at the top of the GFF file. See the [pragma](#pragmas) section below.
  - **Example** [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.8yzwr29w95mh](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.8yzwr29w95mh)


Type
====
**Column 3**
  - **Change level**: no change
  - **Summary**: We endorse the Alliance recommendations for the `type` field when modeling hierarchical gene features. This aligns with the SO specification that expects this to be "either a term from the Sequence Ontology or an SO accession number."
  - **Proposed changes to specification**: none
  - **Rationale**: Software interpreting the type column can run into difficulties with complex cases. Software is easier to develop and maintain if we can make some simplifying assumptions about how genes are typically modeled. Using simple terms will additionally improve human readability and interpretation.
  - **Best practice**: Top-level feature types can include `gene` and `pseudogene`. Optionally, include a `so_term_name` attribute in column 9 to specify the child (type) of `gene` - e.g. `protein_coding_gene`, `ncRNA_gene`, `miRNA_gene` and `snoRNA_gene` ([http://purl.obolibrary.org/obo/SO\_0000704](http://purl.obolibrary.org/obo/SO_0000704)). Transcript features should include the appropriate SO term in column 3 (e.g. `mRNA`, `snoRNA`, etc).
  - **Validation**
    - Must be a valid SO term or SO accession number
    - All child rows should use a type within the hierarchy of the parent
    - A list of the SO terms and the hierarchy in OBO format is fetched from [http://song.cvs.sourceforge.net/viewvc/\*checkout\*/song/ontology/sofa.obo](http://song.cvs.sourceforge.net/viewvc/*checkout*/song/ontology/sofa.obo) by default
  - **Example** [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.t94q0hgki4w9](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.t94q0hgki4w9)


Start, End
==========
**Column 4,5**
  - **Change level**: no change
  - **Summary**: No changes from the SO specification.
  - **Proposed changes to specification**: none
  - **Rationale**: Consistent use of `start` and `end` coordinates are essential.
  - **Best practice**: We recommend that programs generating and consuming GFF3 be aware of the existence of circular chromosomes, which will require alternate interpretation of the end coordinates.
  - **Validation**:
    - `start` and `end` are 1-based coordinates.
    - `start` must always be less than or equal to `end`.
    - A feature with no length, for example, an insertion site, is indicated by `start = end`. The insertion site is to the right of the position. There is no recommendation for representing an insertion at the beginning, that is, before the first base as 0 is an invalid coordinate.
    - A feature that is one base in length, e.g., a SNP, is also indicated by `start = end`. Distinguishing between one-base and zero-length features will have to rely on other fields, such as the type field.
    - In the absence of the `Is_circular=true` attribute in column 9, `end` indicates the terminal coordinate of the feature.
    - If `Is_circular=true` appears in column 9, `start` gives the beginning coordinate of the feature and `end` is `start + (feature length - 1)`. This means the value for `end` may be larger than the chromosome size.
  - **Examples**: [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.15ngz7f2sit7](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.15ngz7f2sit7)


Score
=====
**Column 6**

  - **Change level**: moderate
  - **Summary**: There is no clear guidance on how to interpret the score column. Therefore, define how the score was calculated in a pragma.
  - **Proposed changes to specification**: Define score calculation via pragma.
  - **Rationale**: There is currently no standard for providing metadata or context for the score column, rendering the score essentially meaningless.
  - **Best practice**: Define how the score was calculated in a pragma. May have multiple parts, such as score name, program, version, range, and whether quality increases or decreases or is constant with increasing values. Use [EDAM ontology](https://edamontology.org) where possible. The score itself must be a floating point number. This recommendation considers the score column only when representing gene models. See [Pragmas section](#pragmas) for more information.
  - **Validation**
    - A period indicates no score.
    - If any record has a value in the score column:
      - It must be a floating point number
      - There must be a `#!score` pragma in the following format: `#!score score-name program program-version score-range decreases|increases`
  - **Example**: [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.wsrdofcyxnmu](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.wsrdofcyxnmu)


Strand
======
**column 7**
  - **Change level**: No change.
  - **Summary**: No change from the original specification.
  - **Rationale**: NA
  - **Best practices**: Follow the original specification.
  - **Validation**: Values should include `+`, `-` or `.`. A `?` can be used when the strand is relevant but unknown.
  - **Example**: NA


Phase
=====
**Column 8**
  - **Change level**: recommendation only
  - **Summary**: Programs generating and consuming gff3 should pay close attention to the phase field and validate it, as phase is often incorrect.
  - **Proposed changes to specification**: none
  - **Rationale**: We have identified three main problems with the phase field. 
    1. Phase is often ignored or misinterpreted, both by programs generating gff3 and programs that consume it. When recorded manually, phase is often incorrect. This is problematic for programs that calculate the CDS and protein sequence using the combination of CDS coordinates and phase. Other methods that are frequently used to calculate the CDS and protein sequence (e.g. longest ORF, identifying start and stop codons) make critical assumptions that can also generate incorrect sequence, in particular for fragmented genomes where gene models may not have start and/or stop codons. 
    2. Even if the phase is correct, a translation table is required to correctly calculate the protein sequence, and there may be multiple translation tables needed for a given gff3, for example when both nuclear and organellar sequence is represented. 
    3. Even when the phase and translation tables are correct, the correct sequence may not be inferred due to post-translational modifications (e.g. selenocysteines) or problematic reference genome assemblies. NCBI represents these edge cases via the `transl_except` attribute.
  - **Best practices**: We recommend that programs generating and consuming gff3 pay close attention to this value and validate it; however, validation may still fail in complex cases. Phase may be an example where the GFF3 format has reached its limit. In cases where the correct sequence may not be inferred due to post-translational modifications (e.g. selenocysteines) or problematic reference genome assemblies, use the `transl_except` convention developed by NCBI on the CDS feature: `transl_except=(pos:<base_range>%2Caa:<amino_acid>)`; see: [https://www.ncbi.nlm.nih.gov/genbank/genomes\_gff/](https://www.ncbi.nlm.nih.gov/genbank/genomes_gff/)
  - **Validation**: For a description of what phase means in the context of a single CDS line, see the "Column 8: phase" section of the current [GFF3 specification.](https://github.com/The-Sequence-Ontology/Specifications/blob/master/gff3.md) Optionally provide the translation table id, and the reference sequences in the GFF3 that will use that translation table id, in a phase pragma. The validator will use the translation tables in [https://www.ncbi.nlm.nih.gov/IEB/ToolBox/C\_DOC/lxr/source/data/gc.prt](https://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/source/data/gc.prt). If no phase pragma is given, or if not all reference sequences are specified in the phase pragma, the validator will use the Standard genetic code (id 1). Protein and CDS fasta are optional but highly recommended. Validator will generate CDS/protein sequences based on the phase specified in the gff3 file.
    - If no CDS/protein fasta is available:
      - Check for internal stops in the protein sequence - validation fails if stops are present
    - If CDS and protein fasta are available:
      - Compare given sequence to sequence generated from gff3 - validation fails if sequences are not identical
  - **Example**:
    - Pragma specifying translation table: specify exemptions to standard code only [https://www.ncbi.nlm.nih.gov/IEB/ToolBox/C\_DOC/lxr/source/data/gc.prt#0150](https://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/source/data/gc.prt#0150)
    - Example of incorrect vs. correct phase: [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.d9hxh9okm6rg](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.d9hxh9okm6rg)


Attributes : ID
===============
**Column 9**
  - **Change level**: recommendation only
  - **Summary**: The ID attribute's role is to specify relationships between parent and child features within the GFF3. However, it is often - but not always - also used to specify a globally unique, persistent identifier. This second interpretation causes many problems with downstream software and validators. We recommend NOT using the ID attribute to specify the globally unique, persistent identifier, but instead using a separate attribute, such as `Dbxref` or `gene_id`.
  - **Proposed changes to specification**: Recommend additional attributes (reserved or non-reserved) to specify the globally unique, persistent identifier
  - **Rationale**: The GFF3 specification requires an ID attribute to define parent-child (part of) relationships for hierarchically modeled features (see specification for a detailed definition). In practice, the ID attribute is often also used to stand in for a globally unique, persistent identifier (in particular for genes, transcripts and proteins; see this article for definitions of and best practices on persistent identifiers: [https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.2001414](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.2001414)). This second interpretation becomes problematic when users or software assume it is true in all cases, as they expect additional meaning beyond a generic, unique string. While a dual-use ID attribute may seem convenient, it is not always clear which level of a gene feature may have the 'true' persistent identifier - the gene, transcript, protein, even exon? In addition, it is not always possible to accurately model parent-child relationships if it is also a globally unique, persistent identifier.
  - **Best practices**: Use the ID attribute as originally intended by the GFF3 specification, and do not assume that it contains a globally unique persistent identifier. For these, use an additional attribute. The attribute may depend on the use case - for example, if the persistent identifier is maintained by another database, Dbxref may be used (however, it may be confusing if multiple Dbxref identifiers are specified). The Alliance for Genome Resources uses the unreserved attributes `gene_id`, `transcript_id`, and `protein_id`, where the values of these are curies ([https://en.wikipedia.org/wiki/CURIE](https://en.wikipedia.org/wiki/CURIE)). NCBI RefSeq uses the unreserved attributes `gene`, `transcript_id`, and `protein_id`. We recommend using either of these conventions, with an additional `feature_id` attribute for features that are neither genes, transcripts, or proteins. While this recommendation may require software and databases to adjust, this is simpler than forging a way for the ID attribute to meet all downstream tools' and users' needs. Note the still unresolved yet related problem of the corresponding FASTA definition line - there are no guidelines in terms of correspondence between the FASTA defline and identifying information in the GFF3 file.
  - **Validation**: only validate whether IDs for each feature are unique within the scope of the GFF file, with the exception of discontinuous features. Persistent identifiers specified via the `Dbxref` attribute can be validated according to Dbxref rules.
  - **Example**: [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.ajk6obcear0c](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.ajk6obcear0c)


Attributes : Name
=================
**Column 9**
  - **Change level**: recommendation only
  - **Summary**: A designation for the given feature used for display.
  - **Proposed changes to specification**: None
  - **Rationale**: Naming standards exist and should be followed when possible.
  - **Best practices**:
    - Various taxonomic communities have nomenclature standards that aim to provide consistent naming across genes. Please identify and follow these standards for your community.
    - Name should not be mistaken for a unique identifier, or `dbxref`
    - Note that different, non-reserved attributes are sometimes used instead of Name. For example, NCBI uses `product` for the protein product name, `gene_desc` for the full gene name, and `symbol` as the gene abbreviation.
  - **Validation**: refer to different community standards. No automated validation currently possible.
  - **Example**: [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.or36qtilfraa](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.or36qtilfraa)


Attributes : Alias
==================
**Column 9**
  - **Change level**: Recommendation only
  - **Summary**: There are no major changes from the previous SO specification. The primary function of Alias appears to be for human consumption, display, indexing, and tracking synonyms or prior names. If there are additional functions that you know the value should be used for, we recommend that you choose a different attribute that is more specific, and could be consumed programmatically. The validator won't check Alias values.
  - **Rationale**: The Alias field is used for alternate names or identifiers, and there are no real constraints on what these may be. Some important cases are as follows. If a gene is merged, or if the type of a genomic feature is changed, the name of the original feature may need to change. Conversely, if a gene is split, we should retain a reference to its original name. If two papers cite the same gene using a different name, we should be able to search for either one even if only one is an official one.
  - **Proposed changes to specification**: None
  - **Best practices**: Use Alias for human consumption of alternate or historical names and identifiers (e.g., gene merge), but do not assume that this field will be consumed programmatically. Alias should not be a replacement for Dbxref, and valid CURIE in Dbxref should be housed within Dbxref and not in Alias. Commas, tabs, and pipes should be avoided in alias names. It is possible to have multiple Alias values. It is recommended that these be separated via a comma.
  - **Validation**: Any set of symbols would be appropriate and does not need to be unique. Alias values can not include a semicolon.
  - **Example**: [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.qub8uu7yavbh](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.qub8uu7yavbh)


Attributes : Dbxref
===================
**Column 9**
  - **Change level**: Recommendation only
  - **Summary**: 
    1. Use the `Dbxref` field to cross-reference the _same_ entity at a database - the field is sometimes mis-interpreted to reference related information, but not the same entity. 
    2. The `Dbxref` should result in a resolvable URL.
  - **Proposed changes to specification**: Recommend use of the field for global entity references and crosslinks between databases.
  - **Rationale**: The database cross-reference (dbxref) links a particular feature in the GFF record to a specific external database record by identity, source, association, or ontology links. To date, there are four established lists with hundreds of registered databases that offer external links by dbxref. The GO consortium list is maintained on GitHub with defined schema and format validation tools. It currently lists 266 databases ([http://amigo.geneontology.org/xrefs](http://amigo.geneontology.org/xrefs)). The UniProt Knowledgebase cross-reference list contains 183 databases ([https://www.uniprot.org/docs/dbxref](https://www.uniprot.org/docs/dbxref)) flagged to represent 18 categories of databases ([https://www.uniprot.org/database/](https://www.uniprot.org/database/)) for better utility. The NCBI-GenBank db\_xrefs list was developed in 1997 ([https://doi.org/10.1038/ng0497-339](https://doi.org/10.1038/ng0497-339)), however it has only 129 databases listed to date ([https://www.ncbi.nlm.nih.gov/genbank/collab/db\_xref/](https://www.ncbi.nlm.nih.gov/genbank/collab/db_xref/)). In addition, identifiers.org ([https://identifiers.org/](https://identifiers.org/)) provides a free service for looking up and referencing a data ID to one of the 714 pre-curated life science database locations using Compact Identifiers syntax. For example, the uniform resource identifier (URI) [http://identifiers.org/pdb/2gc4](http://identifiers.org/pdb/2gc4) can be instantly forwarded to [https://www.rcsb.org/structure/2gc4](https://www.rcsb.org/structure/2gc4). The syntax of Compact Identifiers includes three parts: a provider code, a namespace prefix, and an accession. The provider code and namespace prefix are manually curated and stable, and can be easily looked up at its web site. Note that the databases represented by these four resources may overlap.
  - **Best practices**: The `dbxref` must refer to the same entity (not related information) in an external database. The format of a dbxref record may take the form `dbxref=database:identifier`, where 'database' is an abbreviated database name registered on a known dbxref list (above), affixed without space with a specific path leading to the database agent that accepts the 'identifier' for information retrieval. The 'database:identifier' construct is called unique resource identifier (URI). The URI must be specific to a record. Both dbxref record and the resolved URL should be a continuous string compliant to [RFC-3986](https://datatracker.ietf.org/doc/rfc3986/). The composed URL must be unique and exist. Multiple dbxref items may be allowed for one GFF record. When the 'database' abbreviation and path exists in a non-publicly available dbxref resource, it must be specified in a README file attached to the GFF file. We recommend using identifiers.org for identifier resolution.
  - **Pragma**: 
    - Format: `dbxref=URI`; where URI is concatenated (without space) with the follow components: `Protocol://domain name/ + [namespace] + identifier`
    - Database short name is translated to an URL (protocol://domain name);
    - Name space is the path to the identifier handler;
    - Identifier: unique accession of an entity.
  - **Validation**: There may be a challenge when writing the validators, given that there are multiple independently maintained lists, and there may be lags in information updates. The validator could check whether a HTTP response status code 200 is returned based on the url built from the dbxref registry in the directive. Semicolons (`;;`) are not permitted in the URL as it is used as the delimiter between column 9 `name=value` pairs.
  - **Example**: [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.3ylh08mnzm75](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.3ylh08mnzm75)


Attributes : Derives_from
==========================
**Column 9**
  - **Change level:** recommendation
  - **Summary:** The most common use for the Derives\_from attribute is to describe the relationship between CDS and polypeptide features. However, 1) not all software recognizes this relationship, and 2) we do not recommend modeling polypeptide features in GFF3 (see recommendations for &#39;Modeling hierarchical relationships of a protein-coding gene&#39;). Avoid modeling polypeptide features in general to prevent downstream interpretation problems of Derives\_from.
  - **Proposed changes to specification:** None.
  - **Rationale:** The Derives\_from attribute ([http://purl.obolibrary.org/obo/RO\_0001000](http://purl.obolibrary.org/obo/RO_0001000)) is used in situations where the relationship between features is temporal, and therefore the part\_of relationship implied by the &#39;Parent&#39; attribute is not appropriate (e.g. polypeptides are derived from CDS features, or in the case of polycistronic genes). In practice most programs that consume or create GFF3 do not check whether implied part\_of relationships are actually valid per the Sequence Ontology (a notable exception is Genometools ([http://genometools.org/cgi-bin/gff3validator.cgi](http://genometools.org/cgi-bin/gff3validator.cgi))).
  - **Best practices:** To avoid breaking software that consumes GFF3, we recommend not specifying a polypeptide feature if you&#39;re modeling a typical protein-coding gene based on genomic coordinates (see also recommendations for &#39;Modeling hierarchical relationships of a protein-coding gene&#39; below).
  - **Validation:** This needs more analysis and discussion.
  - **Example:** [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.l7o3yeprwedb](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.l7o3yeprwedb)


Attributes : Note
=================
**Column 9**
  - **Change level:** No change
  - **Summary:** No changes relative to the original definition.
  - **Proposed changes to specification** : None
  - **Rationale:** Sometimes the Note attribute can contain irrelevant information.
  - **Best practices:** Use primarily for notes relevant for public consumption (e.g., important errata for downstream users). Use as a last resort and at your own risk. In some cases a custom tag may be more appropriate. A common use-case would be for curation notes relevant to external users.
  - **Validation:** May not include a semicolon. May be repeated.
  - **Example:**

...;Note=This was a gene split from pax6;Note=Version 6.3a published on Sep 15, 2020


Attributes : Ontology\_term
===========================
**Column 9**
  - **Change level:** Recommendation only.
  - **Summary:** Avoid.
  - **Proposed changes to specification** : None
  - **Rationale:** See section about functional annotation and metadata below.
  - **Best practices:** Avoid. In general, do not use gff3 for functional annotation if possible. Use GAF, GPAD or other instead. If you do have to use it, use a CURIE backed ontology term (e.g. GO:0000077). In general, supplying an ontology term without underlying evidence, reference, or other context will be under-defined functional annotation and will be incomplete for downstream users and of little utility.
  - **Validation:** Should be a CURIE that resolves into a published ontology term. The validator should issue a warning that Ontology\_term should be avoided.
  - **Example:** Ontology\_term=&quot;GO:0046703&quot; (although note that evidence code should also be included; see discussion on &quot;complex metadata&quot;)


Attributes : Target, Gap
========================
**Column 9**
  - **Change level:** Recommendation only.
  - **Summary:** Target is an attribute intended to encode a parseable relationship between a region on the sequence given in column 1 and a region on another sequence, possibly (but not necessarily) another sequence referenced in column 1 of another record in the same gff file. Gap is an associated attribute encoding the alignment (i.e. gapping structure) needed to put the elements of the two sequences into homologous correspondence.
  - **Proposed changes to specification** : None
  - **Rationale:** The GFF3 specification describes the purpose of these two attributes as being for the representation of sequence alignments. Since the time that specification was written, numerous alternative formats for storing sequence alignments have been developed in response to the proliferation of sequence data brought about by the advent of next generation sequencing technologies. Therefore, although there may be cases in which the use of such attributes to represent alignments in the context of GFF files is convenient for a specific purpose (e.g. a gene prediction program seeking to represent the evidence behind its models), in general we advocate the use of alternative file formats such as BAM or PAF for representing alignments between sequences.
  - There are additional contexts in which the representation of homology between regions on sequences is desirable without the fine grained structure of base pair correspondence. For example, syntenic relationships (SO:0005858) between regions of chromosomes that are derived from collinear blocks of homologous genes may be computed without having the full details of the alignment of the genomic regions. In this case, since the relationship is likely going to be an inter-genomic comparison (e.g. between species), it is important to be able to represent the information as a single record bearing the information about the paired genomic regions. Target can be used in such cases to represent the relationship, following the space-delimited structure suggested in the GFF3 specification &quot;target\_id start end [strand]&quot;. We note that although the GFF3 specification indicates that spaces in the target\_id must be hex-encoded, we would recommend that the target\_id represent a sequence identifier such as would be found in Column 1 of the same (or another) GFF3 file containing annotations of the target sequence.
  - **Best practices:** We advocate the use of alternative file formats such as BAM or PAF for representing alignments between sequences.
  - **Validation:** The components of the encoded attribute must be single-space delimited and must consist of not less than 3 and not more than 4 fields. Field 1 must conform to the same syntax and semantics as specified for Column 1 (seqid). Fields 2 and 3 must conform to the syntax and semantics for Columns 4 and 5 (start and stop) respectively. Field 4 is optional and conforms to the syntax and semantics of Column 7 (strand), though a missing value will be indicated by absence of the field rather than using &quot;.&quot;
  - **Example:**

(representing synteny)

glyma.Wm82.gnm2.ann1.Gm01 DAGchainer syntenic\_region 227021 926129 1243.0 + . **Target=glyma.Wm82.gnm2.ann1.Gm08 2159073 2752670 +** ; median\_Ks=0.7982

(representing sequence-level alignment)

scaffold\_44 blastn match\_part 36730 38665 1758 - . ID=33;Parent=32; **Target=scaffold\_18G19.1 1 1934 +;Gap=M884 D1 M947 D1 M103**


Attributes complex metadata / functional annotations
====================================================
**Column 9**
  - **Change level:** Major change
  - **Summary:** In some instances we may need to model complex sets of metadata within the GFF3.
  - **Proposed changes to specification:**
    - Provides a format for including richer metadata.
  - **Rationale:** In general should be avoided if other mechanisms ([GPAD](http://geneontology.org/docs/gene-product-association-data-gpad-format/), [GPI](http://geneontology.org/docs/gene-product-information-gpi-format/), [all spec formats and versions](https://github.com/geneontology/go-annotation/tree/master/specs) ) are available. However, in some instances GPAD, or other formats are not sufficient or readily available for tooling. In those instances, the ability to include information such as functional annotations that track annotation provenance, gene products, or properly annotated GO evidence may be necessary.
  - **Best practices:**
  - We discourage the use of GO terms, or any functional annotation that requires an evidence code without supplying the evidence code.

  - These use of GO term or functional annotations should never be incorporated into gff within the Dbxref or Ontology\_term fields. Another file format should be used, e.g. GAF or GPAD, if possible, otherwise modeling using \&lt;complex metadata\&gt; is recommended.
  - Also, GO terms should be updated annually - you might not want to include this as &#39;static&#39; information.
  - In the context where metadata such as functional annotations must be included in GFF3 column 9, the general format we would suggest and has been adopted in Apollo and Artemis for GO (go\_annotations), Gene Product (gene\_product), and Provenance (provenance) annotations is \&lt;type\&gt;=\&lt;type annotation\&gt;; **.** Each type is only included once, but can include multiple type annotations. Note that \&lt;type annotation\&gt; is URL encoded **.**
  - \&lt;type\_annotations\&gt; are URL encoded and of the format: rank=\&lt;rankA\&gt;;\&lt;key1\&gt;=\&lt;value1A\&gt;;\&lt;key2\&gt;=\&lt;value2A\&gt;,rank=\&lt;rankB\&gt;;\&lt;key1\&gt;=\&lt;value1B\&gt;;\&lt;key2\&gt;=\&lt;value2B\&gt; If we have multiple annotations, we provide a rank to indicate which would go first, though this may not always be relevant. Multiple annotations are comma-delimited. Multiple key/value pairs are semicolon delimited.
- **Validation:**
  - \&lt;type\&gt; should be lower-case and be of the form \&lt;type1\&gt;=\&lt;type1 annotations\&gt;;\&lt;type2\&gt;=\&lt;type2 annotations\&gt;; etc.
  - \&lt;type annotation\&gt; entries are URL encoded
  - A \&lt;type\&gt; can have multiple \&lt;type annotations\&gt;, which are separated by a comma (url-encoded %3B).
  - Each \&lt;type annotation\&gt; has multiple key-value pairs, separated by a semi-colon (url-encoded as %2C).
  - \&lt;rank\&gt; is not necessary, but it is preferred if more than one annotation for a type exists.
  - \&lt;type\&gt;, \&lt;rank\&gt;, \&lt;key\&gt; should all be lower-case.
- **Example:** [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.h4b9kbtmxne6](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.h4b9kbtmxne6)


# Modeling hierarchical relationships of a protein-coding gene

  - **Change level:** Recommendation only
  - **Summary:** The primary purpose of GFF3 is to model gene structure.
  - **Rationale:** We wish to provide a standard way to render this type of information as there are many valid ways to render the same protein-coding gene.
  - **Proposed changes to specification:** None
  - **Best practices:**

    - Strongly encourage only one parent per feature. However, parsers and validators should still support multiple parents per feature, in particular for elegance and backwards compatibility and to support more &quot;non-standard&quot; protein structures especially within non-eukaryotic organisms.
    - Edge cases that can&#39;t follow this standard recommendation exist, e.g. ribosome slippage, trans-splicing, features split across scaffolds due to assembly problems - further recommendations need to be developed.
    - Sort order. This pertains to having child features come after parent features in the gff; most loaders don&#39;t care about order of the features by coordinates. Child features should be listed after parent features.
    - Child coordinates that are not contained within parent coordinates often indicate an error and should trigger a warning in a gff3 validator.
    - The gff3 format assumes that parent and child features have a part\_of relationship type.
    - There can be a ### directive between gene models.
    - Do not list multiple values in column 1 (for features split across scaffolds)
    - Polypeptide features are not required or recommended
    - Introns can be annotated, but are not necessary and are implied.
    - Type should be specified and validated as part of the sequence ontology cv terms (see also notes on column 3, type, above): http://www.sequenceontology.org/miso
  - **Validation:**

- Entries that are non-parent entries should have a valid parent entry via the ID.
- IDs should be internally resolvable.
- **Example:** [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.k29nmx3hdkik](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.k29nmx3hdkik)


Pragmas
=======

  - **Seqid alias table**
    - Used to cross reference multiple naming schemes for seqid values, for example, to correlate project chromosome names with NCBI accessions. Starts with ##alias-table [columns] and ends with ###. See [https://docs.google.com/document/d/180g1rfC5n\_cR6sioG\_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.tr1vioe7doqb](https://docs.google.com/document/d/180g1rfC5n_cR6sioG_LFGaUPNmQyDqTsPafVu4gM018/edit#bookmark=id.tr1vioe7doqb).

- **Source.**
  - This pragma is optional.
  - The current SO recommendation is to specify the ontology of source names. However, no such ontology exists (yet).
  - Instead, we suggest the following change. Follow the VCF specification for Info/ID: [https://samtools.github.io/hts-specs/VCFv4.3.pdf](https://samtools.github.io/hts-specs/VCFv4.3.pdf)
  - Example: ##Source=\&lt;ID=BestRefSeq,Description=&quot;RefSeq transcript that underwent manual inspection&quot;, Software=&quot;URL here&quot;\&gt;
  - Primarily for human consumption - no validation at this point.
  - We discourage verbose use of this pragma. However, since it won&#39;t be validated, it is possible.
- **Score**
  - This pragma is optional.
  - Format: ##Score name=&quot;[name/calculated-by]&quot;;min=[min-value]; max=[max-val];best=[lower/higher]
  - All parameters (_name_, _min_, _max_, and _best)_ are optional, but at least one should be set.
  - Example: ##Score name=&quot;AED (Annotation Edit Distance) score&quot;; min=0;max=1;best=lower
- **Phase.**
  - This pragma is optional. It is designed to handle reference sequences that do not use the standard genetic code (id = 1, see [https://www.ncbi.nlm.nih.gov/IEB/ToolBox/C\_DOC/lxr/source/data/gc.prt#0107](https://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/source/data/gc.prt#0107))
  - Format: ##Phase \&lt;RefSeq ID\&gt; \&lt;translation table ID\&gt;;
  - Example (invertebrate mitochondrial sequence in genome assembly GCF\_013339725.1; all other sequences follow the standard genetic code): ##Phase \&lt;NC\_023335.1\&gt; \&lt;5\&gt;
- **Dbxref**
  - **This pragma is optional**
  - **Format: ##dbxref=\&lt;Namespace:ID,refsrc=URL\&gt;**
  - **Example: ##dbxref=ncbiprotein:CAA71118.1,refsrc= identifiers.org; (which resolves to** **[https://identifiers.org/ncbiprotein:CAA71118.](https://identifiers.org/ncbiprotein:CAA71118.1)[1](https://identifiers.org/ncbiprotein:CAA71118.1)****)**
  - **Addendum: This requires that the database providing the xref register at and obtain a namespace from Identifiers.org.**
- **Ontology URIs** 
  - This pragma is optional.
  - Format: ## Ontology <URL>
  - Example: ## Ontology [http://purl.obolibrary.org/obo/so.obo](http://purl.obolibrary.org/obo/so.obo)
  - The ontology URL should be the official OBO version permanent URL in unencoded format. 
- **Species** - don&#39;t use URLs as in the spec, as these can vary. Use an OBO CURIE. E.g. NCBITaxon:9606


# Other caveats and unresolved issues

  - We need recommendations/standards for fasta deflines. See
    - [https://www.ncbi.nlm.nih.gov/Sequin/modifiers.html](https://www.ncbi.nlm.nih.gov/Sequin/modifiers.html)
    - [https://www.uniprot.org/help/fasta-headers](https://www.uniprot.org/help/fasta-headers)
    - [https://ftp.ncbi.nlm.nih.gov/genomes/genbank/vertebrate\_mammalian/Homo\_sapiens/all\_assembly\_versions/GCA\_000001405.15\_GRCh38/seqs\_for\_alignment\_pipelines.ucsc\_ids/](https://ftp.ncbi.nlm.nih.gov/genomes/genbank/vertebrate_mammalian/Homo_sapiens/all_assembly_versions/GCA_000001405.15_GRCh38/seqs_for_alignment_pipelines.ucsc_ids/)
  - Recommendations for modeling QTL data in gff.
  - Recommendations for modeling miRNAs.
  - How to handle gene models that are split across reference sequences.
  - There are a number of unresolved issues on the gff3 spec that could be worth addressing: [https://github.com/The-Sequence-Ontology/Specifications/issues](https://github.com/The-Sequence-Ontology/Specifications/issues)
  - Discuss adding attributes to address genome context relevant to noted aberrations in genome structure in pan-genomes
  - While the GFF file can be used for the representation of genetic objects on a common coordinate system without regard to the actual coordinate system of the object, the GFF specification is designed to represent genetic objects in their native coordinate system. Therefore, any use of the GFF file for other purposes is not covered in the specification.
  - Identify a standard way of linking to resources that contain information related to what is contained in a GFF. The most obvious case would be for allowing explicit associations to fasta files; alignments externally might provide another case
  - Discuss adding pragma with workflow provenance including functional annotation tool (e.g. Prokka), which implies evidence and GO ref. This would get around having a separate GAF file, or using the &#39;complex metadata&#39; format.

Examples
========



**Seqid (Column 1)**

#!alias-table [https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/005/005/GCF\_000005005.2\_B73\_RefGen\_v4/GCF\_000005005.2\_B73\_RefGen\_v4\_assembly\_report.txt](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/005/005/GCF_000005005.2_B73_RefGen_v4/GCF_000005005.2_B73_RefGen_v4_assembly_report.txt)

**Source (column 2)**

gene

##gff-version 3

##Source=ID=maker\_ITAG, Description=&quot;Solgenomics (SGN) tomato genome annotation pipeline based on Maker, Mikado and AHRD. Details in methods of genome publication&quot;, Software=&quot;https://doi.org/10.1101/767764&quot;

SL4.0ch00 maker\_ITAG gene 93750 94430 . + . ID=gene:Solyc00g500001.1;Alias=Solyc00g500001;Name=Solyc00g500001.1

mRNA

##gff-version 3

##Source=ID=maker\_ITAG, Description=&quot;Solgenomics (SGN) tomato genome annotation pipeline based on Maker, Mikado and AHRD. Details in methods of genome publication&quot;, Software=&quot;https://doi.org/10.1101/767764&quot;

SL4.0ch00 maker\_ITAG mRNA 93750 94430 . + .

ID=mRNA:Solyc00g500001.1.1;Parent=gene:Solyc00g500001.1;Name=Solyc00g500001.1.1;Note=Retrovirus-related Pol polyprotein from transposon TNT 1-94

**Type (column 3, example is from a RefSeq gff3 file)**

NW\_023276341.1 Gnomon gene 52942 57885 . - . ID=gene-LOC118063598;Dbxref=GeneID:118063598;Name=LOC118063598;gbkey=Gene;gene=LOC118063598;gene\_biotype=protein\_coding

NW\_023276341.1 Gnomon mRNA 52942 57885 . - . ID=rna-XM\_035081708.1;Parent=gene-LOC118063598;Dbxref=GeneID:118063598,Genbank:XM\_035081708.1;Name=XM\_035081708.1;gbkey=mRNA;gene=LOC118063598;model\_evidence=Supporting evidence includes similarity to: 2 Proteins%2C and 100%25 coverage of the annotated genomic feature by RNAseq alignments%2C including 5 samples with support for all annotated introns;product=uncharacterized LOC118063598;transcript\_id=XM\_035081708.1

NW\_023276341.1 Gnomon exon 57433 57885 . - . ID=exon-XM\_035081708.1-1;Parent=rna-XM\_035081708.1;Dbxref=GeneID:118063598,Genbank:XM\_035081708.1;gbkey=mRNA;gene=LOC118063598;product=uncharacterized LOC118063598;transcript\_id=XM\_035081708.1

NW\_023276341.1 Gnomon exon 52942 57315 . - . ID=exon-XM\_035081708.1-2;Parent=rna-XM\_035081708.1;Dbxref=GeneID:118063598,Genbank:XM\_035081708.1;gbkey=mRNA;gene=LOC118063598;product=uncharacterized LOC118063598;transcript\_id=XM\_035081708.1

NW\_023276341.1 Gnomon CDS 53079 57215 . - 0 ID=cds-XP\_034937599.1;Parent=rna-XM\_035081708.1;Dbxref=GeneID:118063598,Genbank:XP\_034937599.1;Name=XP\_034937599.1;gbkey=CDS;gene=LOC118063598;product=uncharacterized protein LOC118063598;protein\_id=XP\_034937599.1

**Start, end (column 4,5)**

mRNA

##gff-version 3

# organism zea mays

Chr1 gramene mRNA 44289 49837 . + . ID=420346;Name=Zm00001d027230\_T001;Parent=4711

Bacterial CDS (taken from the SO specification)

##gff-version 3.1.26

# organism Enterobacteria phage f1

# Note Bacteriophage f1, complete genome.

J02448 GenBank region 1 6407 . + . ID=J02448;Name=J02448;Is\_circular=true;

J02448 GenBank CDS 6006 7238 . + 0 ID=geneII;Name=II;Note=protein II;

SNP

##gff-version 3

##date Fri Dec 11 11:41:25 2020

# organism Arachis ipaensis

Araip.B01 PolymorphicArray SNP 2975884 2975884 . . . Name=AX-176823085;ID=21305;origin=A.ipaensis;alleles=A%2FG

**Score (column 6)**

Format:

#!score score-name program program-version score-range decreases|increases.

Example: score is the AED (Annotation Edit Distance) for a gene model feature, generated by MAKER-P.

##gff-version 3

# organism zea mays

#!score AED MAKER-P 3.0 0-1 increases

chr1 NAM mRNA 107080 108196 **0.38** - . ID=45221;Parent=Zm00001e000004;transcript\_id=Zm00001e000004\_T001

**Phase (column 8).**

In this example, the phase is incorrect, but the correct sequence can be calculated by finding the longest ORF.

QKKF01020412.1 EVM gene 1371 1574 . + . ID=evm.TU.Contig10112.1;Name=EVM prediction Contig10112.1

QKKF01020412.1 EVM mRNA 1371 1574 . + . ID=evm.model.Contig10112.1;Parent=evm.TU.Contig10112.1;Name=EVM prediction Contig10112.1

QKKF01020412.1 EVM exon 1371 1574 . + . ID=evm.model.Contig10112.1.exon1;Parent=evm.model.Contig10112.1

QKKF01020412.1 EVM CDS 1371 1574 . + 1 ID=cds.evm.model.Contig10112.1;Parent=evm.model.Contig10112.1

Here are the CDS and protein sequence calculated using GFF3 phase 1, which is incorrect (as seen by the premature stop codons in the protein sequence):

\&gt;cds.evm.model.Contig10112.1

AGCTCGGGTGGTAATGGCATGTCGCAATTTGGAAAAAGCGGACGAGGCGGCCAAAGATATAAGGAAAACGCTGGAAGGGGTTGAAGGTGTAGGACAAATCACTGTGAAGCATCTCGATCTGTCATCATTGTCATCTGTCAGAACCTGTGCCGAACAACTTCTCAAAGAAGAACCAAACATACATTTATTGATTAACAATGCTG

\&gt;evm.model.Contig10112.1

SSGGNGMSQFGKSGRGGQRYKENAGRG\*RCRTNHCEASRSVIIVICQNLCRTTSQRRTKHTFID\*QC

Here are the correct CDS and protein sequence, calculated by finding the longest ORF.

\&gt;cds.evm.model.Contig10112.1

GAGCTCGGGTGGTAATGGCATGTCGCAATTTGGAAAAAGCGGACGAGGCGGCCAAAGATATAAGGAAAACGCTGGAAGGGGTTGAAGGTGTAGGACAAATCACTGTGAAGCATCTCGATCTGTCATCATTGTCATCTGTCAGAACCTGTGCCGAACAACTTCTCAAAGAAGAACCAAACATACATTTATTGATTAACAATGCTG

\&gt;evm.model.Contig10112.1

ARVVMACRNLEKADEAAKDIRKTLEGVEGVGQITVKHLDLSSLSSVRTCAEQLLKEEPNIHLLINNA

The correct gff:

QKKF01020412.1 EVM gene 1371 1574 . + . ID=evm.TU.Contig10112.1;Name=EVM prediction Contig10112.1

QKKF01020412.1 EVM mRNA 1371 1574 . + . ID=evm.model.Contig10112.1;Parent=evm.TU.Contig10112.1;Name=EVM prediction Contig10112.1

QKKF01020412.1 EVM exon 1371 1574 . + . ID=evm.model.Contig10112.1.exon1;Parent=evm.model.Contig10112.1

QKKF01020412.1 EVM CDS 1371 1574 . + 0 ID=cds.evm.model.Contig10112.1;Parent=evm.model.Contig10112.1

Correct CDS and protein sequence, calculated from GFF and genome fasta:

\&gt;cds.evm.model.Contig10112.1

GAGCTCGGGTGGTAATGGCATGTCGCAATTTGGAAAAAGCGGACGAGGCGGCCAAAGATATAAGGAAAACGCTGGAAGGGGTTGAAGGTGTAGGACAAATCACTGTGAAGCATCTCGATCTGTCATCATTGTCATCTGTCAGAACCTGTGCCGAACAACTTCTCAAAGAAGAACCAAACATACATTTATTGATTAACAATGCTG

\&gt;evm.model.Contig10112.1

ARVVMACRNLEKADEAAKDIRKTLEGVEGVGQITVKHLDLSSLSSVRTCAEQLLKEEPNIHLLINNA

**Phase pragma (column 8).**

#!Translation-table 5 scaffold1,scaffold2

Where &#39;5&#39; refers to the genetic code id specified in [https://www.ncbi.nlm.nih.gov/IEB/ToolBox/C\_DOC/lxr/source/data/gc.prt](https://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/source/data/gc.prt). The example here, 5, refers to the invertebrate mitochondrial code, which can also be found here: [https://www.ncbi.nlm.nih.gov/IEB/ToolBox/C\_DOC/lxr/source/data/gc.prt#0150](https://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/source/data/gc.prt#0150).

Where scaffold1,scaffold2 refer to the reference sequences that should use Translation-table 5.

**Attributes (column 9): ID (example is from a RefSeq gff3 file):**

NW\_023276341.1 Gnomon gene 52942 57885 . - . ID=gene-LOC118063598;Dbxref=GeneID:118063598;Name=LOC118063598;gbkey=Gene;gene=LOC118063598;gene\_biotype=protein\_coding

NW\_023276341.1 Gnomon mRNA 52942 57885 . - . ID=rna-XM\_035081708.1;Parent=gene-LOC118063598;Dbxref=GeneID:118063598,Genbank:XM\_035081708.1;Name=XM\_035081708.1;gbkey=mRNA;gene=LOC118063598;model\_evidence=Supporting evidence includes similarity to: 2 Proteins%2C and 100%25 coverage of the annotated genomic feature by RNAseq alignments%2C including 5 samples with support for all annotated introns;product=uncharacterized LOC118063598;transcript\_id=XM\_035081708.1

NW\_023276341.1 Gnomon exon 57433 57885 . - . ID=exon-XM\_035081708.1-1;Parent=rna-XM\_035081708.1;Dbxref=GeneID:118063598,Genbank:XM\_035081708.1;gbkey=mRNA;gene=LOC118063598;product=uncharacterized LOC118063598;transcript\_id=XM\_035081708.1

NW\_023276341.1 Gnomon exon 52942 57315 . - . ID=exon-XM\_035081708.1-2;Parent=rna-XM\_035081708.1;Dbxref=GeneID:118063598,Genbank:XM\_035081708.1;gbkey=mRNA;gene=LOC118063598;product=uncharacterized LOC118063598;transcript\_id=XM\_035081708.1

NW\_023276341.1 Gnomon CDS 53079 57215 . - 0 ID=cds-XP\_034937599.1;Parent=rna-XM\_035081708.1;Dbxref=GeneID:118063598,Genbank:XP\_034937599.1;Name=XP\_034937599.1;gbkey=CDS;gene=LOC118063598;product=uncharacterized protein LOC118063598;protein\_id=XP\_034937599.1

**Attributes (col 9): Name**

KZ308124.1 ladful\_OGSv1.0 mRNA 2340580 2344890 . + . ID=LFUL019771-RA;Name=Odorant receptor 1;Parent=LFUL019771;product=Odorant receptor 1;transcript\_id=gnl|J437|LFUL019771-RA;

**Attributes (col 9): Alias**

In the case of pax6a ([http://zfin.org/ZDB-GENE-990415-200](http://zfin.org/ZDB-GENE-990415-200)) there are multiple &quot;aliased names&quot; associated with it under previous names: pax-a Pax6.1 pax6 pax[zf-a] (1) paxzfa zfpax-6a cb280 (1) etID309716.25 (1) fc20e07 wu:fc20e07 (1) zfpax-6b

##gff-version 3

25 ZFIN gene 15029041 15049781 1 - . ID=403398;Name=pax6a;gene\_id=ZDB-GENE-990415-200;Alias=pax-a, Pax6.1, pax[zf-a], paxzfa,...

25 ZFIN mRNA 15029041 15049694 . - . ID=403399;Name=pax6a-204;Parent=403398

**Attributes (col 9): Dbxref**

- SNP example:
  - db\_xref=dbSNP:rs133073; (resolves to [https://www.ncbi.nlm.nih.gov/snp/rs133073?horizontal\_tab=true](https://www.ncbi.nlm.nih.gov/snp/rs133073?horizontal_tab=true) via [http://identifiers.org/dbSNP:rs133073](http://identifiers.org/dbSNP:rs133073))
- Protein-coding gene example:
  - Dbxref=MaizeGDB.locus:12098 (resolves to [https://www.maizegdb.org/gene\_center/gene/12098](https://www.maizegdb.org/gene_center/gene/12098) via [https://identifiers.org/maizegdb.locus:12098](https://identifiers.org/maizegdb.locus:12098))

​​

**Attributes (col 9): Derives\_from**

Using derives\_from for polypeptide features in a gene model (if you have to, modified from GFF3 spec)

chrX . gene XXXX YYYY . + . ID=gene01;name=resA

chrX . mRNA XXXX YYYY . + . ID=tran01;Parent=gene01

chrX . exon XXXX YYYY . + . Parent=tran01

chrX . CDS XXXX YYYY . + . ID=cds01;Parent=tran01

chrX . polypeptide XXXX YYYY . + . ID=poly01;Derives\_from=cds01

Using derives\_from for polycistronic genes (copied from GFF3 spec)

chrX . gene XXXX YYYY . + . ID=gene01;name=resA

chrX . gene XXXX YYYY . + . ID=gene02;name=resB

chrX . gene XXXX YYYY . + . ID=gene03;name=resX

chrX . gene XXXX YYYY . + . ID=gene04;name=resZ

chrX . mRNA XXXX YYYY . + . ID=tran01;Parent=gene01,gene02,gene03,gene04

chrX . exon XXXX YYYY . + . ID=exon00001;Parent=tran01

chrX . CDS XXXX YYYY . + . Parent=tran01;Derives\_from=gene01

chrX . CDS XXXX YYYY . + . Parent=tran01;Derives\_from=gene02

chrX . CDS XXXX YYYY . + . Parent=tran01;Derives\_from=gene03

chrX . CDS XXXX YYYY . + . Parent=tran01;Derives\_from=gene04

**Attributes (col 9): Note,**

**Attributes (col 9): Ontology\_term**

**Attributes (col 9): Target**

**Attributes (col 9): Gap**

**Complex metadata**

chr9 . mRNA 99185587 99185866 . - . owner=demo@demo.com;**provenance=rank%3D1%3Bfield%3DDESCRIPTION%3Bdb\_xref%3D:%3Bevidence%3DECO:0000501%3Bnote%3D[&quot;Description of provenance of an individual field within an annotation. &quot;]%3Bbased\_on%3D[]%3Blast\_updated%3D2021-01-11 16:39:32.454%3Bdate\_created%3D2021-01-11 16:39:32.454;**Parent=c9637c84-1c18-4320-8c58-7277ef768fd9;**go\_annotations=rank%3D1%3Baspect%3DMF%3Bterm%3DGO:0004381%3Bdb\_xref%3DPMID:171711%3Bevidence%3DECO:0000315%3Bgene\_product\_relationship%3DRO:0002327%3Bnegate%3Dfalse%3Bnote%3D[&quot;This is a made up example.&quot;]%3Bbased\_on%3D[&quot;UniProt:`123141&quot;]%3Blast\_updated%3D2021-01-11 16:45:45.133%3Bdate\_created%3D2021-01-11 16:45:45.133%2Crank%3D2%3Baspect%3DBP%3Bterm%3DGO:0000077%3Bdb\_xref%3DAspGD\_REF:ASPL0000000005%3Bevidence%3DECO:0000501%3Bgene\_product\_relationship%3DRO:0002331%3Bnegate%3Dfalse%3Bnote%3D[&quot;This was pulled from AMIGO: http://amigo.geneontology.org/amigo/gene\_product/AspGD:ASPL0000108267&quot;%2C&quot;This is an example GO functional annotation created within Apollo&quot;]%3Bbased\_on%3D[&quot;SGD:S000002848&quot;%2C&quot;UniProt:11771&quot;]%3Blast\_updated%3D2021-01-11 16:37:58.871%3Bdate\_created%3D2021-01-11 16:35:41.092;****gene\_product=rank%3D1%3Bterm%3DAfu5g12110%3Bdb\_xref%3DAspGD\_REF:ASPL0000000005%09%3Bevidence%3DECO:0000501%3Balternate%3Dfalse%3Bnote%3D[&quot;Created from http://amigo.geneontology.org/amigo/gene\_product/AspGD:ASPL0000108267&quot;]%3Bbased\_on%3D[&quot;SGD:S000002848\t&quot;]%3Blast\_updated%3D2021-01-11 16:38:52.342%3Bdate\_created%3D2021-01-11 16:38:52.342;ID=977dcd44-5d57-4eea-9127-a784b2400f3f;**orig\_id=transcript:ENST00000469816;date\_last\_modified=2021-01-11;Name=RN7SL794P-201-00001;date\_creation=2020-07-23

**Modeling hierarchical relationships of a protein-coding gene**

##gff-version 3

##sequence-region chr9 1 138394717

chr9 . gene 99206226 99222116 . - . owner=demo@demo.com;ID=98674a30-c148-4d05-8b17-53e3d09645ac;date\_last\_modified=2020-04-09;Name=ALG2-002a;date\_creation=2019-11-05

chr9 . mRNA 99206226 99222116 . - . owner=demo@demo.com;Parent=98674a30-c148-4d05-8b17-53e3d09645ac;ID=7dd2784f-6357-4852-9c9b-21d8483255d8;orig\_id=transcript:ENST00000476832;date\_last\_modified=2020-03-02;Name=ALG2-002a-00001;date\_creation=2019-10-25

chr9 . CDS 99217934 99218680 . - 0 Parent=7dd2784f-6357-4852-9c9b-21d8483255d8;ID=b0fc5d5b-65e6-41a0-a65e-7afadfe1d8d1;Name=b0fc5d5b-65e6-41a0-a65e-7afadfe1d8d1

chr9 . exon 99221130 99222116 . - . Parent=7dd2784f-6357-4852-9c9b-21d8483255d8;ID=7aa73c7e-bb44-4e86-aeb3-921340d60e45;Name=7aa73c7e-bb44-4e86-aeb3-921340d60e45

chr9 . exon 99206226 99218846 . - . Parent=7dd2784f-6357-4852-9c9b-21d8483255d8;ID=9baa5521-2f79-4493-9435-0120aa4f96d0;Name=9baa5521-2f79-4493-9435-0120aa4f96d0

chr9 . mRNA 99217367 99218836 . - . owner=demo@demo.com;Parent=98674a30-c148-4d05-8b17-53e3d09645ac;ID=6eebcdae-fb8b-42af-a178-f70a0e8ff5da;orig\_id=transcript:ENST00000476832;date\_last\_modified=2020-04-09;Name=ALG2-002a-00003;date\_creation=2020-04-09

chr9 . CDS 99217934 99218836 . - 0 Parent=6eebcdae-fb8b-42af-a178-f70a0e8ff5da;ID=6eebcdae-fb8b-42af-a178-f70a0e8ff5da-CDS;Name=6eebcdae-fb8b-42af-a178-f70a0e8ff5da-CDS

chr9 . exon 99217367 99218836 . - . Parent=6eebcdae-fb8b-42af-a178-f70a0e8ff5da;ID=cbf89b7d-32de-40a0-be12-1332afe2a527;Name=cbf89b7d-32de-40a0-be12-1332afe2a527

chr9 . mRNA 99217372 99218836 . - . owner=demo@demo.com;Parent=98674a30-c148-4d05-8b17-53e3d09645ac;ID=3da41f24-d14b-4f01-8e27-c15f86912acb;orig\_id=transcript:ENST00000319033;date\_last\_modified=2020-04-09;Name=ALG2-002a-00004;date\_creation=2020-04-09

chr9 . exon 99217372 99218836 . - . Parent=3da41f24-d14b-4f01-8e27-c15f86912acb;ID=b5adb98d-82c9-4b5b-a320-1c044bced6e0;Name=b5adb98d-82c9-4b5b-a320-1c044bced6e0

chr9 . CDS 99217934 99218836 . - 0 Parent=3da41f24-d14b-4f01-8e27-c15f86912acb;ID=3da41f24-d14b-4f01-8e27-c15f86912acb-CDS;Name=3da41f24-d14b-4f01-8e27-c15f86912acb-CDS

chr9 . mRNA 99217367 99221956 . - . owner=demo@demo.com;Parent=98674a30-c148-4d05-8b17-53e3d09645ac;ID=7cf3b8f6-7241-4145-9822-00bbb8fd4a87;orig\_id=transcript:ENST00000476832;date\_last\_modified=2020-04-08;Name=ALG2-002a-00002;date\_creation=2020-04-08

chr9 . CDS 99221547 99221954 . - 0 Parent=7cf3b8f6-7241-4145-9822-00bbb8fd4a87;ID=010f5cfb-3f84-4e74-9ad0-6d54c1463502;Name=010f5cfb-3f84-4e74-9ad0-6d54c1463502

chr9 . CDS 99217934 99218836 . - 0 Parent=7cf3b8f6-7241-4145-9822-00bbb8fd4a87;ID=010f5cfb-3f84-4e74-9ad0-6d54c1463502;Name=010f5cfb-3f84-4e74-9ad0-6d54c1463502

chr9 . exon 99221547 99221956 . - . Parent=7cf3b8f6-7241-4145-9822-00bbb8fd4a87;ID=58e7b74f-f60a-438f-a869-e8a0fc13bd3a;Name=58e7b74f-f60a-438f-a869-e8a0fc13bd3a

chr9 . exon 99217367 99218836 . - . Parent=7cf3b8f6-7241-4145-9822-00bbb8fd4a87;ID=d39ce049-51aa-48b4-a45b-ce8422b7693b;Name=d39ce049-51aa-48b4-a45b-ce8422b7693b

###

22
