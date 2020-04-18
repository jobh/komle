# komle

komle, the name of a dish, is a python library to work with WITSML v1.4.1.1. It uses [PyXB](http://pyxb.sourceforge.net/) to marshal/unmarshal xml files according to the [WITSML version 1.4.1.1 schema](http://w3.energistics.org/schema/WITSML_v1.4.1.1_Data_Schema/witsml_v1.4.1.1_data/index_witsml_schema.html). It also comes with a Soap client to talk to a witsml server, according to the [webservice description](http://w3.energistics.org/schema/witsml_v1.4.0_api/WMLS.WSDL). The documents are validated agains the generated_read_schemas of WITSML version 1.4.1.1.

This code is just a fast write up of how to work with witsml in python.

## Install

``` bash
pip3 install git+ssh://git@github.com/knle88/komle.git
```

Or if the repo is cloned

``` bash
pip3 install -U .
```

## Getting started

``` bash
from komle.read_bindings import witsml

with open('log.xml', 'r') as log_file:
    # logs is a regular python object according to the witsml schema
    logs = witsml.CreateFromDocument(log_file.read())

# Print the witsml documentation for logs
print(logs._element().documentation())

# Print the schema location for logCurveInfo, nice to have for reference
print(logs.log[0].logCurveInfo[0]._element().xsdLocation().locationBase)

print([l.name for l in logs.log])
```

`witsml.CreateFromDocument` works on any witsml object, like trajectorys, mudLogs, tubulars etc, and returns a python representation according to 
the schema. Nodes are converted to there corresponding python types and accessed like any other python object, the exception is leaf nodes with attributes where one must call `value()` since primitive types in python does not have custom attributes. For example `mdTop.value()` where mdTop also has the attribute `mdTop.uom`, also see [examples/hello_witsml.py](examples/hello_witsml.py).