# A Knowledge Graph of RxNORM and FDA drug data

This project creates a knowledge graph version of (a subset of) the RXNORM and the FDA databases. 

The graphical representation of the processing steps appears at: 
https://github.com/vlalcsci/RXNorm_FDA_Triples/blob/main/rxnorm_fda_kg/architecture/architecture_diagram.JPG


# **Step 0: Download RXNORM and FDA Data**
**RXNORM**:
1. Use this DATABASE creation automation script from RXNORM Technical Documentation: https://www.nlm.nih.gov/research/umls/rxnorm/docs/techdoc.html#s13_0
2. The RXNORM database files are available at: https://www.nlm.nih.gov/research/umls/rxnorm/docs/rxnormfiles.html
3. Convert the Resultant SQL Files to get required CSV Files 

**FDA**:
1. Use this JSON from OPENFDA Documentation to get required - https://api.fda.gov/download.json
2. Get the Required FDA files from: fda_json["results"]["drug"]["ndc"], fda_json["results"]["drug"]["label"], fda_json["results"]["drug"]["drugsfda"] & fda_json["results"]["drug"]["enforcement"]

The csv files for RXNORM and json files for FDA used in this project are at: 
https://drive.google.com/drive/folders/1oB-_gNqc29ZplYAQ5MlWyCRX2hXmR5y4


# **Step 1: Generate Intermediate RXNORM Triples**
# **1A. Generate Intermediate RXNORM Triples from RXNCONSO table**
RXNCONSO table provides the following information:<br>
*   RXNorm General Information: rxcui (RXNormID), name (label), tty (description), language (lat) and suppress (Y/N)
*   RXNorm Synonym Information: synonym (alias)
*   RXNorm Related Identifier Information: such as MSH, DRUGBANK and SNOMEDCT etc.

**RXNCONSO Headers for Reference**:<br>
RXCUI,LAT,TS,LUI,STT,SUI,ISPREF,RXAUI,SAUI,SCUI,SDUI,SAB,TTY,CODE,STR,SRL,SUPPRESS,CVF

**RXNorm General Information**:<br>
Example for Entresto sample record:<br>
1656341,ENG,,,,,,7249807,7249807.0,1656341,,RXNORM,BN,1656341,Entresto,,N,4096.0<br>
Subject: RXCUI (1656341) <br>
Predicates & Object: [Read as Predicate: Source Header (Object Value)]<br>
*   label: STR (Entresto)<br>
*   description: TTY (BN)<br>
*   language: LAT (ENG)<br>
*   suppress: SUPPRESS (N)<br>
*   rxcui: CODE (1656341) <br>

**RXNorm Synonym Information**:<br>
Example for Entresto sample record:<br>
3 entries- One Regular Information for Term Type SBD followed by 2 synonyms- PSN (Prescribable Name) and SY (Synonym)<br>
Regular Record:<br>
1656346,ENG,,,,,,7249812,7249812.0,1656346,,RXNORM,SBD,1656346,sacubitril 24 MG / valsartan 26 MG Oral Tablet [Entresto],,N,4096.0<br>
2 Synonym Records:<br>
1656346,ENG,,,,,,7249813,7249813.0,1656346,,RXNORM,PSN,1656346,Entresto 24 MG / 26 MG Oral Tablet,,N,4096.0
1656346,ENG,,,,,,7249814,7249814.0,1656346,,RXNORM,SY,1656346,Entresto (sacubitril 24 MG / valsartan 26 MG) Oral Tablet,,N,4096.0

Subject: RXCUI (1656346) <br>
Predicates & object:<br>
*   alias: STR (Entresto 24 MG / 26 MG Oral Tablet)<br>
*   alias: STR (Entresto (sacubitril 24 MG / valsartan 26 MG) Oral Tablet)<br>

**RXNorm Identifier Information**:<br>
Example for Entresto sample record:<br>
2 entries- One for MMSL and other for MSH:<br>
1656341,ENG,,,,,,7255921,,,,MMSL,BN,234762,Entresto,,N,
1656341,ENG,,,,,,8138471,,M000614616,C549068,MSH,PCE,C549068,entresto,,N,

