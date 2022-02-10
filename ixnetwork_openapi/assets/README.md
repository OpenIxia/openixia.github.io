# ResourceManager API (9.20.2112.6)

## Overview
The resourcemanager importConfig API is used to efficiently create or update an IxNetwork configuration in one call.
- The IxNetwork configuration is composed of schema objects detailed in this specification.

## Getting Started
### Create a raw traffic configuration
```python
import json
from ixnetwork_restpy import SessionAssistant

session = SessionAssistant(IpAddress='127.0.0.1')

raw_traffic_objects = [
    {
        'xpath': '/vport[1]',
        'name': 'Tx Port'
    },
    {
        'xpath': '/vport[2]',
        'name': 'Rx Port'
    },
    {
        'xpath': '/traffic/trafficItem[1]/endpointSet[1]',
        'sources': ['/vport[1]/protocols'],
        'destinations': ['/vport[2]/protocols']
    },
    {
        'xpath': "/traffic/trafficItem[1]/configElement[1]/stack[@alias = 'ethernet-1']/field[@alias = 'ethernet.src.mac']",
        'singleValue': '00faceface'
    },
    {
        'xpath': "/traffic/trafficItem[1]/configElement[1]/stack[@alias = 'vlan-2']/field[@alias = 'vlan.header.vlanTag.vlanID-3']",
        'singleValue': '39'
    },
    {
        'xpath': "/traffic/trafficItem[1]/configElement[1]/stack[@alias = 'ipv4-3']/field[@alias = 'ipv4.header.srcIp-27']",
        'singleValue': '1.1.1.1'
    },
    {
        'xpath': "/traffic/trafficItem[1]/configElement[1]/stack[@alias = 'tcp-4']/field[@alias = 'tcp.header.dstPort-2']",
        'auto': False,
        'singleValue': '11009'
    }
]
session.Ixnetwork.ResourceManager.ImportConfig(json.dumps(raw_traffic_objects), True)
```
### Create an ipv4 traffic configuration
```python
import json
from ixnetwork_restpy import SessionAssistant

session = SessionAssistant(IpAddress='127.0.0.1')

ipv4_traffic_objects = [
    {
        'xpath': '/vport[1]',
        'name': 'Tx Port'
    },
    {
        'xpath': '/vport[2]',
        'name': 'Rx Port'
    },
    {
        'xpath': '/topology[1]',
        'name': 'Tx Topology',
        'ports': ['/vport[1]']
    },
    {
        'xpath': '/topology[2]',
        'name': 'Rx Topology',
        'ports': ['/vport[2]']
    },
    {
        'xpath': '/topology[1]/deviceGroup[1]',
        'multiplier': 10
    },
    {
        'xpath': '/topology[2]/deviceGroup[1]',
        'multiplier': 10
    },
    {
        'xpath': '/topology[1]/deviceGroup[1]/ethernet[1]',
        'name': 'Tx Ethernet'
    },
    {
        'xpath': '/topology[2]/deviceGroup[1]/ethernet[1]',
        'name': 'Rx Ethernet'
    },
    {
        'xpath': '/topology[1]/deviceGroup[1]/ethernet[1]/ipv4[1]',
        'name': 'Tx Ipv4'
    },
    {
        'xpath': '/topology[2]/deviceGroup[1]/ethernet[1]/ipv4[1]',
        'name': 'Rx Ipv4',
    },
    {
        "xpath": "/multivalue[@source = '/topology[1]/deviceGroup[1]/ethernet[1]/ipv4[1] address']/counter",
        "start": "1.1.1.1",
        "step": "0.0.0.1",
        "direction": "increment"
    },
    {
        "xpath": "/multivalue[@source = '/topology[2]/deviceGroup[1]/ethernet[1]/ipv4[1] address']/counter",
        "start": "1.1.2.1",
        "step": "0.0.0.1",
        "direction": "increment"
    },
    {
        "xpath": "/multivalue[@source = '/topology[1]/deviceGroup[1]/ethernet[1]/ipv4[1] gatewayIp']/singleValue",
        "value": "1.1.2.1"
    },
    {
        "xpath": "/multivalue[@source = '/topology[2]/deviceGroup[1]/ethernet[1]/ipv4[1] gatewayIp']/singleValue",
        "value": "1.1.1.1"
    },
    {
        'xpath': '/traffic/trafficItem[1]',
        'name': 'Ipv4 Traffic',
        'trafficType': 'ipv4',
        'endpointSet': {
            'xpath': '/traffic/trafficItem[1]/endpointSet[1]',
            'sources': ['/topology[1]'],
            'destinations': ['/topology[2]']
        }
    }
]
session.Ixnetwork.ResourceManager.ImportConfig(json.dumps(ipv4_traffic_objects), True)
```

## Hints
- View the structure and details of the importConfig schema
- Select a schema object from the tree and view the python requests sample to see how the importConfig operation is called
- Locate specific objects to import using the Schema Objects tree
- View sample code to see how the json object and specifically the xpath is constructed
- Use the API Browser to view resources and their structure
- Export an existing configuration to view object structure
```python
import json
from ixnetwork_restpy import SessionAssistant
session = SessionAssistant(IpAddress='127.0.0.1')
export_paths = [
    '/vport/descendant-or-self::\*',
    '/topology/descendant-or-self::\*',
    '/traffic/descendant-or-self::\*'
]
json_string = session.Ixnetwork.ResourceManager.ExportConfig(export_paths, True, 'json')
config = json.loads(json_string)
```

## API Usage
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
