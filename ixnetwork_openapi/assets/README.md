# Overview

The resourcemanager importConfig API is used to efficiently create or update an IxNetwork configuration in one call.
- The IxNetwork configuration is composed of schema objects detailed in this specification.

## Getting Started
- View the structure and details of the importConfig schema
- Select a schema object from the tree and view the python requests sample to see how the importConfig operation is called
- Locate specific objects to import using the Schema Objects tree
- View sample code to see how the json object and specifically the xpath is constructed
- Use the API Browser to view resources and their structure

## ImportConfig API Usage
### A Configuration can be created or updated
```python
import json
from ixnetwork_restpy import SessionAssistant
session = SessionAssistant(IpAddress='127.0.0.1')

# create a new configuration
vport = {
    'xpath': '/vport[1]',
    'name': 'Created vport name'
}
session.Ixnetwork.ResourceManager.ImportConfig(json.dumps(vport), True)

# update an existing configuration
vport = {
    'xpath': '/vport[1]',
    'name': 'Updated vport name'
}
session.Ixnetwork.ResourceManager.ImportConfig(json.dumps(vport), False)   
```

### Objects can be imported one at a time
```python
import json
from ixnetwork_restpy import SessionAssistant
session = SessionAssistant(IpAddress='127.0.0.1')
vport = {
    'xpath': '/vport[1]'
}
session.Ixnetwork.ResourceManager.ImportConfig(json.dumps(vport), True)        
```

### Multiple objects can be imported at the same time in a flat list
```python
import json
from ixnetwork_restpy import SessionAssistant

session = SessionAssistant(IpAddress='127.0.0.1')

raw_traffic_objects = [
    {
        'xpath': '/vport[1]',
        'name': 'Tx Port'
    },
    {
        'xpath': '/vport[2]',
        'name': 'Rx Port'
    },
    {
        'xpath': '/traffic/trafficItem[1]/endpointSet[1]',
        'sources': ['/vport[1]/protocols'],
        'destinations': ['/vport[2]/protocols']
    }
]
session.Ixnetwork.ResourceManager.ImportConfig(json.dumps(raw_traffic_objects), True)        
```