Subject: RXCUI (1656346) <br>
Predicates & Object:<br>
*   MMSL: CODE (234762)<br>
*   MSH: CODE (C549068)<br>

# **1B. Generate Intermediate RXNORM Triples from RXNREL table**
RXNREL table provides the following information:<br>
*   RXNorm Relation Information: has_tradename, ingredient_of etc.<br>


**RXNREL Headers for Reference**:<br>
RXCUI1,RXAUI1,STYPE1,REL,RXCUI2,RXAUI2,STYPE2,RELA,RUI,SRUI,SAB,SL,RG,DIR,SUPPRESS,CVF<br>
Note: Relationship is what RXCUI2 HAS TO RXCUI1<br>

**RXNorm Relationship Information**:<br>
Example for Entresto sample record:<br>
1656355.0,,CUI,RO,1656341.0,,CUI,ingredient_of,86154613.0,,RXNORM,,,,,4096.0<br>
1656346.0,,CUI,RO,1656341.0,,CUI,ingredient_of,86154533.0,,RXNORM,,,,,4096.0<br>
1656328.0,,CUI,RN,1656341.0,,CUI,tradename_of,86154502.0,,RXNORM,,,,,4096.0<br>

Subject: RXCUI2 (1656341) <br>
Predicates & Object: [Read as Predicate: Source Header (Object Value)]<br>
*   ingredient_of: RELA (1656355)<br>
*   ingredient_of: RELA (1656346)<br>
*   tradename_of: RELA (1656328)<br>

# **1C. Generate Intermediate RXNORM Triples from RXNSAT table**
RXNSAT table provides the following information:<br>
*   RXNorm Strength Information: RXN_STRENGTH, RXN_AVAILABLE STRENGTH
*   NDC Code Information: NDC11 Code, NDC 2 Segment, NDC 3 Segment, SPL_SET_ID, DrugsFDA Application Number
*   UMLS Code Information: UMLSCUI, UMLSAUI

**Note: NDC Code Information Identifiers provide the Link to FDA**

**RXNSAT Headers for Reference**:<br>
RXCUI,LUI,SUI,RXAUI,STYPE,CODE,ATUI,SATUI,ATN,SAB,ATV,SUPPRESS,CVF

**RXNorm Strength Information**:<br>
Example for Entresto sample record:<br>
1656340,,,7249806,AUI,1656340,,,RXN_AVAILABLE_STRENGTH,RXNORM,24 MG / 26 MG,N,4096.0<br>
Subject: RXCUI (1656340) <br>
Predicates & Object: [Read as Predicate: Source Header (Object Value)]<br>
*   RXN_AVAILABLE_STRENGTH: ATV (24 MG / 26 MG)<br>

**NDC Code Information**:<br>
Example for NDC Code:<br>
1305100,,,12332251,AUI,1305100,,,NDC,RXNORM,75142000109,N,4096.0<br>
1305100,,,12332251,AUI,1305100,,,NDC,RXNORM,52687000201,N,4096.0<br>
1305100,,,12387798,AUI,50563-195,,,NDC,MTHSPL,50563-195-08,N,4096.0
1305100,,,12374233,AUI,75556-001,,,NDC,MTHSPL,75556-001-05,N,4096.0

Subject: RXCUI (1305100) <br>
Predicates & object:<br>
*   NDC11: ATV (75142000109)<br>
*   NDC11: ATV (52687000201)<br>
*   NDC 3 Segment: ATV (50563-195-08)<br>
*   NDC 3 Segment: ATV (75556-001-05)<br>
*   NDC 2 Segment: ATV (50563-195)<br>
*   NDC 2 Segment: ATV (75556-001)<br>

Example for SPL_SET_ID Code:<br>
1305100,,,12388790,AUI,76861-001,,,SPL_SET_ID,MTHSPL,a4f6c932-fe40-7226-e053-2a95a90a2205,N,4096.0<br>

Subject: RXCUI (1305100) <br>
Predicates & object:<br>
*   SPL_SET_ID: ATV (a4f6c932-fe40-7226-e053-2a95a90a2205)<br>

