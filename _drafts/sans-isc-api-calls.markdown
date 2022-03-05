---
layout: "post"
title: "SANS ISC IPA Calls"
date: "2022-02-28 08:11"
---
[ISC/DSHield API](https://isc.sans.edu/api/)
There is a python package called [dshield](https://pypi.org/project/dshield/) which can be pip insalled (pip install dshield). This can be used to query [ISC/DSHield API](https://isc.sans.edu/api/). Usage is documented at [DSHield Reada the docs](https://dshield.readthedocs.io/en/latest/).

Some example usage from the documentation
```python
    >>> import dshield
    >>> dshield.infocon()
    {'status': 'green'}
    >>> dshield.infocon(dshield.JSON)
    '{"status";"green"}'
```
This seems really usefull to query the API using python from the command-line, some interresting examples include `dhield.ip()` to query the database and `dhield.topips()` to list top reported IP adresses. You can use different return formats like XML, TEXT, PHP and as in the example above JSON. With other options available.


hopscotch
