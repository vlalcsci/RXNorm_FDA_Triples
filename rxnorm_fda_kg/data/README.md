# **Step 0: Download RXNORM and FDA Data**
**RXNORM**:
1. Use this DATABASE creation automation script from RXNORM Technical Documentation- https://www.nlm.nih.gov/research/umls/rxnorm/docs/techdoc.html#s13_0
2. Convert the Resultant SQL Files to get required CSV Files 

**FDA**:
1. Use this JSON from OPENFDA Documentation to get required - https://api.fda.gov/download.json
2. Get the Required FDA files from: fda_json["results"]["drug"]["ndc"], fda_json["results"]["drug"]["label"], fda_json["results"]["drug"]["drugsfda"] & fda_json["results"]["drug"]["enforcement"]