Example for Application Number:<br>
995253,,,12387879,AUI,55700-860,,,ANDA,MTHSPL,ANDA040156,N,4096.0<br>

Subject: RXCUI (995253) <br>
Predicates & object:<br>
*   ANDA: ATV (ANDA040156)<br>

**UMLS Code Information**:<br>
Example for Entresto sample record:<br>
2 entries- One for UMLSCUI and other for UMLSAUI:<br>
1656341,,,7249807,AUI,1656341,,,UMLSCUI,RXNORM,C4033616,,4096.0
1656341,,,7255921,AUI,234762,,,UMLSAUI,RXNORM,A24842892,,

Subject: RXCUI (1656341) <br>
Predicates & Object:<br>
*   UMLSCUI: ATV (C4033616)<br>
*   UMLSAUI: ATV (A24842892)<br>

# **1D. Create List/Dict to hold Known Information**
1.   Identifier Source List
2.   RXNORM Relationship Types
3.   RXNorm Term Type Dictionary
4.   Predicates in Wikidata Dictionary

# **Step 2. Find RXNormID Coverage in WIKIDATA**
# **2A. Query Wikidata SPARQL Endpoint**
Create Functions to:<br>
Generate Query for RXNorm Identifier<br>
Generate Results from the Query<br>

Note: Wikidata SPARQL Endpoint is used to get Full Coverage. To Remove API Dependency, Wikidata Dump must be used <br>

# **2B. Create Dictionary for RXNormIDs with Wikidata QNode**
1.   Get the results from function created
2.   Create Dictionary qnode_dict_inwiki
3.   Write the results for P3345 [RXNORMID] to RXNorm QRXNode_PNode file


# **Step 3. Find RXNORM NOT in Wikidata coverage using Intermediate Triples**
# **3A. Create Dictionary for RXNormIDs NOT in Wikidata**
1.   Get the RXNORMIDs from Intermediate Triples [using results from Step 1]
2.   Create Dictionary qnode_dict_notinwiki by checking if RXNORMID is in Wikidata or not [using results from Step 2]
3.   Assign QRXNode to RXNormIDs NOT in Wikidata

# **3B. Add InstanceOf Predicate for RXNormIDs NOT in Wikidata**
1.   Add P31 as 'Pharmaceutical Product' for this QRXNode. 
2.   Write the results to QRXNode_PNode file


# **Step 4: Generate Intermediate Triples for FDA using NDC Code Identifiers**
Uses NDC Code information from RXNORM intermediate Triples generated in Step 1C:
1. NDC 2 Segment: Used to link Predicates in Drug-NDC source
2. NDC 3 Segment: Used to link Predicates in Drug-NDC and Drug-Enforcement Source
3. SPL_SET_ID: Used to link Predicates in Drug-NDC and Drug-Label source
4. Application Number: Used to link Predicates in Drug-Drugs@FDA source


# **4A: Generate Intermediate Triples from Drug-NDC Source**
1. Load the data present in JSON Format. There is 1 file for this source<br> 
2. For Drug-NDC, we get information at 3 levels: NDC 2 Segment, NDC 3 Segment and SPL_SET_ID
For FDA-NDC, we get predicates for Product NDC Code [NDC 2 segment] E.g. marketing_start_date, product_type, marketing_category etc.<br>
For FDA-NDC, we also get predicates for OpenFDA attributes which uses [SPL_SET_ID] E.g. is_original_packager, manufacturer_name, unii etc.<br>
For FDA-NDC, we also get predicates for Packaging attributes which uses Package_NDC_Code [NDC 3 Segment] E.g. marketing_start_date, sample, description etc.<br>
3. For Active Ingredients, we also get **Qualifiers** for Strength
4. Write the results to 3 Intermediate Triple Files:
fda_triples_product_ndc, fda_triples_package_ndc and fda_triples_spl_ndc

# **4B: Generate Intermediate Triples from Drug-Label Source**
1. Load the data present in JSON Format. There are 9 file for this source<br> 
2. For Drug-Label, we get information at 1 levels: SPL_SET_ID
For FDA-Labeling, we get predicates at SPL_SET_ID level E.G package_label_principal_display_panel, pregnancy, pharmacokinetics, drug_interactions etc.<br>
4. Write the results to 1 Intermediate Triple Files:
fda_triples_spl_label

