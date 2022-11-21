# Generator Expressions:
* save memory
* yield items one by one
* enclosed in parantheses rather than brackets
* save the cost of building a list with a million items

# Tuples
* immutable lists
* records with no field names

## Tuples as Records
Each item in the tuple holds the data for one field, and the position of the item gives its meaning.  
Sorting the tuple would destroy the information because the meaning of each field is given by its position in the tuple.  
The % formatting operator understands tuples and treats each item as a separate field.

## Unpacking
The for loop knows how to retrieve the items of a tuple separately. This is called “unpacking.” When we are not interested in an item, we assign it to _, a dummy variable.

# PEP
PEP (Python Enhancement Proposal):A PEP is a technical design doc for the Python community which describes a new feature for the language itself, its processes, or its environment.