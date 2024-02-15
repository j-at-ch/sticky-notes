# Algorithm Notes

### Enumerate maximal bicliques
This is used when trying to identify common aliases via an intermediate source.
E.g. Jane has multiple cards and multiple accounts at a shop - and we want to match this into a common entity "Jane".
Can we do this by using payments from cards on given accounts alone?

```python
def recurse_graph(
    ida_list: List,
    ida2idbs: Dict[int, List], 
    idb2idas: Dict[int, List]
):
    idb_list = []
    for ida in ida_list:
        idb_list += ida2idbs.get(ida, [])
    idb_list = list(set(idb_list))
    
    ida_list_updated = []
    for idb in idb_list:
        ida_list_updated += idb2idas.get(idb, [])
    ida_list_updated = list(set(ida_list_updated))

    if len(ida_list_updated) == len(ida_list):
        out = ida_list, idb_list
    else:
        out = recurse_graph(ida_list_updated, ida2idbs, idb2idas)
    return out
```