# **4C: Generate Intermediate Triples from Drug-Drugs@FDA Source**
1. Load the data present in JSON Format. There is 1 file for this source<br> 
2. For Drug-Drugs@FDA, we get information at 1 level: Application Number
For FDA-Drugs@FDA, we get predicates for Application Number: Openfda related predicates, sponsor_name, products information and submissions information<br>
3. For Products Information and Submissions Information, we also get a set of **Related Qualifiers**
4. Write the results to 1 Intermediate Triple Files:
fda_triples_application_drugsfda

# **4D: Generate Intermediate Triples from Drug-Enforcement Source**
1. Load the data present in JSON Format. There is 1 file for this source<br> 
2. For Drug-Drugs@FDA, we get information at 1 level: Package-NDC Code which is extracted from the Product Description
For FDA-Drugs@FDA, we get predicates for Package-NDC: Recall, Location, Reason for Recall, Event_id<br>
3. Write the results to 1 Intermediate Triple Files:
fda_triples_package_enforcement

# **Step 5: Generate KGTK Triples for RXNORM using RXNORM-Intermediate Triples**
Uses the RXNORM Intermediate Triples File generated in Step 1<br>
Uses the 2 dictionaries created in Step 2 and Step 3:<br>
qnode_dict_inwiki: RXNormIDs with QNODE in Wikidata<br>
qnode_dict_notinwiki: RXNormIDs with QRXNODE NOT in Wikidata<br>

# **5A: Generate KGTK Triples for RXNORM NODE Edge Files**
1. Get the data in required KGTK Format
2. Dump the Output in 4 files [Naming convention is as follows]:
*   Subject in Wikidata, Predicate in Wikidata: QNODE_PNODE_RXNORM
*   Subject in Wikidata, Predicate NOT in Wikidata: QNODE_PRXNODE_RXNORM
*   Subject NOT in Wikidata, Predicate in Wikidata: QRXNODE_PNODE_RXNORM
*   Subject NOT in Wikidata, Predicate NOT in Wikidata: QRXNODE_PRXNODE_RXNORM
3. Create the Predicates NOT in Wikidata dictionary
4. Write the results to these 4 KGTK Triples Files

# **5B: Generate KGTK Triples for RXNORM PROPERTIES Edge & DataType Files**
1. Segregate and Get the data in required KGTK Format for Edges and DataType using the Predicates NOT in Wikidata dictionary
2. Dump the Output in 3 files [Naming convention is as follows]:
*   Predicates NOT in Wikidata: PRXNODE_RXNORM [For Reference Only]
*   Predicates NOT in Wikidata Edges: PRXNODE_Edges_RXNORM
*   Predicates NOT in Wikidata DataType: PRXNODE_DataType_RXNORM
3. Write the results to these 3 KGTK Triples Files

# **Step 6: Generate KGTK Triples for FDA using FDA-Intermediate Triples**
Uses the FDA Intermediate Triple files generated in Step 4
Uses the 2 dictionaries created in Step 2 and Step 3:<br>
qnode_dict_inwiki: RXNormIDs with QNODE in Wikidata<br>
qnode_dict_notinwiki: RXNormIDs with QRXNODE NOT in Wikidata<br>

# **6A: Generate KGTK Triples for FDA NODES Edge File- from PRODUCT-NDC Intermediate File**
1. Uses the fda_product_ndc Intermediate Triple files generated in Step 4
2. Get the data in required KGTK Format
3. Handle the 4 cases to Dump the Output in 4 files [Naming convention is as follows]:
*   Subject in Wikidata, Predicate in Wikidata: QNODE_PNODE_FDA
*   Subject in Wikidata, Predicate NOT in Wikidata: QNODE_PRXNODE_FDA
*   Subject NOT in Wikidata, Predicate in Wikidata: QRXNODE_PNODE_FDA
*   Subject NOT in Wikidata, Predicate NOT in Wikidata: QRXNODE_PRXNODE_FDA
4. Create the Predicates NOT in Wikidata dictionary
5. Write the results to these 4 KGTK Triples Files

