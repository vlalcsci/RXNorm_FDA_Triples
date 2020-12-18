# **Architecture**
**Note: SPARQL Endpoint is used for Exact Coverage**
This dependency can be removed by using Wikidata Dump

**Current Issues**<br>
KGTK Validation is not possible for 2 KGTK Files for FDA: <br>
QNODE_PRXNODE, QRXNODE_PRXNODE<br>
Reason: The object for these Triples contain complex tables- Some of them in Actual HTML Format, while others are present with KGTK Conflicting delimeters like @, | or "<br>
Read the main README for the project for naming convention used<br>

**Future Work**<br>
There are Basically 4 TYPES of Files for Node Edges in both RXNORM and FDA:<br>
		QNODE_PNODE, QNODE_PRXNODE, QRXNODE_PNODE, QRXNODE_PRXNODE<br>
*   Subject in Wikidata, Predicate in Wikidata [QNODE_PNODE]: <br>
		We can expand the Wikidata Knowledge Graph to include these Additional Properties or Identifiers(PNODE)<br>
		We can also Perform Label, Description or Alias Comparison and Analysis for such QNodes<br>
*   Subject in Wikidata, Predicate NOT in Wikidata [QNODE_PRXNODE]: <br>
		We can Perform Linking/Semantic Labeling for such Predicates(PRXNODE) to see if they are already in Wikidata<br>
		We can also Perform Label, Description or Alias Comparison and Analysis for such QNodes<br>
*   Subject NOT in Wikidata, Predicate in Wikidata [QRXNODE_PNODE]: <br>
		We can Perform Identifiers(PNODE) based linking for such Nodes to see if they are already in Wikidata<br>
		We can Perform Entity Linking for such Nodes to see if they are already in Wikidata<br>
*   Subject NOT in Wikidata, Predicate NOT in Wikidata [QRXNODE_PRXNODE]: <br>
		We can Perform Identifiers(PNODE) based linking for such Nodes to see if they are already in Wikidata<br>
		We can Perform Entity Linking for such Nodes to see if they are already in Wikidata<br>

**SUMMARY:**<br>
For Subject in Wikidata: 
*	Perform Label, Description or Alias Comparison and Analysis for such QNodes<br>

For Subject NOT in Wikidata: 
*	Perform Entity Linking for such Nodes to see if they are already in Wikidata<br><br>

For Predicate in Wikidata: 
* 	For QNode, Expand the Wikidata Knowledge Graph to include these Additional Properties or Identifiers(PNODE)<br>
*	For QRXNode, Perform Identifiers(PNODE) based linking for such Nodes to see if they are already in Wikidata<br>

For Predicate NOT in Wikidata:
*	We can Perform Identifiers(PNODE) based linking for such Nodes to see if they are already in Wikidata<br>
