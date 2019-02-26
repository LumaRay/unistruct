# unistruct v0.0.0

## Universal Open Information Structure (UNISTRUCT) Concept

*This is a concept of an open global distributed javascript/JSON object, that is able to incapsulate entire human data.*

Any country / organization or other entity are able to add their open data subtree to corresponding "leaves" of this global structure and provide their own leaves to let others add their child tree sub-structures.

Initial joined structure object is:

    { 
      "Unistruct": {
        "World Wide Web": {
          "Domains": {
            "github.com": {...}
          }
        }
      } 
    }
    
Referencing the data is easy:

    Unistruct["World Wide Web"]["Domains"]["github.com"]
    
Structure values can be:
* subsequent section trees
* arrays
* strings / digital / boolean values

In a back-end, in order to make structure representation more convenient, it can be split into tree sections (JSON objects) using hash references.

Hash reference is a hash of a child tree section.

Hash reference is a string, which consists of "uni" prefix, followed by a colon, then hash algorithm name, followed by a colon, and the hex value of the hash itself:

    "uni:sha2-512:..."

Subscription to any Unistruct tree section's hash changes is essentially a subscription to changes in a corresponding subtree.

Thus a JSON file/data object representing a particular subsection of the structure, should be rehashed each time it is altered, then it should be saved as another JSON file/data object with a name containing it's new hash value and "json" extension.
After that it's parent structure subsection holder and all other subscribers should be notified that the child hash reference should be changed.
In their turn, the parent structure subsection holder should recalculate hash of their updated JSON file/data object, save as a new file with a name containing new hash value and notify their direct parent and all other subscribers, and so on - up to the root section of the structure.

For better performance it is recommended for each structure section's JSON file to contain only one logical level of the structure, providing hash references to it's children. E.g. for structure section: 

    {
      "item name 1": "uni:sha2-256:<hex hash 1>",
      "item name 2": "uni:sha2-256:<hex hash 2>",
      "item name 3": "uni:sha2-256:<hex hash 3>",
      ...
    }

For array section:

    [
      "uni:sha2-256:<hex hash 1>",
      "uni:sha2-256:<hex hash 2>",
      "uni:sha2-256:<hex hash 3>",
      ...
    ]

All JSON files's hashes may be stored in any popular blockchain transactions and / or be time stamped with any popular TSA in order to enable historical search and proof.

Hash references to parents up to the root are allowed, however to prevent cyclic references, algorithms that parse the tree should break on another branch parsing when they find themselves walking through the nodes they already parsed (e.g. by making parsed hashes' log) - the algorithms should continue to the next branch in this case.

Multiple references to the same structure leaf are allowed, provided that parsing algorithms detect cyclic referencing and intelligenly skip this cases.

Parent structure holders should only accept hash changes notifications from those children, who passes validation of their new JSON files/data objects.

So every structure participant is responsible of it's own as well as it's children structures' validities.