# **6B: Generate KGTK Triples for FDA NODES Edge File- from PACKAGE-NDC Intermediate File**
1. Uses the fda_package_ndc Intermediate Triple files generated in Step 4
2. Get the data in required KGTK Format
3. Handle the 4 cases to Dump the Output in 4 files [Naming convention is as follows]:
*   Subject in Wikidata, Predicate in Wikidata: QNODE_PNODE_FDA
*   Subject in Wikidata, Predicate NOT in Wikidata: QNODE_PRXNODE_FDA
*   Subject NOT in Wikidata, Predicate in Wikidata: QRXNODE_PNODE_FDA
*   Subject NOT in Wikidata, Predicate NOT in Wikidata: QRXNODE_PRXNODE_FDA
4. Create the Predicates NOT in Wikidata dictionary
5. Write the results to these 4 KGTK Triples Files


# **6C: Generate KGTK Triples for FDA NODES Edge File- from SPL-NDC Intermediate File**
1. Uses the fda_spl_ndc Intermediate Triple files generated in Step 4
2. Get the data in required KGTK Format
3. Handle the 4 cases to Dump the Output in 4 files [Naming convention is as follows]:
*   Subject in Wikidata, Predicate in Wikidata: QNODE_PNODE_FDA
*   Subject in Wikidata, Predicate NOT in Wikidata: QNODE_PRXNODE_FDA
*   Subject NOT in Wikidata, Predicate in Wikidata: QRXNODE_PNODE_FDA
*   Subject NOT in Wikidata, Predicate NOT in Wikidata: QRXNODE_PRXNODE_FDA
4. Create the Predicates NOT in Wikidata dictionary
5. Write the results to these 4 KGTK Triples Files

# **6D: Generate KGTK Triples for FDA NODES Edge File- from SPL-LABELING Intermediate File**
1. Uses the fda_spl_labeling Intermediate Triple files generated in Step 4
2. Get the data in required KGTK Format
3. Handle the 4 cases to Dump the Output in 4 files [Naming convention is as follows]:
*   Subject in Wikidata, Predicate in Wikidata: QNODE_PNODE_FDA
*   Subject in Wikidata, Predicate NOT in Wikidata: QNODE_PRXNODE_FDA
*   Subject NOT in Wikidata, Predicate in Wikidata: QRXNODE_PNODE_FDA
*   Subject NOT in Wikidata, Predicate NOT in Wikidata: QRXNODE_PRXNODE_FDA
4. Create the Predicates NOT in Wikidata dictionary
5. Write the results to these 4 KGTK Triples Files

# **6E: Generate KGTK Triples for FDA NODES Edge File- from APPLICATION-DRUGSFDA Intermediate File**
1. Uses the fda_application_drugsfda Intermediate Triple files generated in Step 4
2. Get the data in required KGTK Format
3. Handle the 4 cases to Dump the Output in 4 files [Naming convention is as follows]:
*   Subject in Wikidata, Predicate in Wikidata: QNODE_PNODE_FDA
*   Subject in Wikidata, Predicate NOT in Wikidata: QNODE_PRXNODE_FDA
*   Subject NOT in Wikidata, Predicate in Wikidata: QRXNODE_PNODE_FDA
*   Subject NOT in Wikidata, Predicate NOT in Wikidata: QRXNODE_PRXNODE_FDA
4. Create the Predicates NOT in Wikidata dictionary
5. Write the results to these 4 KGTK Triples Files

# **6F: Generate KGTK Triples for FDA NODES Edge File- from PACKAGE-ENFORCEMENT Intermediate File**
1. Uses the fda_package_enforcment Intermediate Triple files generated in Step 4
2. Get the data in required KGTK Format
3. Handle the 4 cases to Dump the Output in 4 files [Naming convention is as follows]:
*   Subject in Wikidata, Predicate in Wikidata: QNODE_PNODE_FDA
*   Subject in Wikidata, Predicate NOT in Wikidata: QNODE_PRXNODE_FDA
*   Subject NOT in Wikidata, Predicate in Wikidata: QRXNODE_PNODE_FDA
*   Subject NOT in Wikidata, Predicate NOT in Wikidata: QRXNODE_PRXNODE_FDA
4. Create the Predicates NOT in Wikidata dictionary
5. Write the results to these 4 KGTK Triples Files

# **6G: Generate KGTK Triples for FDA PROPERTIES Edge & DataType Files**
1. Segregate and Get the data in required KGTK Format for Edges and DataType using the Predicates NOT in Wikidata dictionary
2. Dump the Output in 3 files [Naming convention is as follows]:
*   Predicates NOT in Wikidata: PRXNODE_FDA [For Reference Only]
*   Predicates NOT in Wikidata Edges: PRXNODE_Edges_FDA
*   Predicates NOT in Wikidata DataType: PRXNODE_DataType_FDA
3. Write the results to these 3 KGTK Triples Files

# **Step 7: Perform KGTK Transformations and Validation on NODES and PROPERTIES Edge files**

# **7A: Perform KGTK Compact Transformation**
1. Perform KGTK Compact Transformation for RXNorm KGTK triples NODES:<br>
      4 Files- QRXNode_PNode, QRXNode_PRXNode, QNode_PNode, QNode_PRXNode
2. Perform KGTK Compact Transformation for FDA KGTK triples NODES:<br>
      4 Files- QRXNode_PNode, QRXNode_PRXNode, QNode_PNode, QNode_PRXNode
3. Perform KGTK Compact Transformation for RXNorm KGTK triples PROPERTIES:<br>
      1 File- PRXNode
4. Perform KGTK Compact Transformation for FDA KGTK triples PROPERTIES:<br>
      1 File- PRXNode

# **7B: Perform KGTK ADD-ID Transformation**
1. Perform KGTK ADD-ID Transformation for RXNorm KGTK triples NODES:<br>
      4 Files- QRXNode_PNode, QRXNode_PRXNode, QNode_PNode, QNode_PRXNode
2. Perform KGTK ADD-ID Transformation for FDA KGTK triples NODES:<br>
      4 Files- QRXNode_PNode, QRXNode_PRXNode, QNode_PNode, QNode_PRXNode
3. Perform KGTK ADD-ID Transformation for RXNorm KGTK triples PROPERTIES:<br>
      1 File- PRXNode
4. Perform KGTK ADD-ID Transformation for FDA KGTK triples PROPERTIES:<br>
      1 File- PRXNode

# **7C: Perform KGTK Validate**
1. Perform KGTK Validate for RXNorm KGTK triples NODES:<br>
      4 Files- QRXNode_PNode, QRXNode_PRXNode, QNode_PNode, QNode_PRXNode
2. Perform KGTK Validate for FDA KGTK triples NODES:<br>
      4 Files- QRXNode_PNode, QRXNode_PRXNode, QNode_PNode, QNode_PRXNode
3. Perform KGTK Validate for RXNorm KGTK triples PROPERTIES:<br>
      1 File- PRXNode
4. Perform KGTK Validate for FDA KGTK triples PROPERTIES:<br>
      1 File- PRXNode

# **Step 8: Generate KGTK Triples for FDA and RXNORM Qualifiers**
Uses the FDA and RXNORM KGTK Triples with IDs [created in Step 7] to generate Qualifier Edges

# **8A: Generate KGTK Triples for FDA Qualifiers**
1. Generate KGTK Triples for Qualifiers Related to Active Ingredients in Drug-NDC, Products information in Drugs@FDA and Submissions Information in Drugs@FDA
2. Get the qualifiers for 2 files:
      QNODE_PRXNODE_QUALIFIER and QRXNODE_PRXNODE_QUALIFIER
3. Write the results to these 2 files

# **8B: Generate KGTK Triples for RXNORM Qualifiers**
1. Generate KGTK Triples for Qualifiers Related to RXNORM Identifiers
2. Get the qualifiers for 4 files:
      QNODE_PRXNODE_QUALIFIER, QRXNODE_PRXNODE_QUALIFIER, QNODE_PNODE_QUALIFIER, QRXNODE_PNODE_QUALIFIER
3. Write the results to these 4 files

# **8C: Generate KGTK Triples for FDA Additional PROPERTIES from Qualifiers**
1. Generate KGTK Triples for Properties Related to FDA Properties
2. Get the properties:<br>
      PRXNODE_QUALIFIER_FDA<br>
      PRXNODE_edges_QUALIFIER_FDA<br>
      PRXNODE_datatype_QUALIFIER_FDA<br>
3. Write the results to these 3 files

# **Step 9: Perform KGTK Transformations and Validation for RXNORM AND FDA QUALIFIERS**

# **9A: Perform KGTK Compact Transformation**
1. Perform KGTK Compact Transformation for RXNORM Qualifiers
2. Perform KGTK Compact Transformation for FDA Qualifiers
3. Perform KGTK Compact Transformation for FDA Properties related to Qualifiers

# **9B: Perform KGTK ADD-ID Transformation**
1. Perform KGTK ADD-ID Transformation for RXNORM Qualifiers
2. Perform KGTK ADD-ID Transformation for FDA Qualifiers
3. Perform KGTK ADD-ID Transformation for FDA Properties related to Qualifiers

# **9C: Perform KGTK Validate**
1. Perform KGTK Validate for RXNORM Qualifiers
2. Perform KGTK Validate Transformation for FDA Qualifiers
3. Perform KGTK Validate Transformation for FDA Properties related to Qualifiers

# **Step 10: Generate Triples for Ingestion**

# **10A: Merge RXNORM KGTK Triples for Edges and Property-DataType**
1. Perform KGTK Concatenate Transformation for RXNorm KGTK triples- NODES edges:<br>
4 Files- QRXNode_PNode, QRXNode_PRXNode, QNode_PNode, QNode_PRXNode
2. Perform KGTK Concatenate Transformation for RXNorm KGTK triples- QUALIFIERS edges:<br>
4 Files- QRXNode_PNode_Qualifier, QRXNode_PRXNode_Qualifier, QNode_PNode_Qualifier, QNode_PRXNode_Qualifier
3. Perform KGTK Concatenate Transformation for RXNorm KGTK triples- PROPERTIES edges:<br>
1 Files- PRXNode_edges
4. Perform KGTK Concatenate Transformation for RXNorm KGTK triples- PPROPERTIES datatype:<br>
1 Files- PRXNode_datatype

# **10B: Merge FDA KGTK Triples for Edges and Property-DataType**
1. Perform KGTK Concatenate Transformation for FDA KGTK triples- NODES edges:<br>
4 Files- QRXNode_PNode, QRXNode_PRXNode, QNode_PNode, QNode_PRXNode
2. Perform KGTK Concatenate Transformation for FDA KGTK triples- QUALIFIERS edges:<br>
4 Files- QRXNode_PNode_Qualifier, QRXNode_PRXNode_Qualifier, QNode_PNode_Qualifier, QNode_PRXNode_Qualifier
3. Perform KGTK Concatenate Transformation for FDA KGTK triples- PROPERTIES edges:<br>
2 Files- PRXNode_edges, PRXNode_edges_Qualifier
4. Perform KGTK Concatenate Transformation for FDA KGTK triples- PPROPERTIES datatype:<br>
2 Files- PRXNode_datatype, PRXNode_datatype_Qualifier

# **10C: Merge RXNORM All Edges and FDA All Edges [Nodes+Properties+Qualifiers]**
1. Perform KGTK Concatenate Transformation for RXNROM KGTK triples- ALL edges:<br>
3 Files- NODES_EDGES, PROPERTIES_EDGES and QUALFIERS_EDGES
2. Perform KGTK Concatenate Transformation for FDA KGTK triples- ALL edges:<br>
3 Files- NODES_EDGES, PROPERTIES_EDGES and QUALFIERS_EDGES

# **10D: FINAL Merge for RXNORM+FDA Combined- ALL Edges AND Property DataType**
1. Perform KGTK Concatenate Transformation for ALL edges:<br>
2 Files- ALLEDGES_RXNORM, ALLEDGES_FDA
2. Perform KGTK Concatenate Transformation for ALL Property DataType:<br>
2 Files- PRXNODE_DATATYPE_RXNORM, PRXNODE_DATATYPE_FDA

