# ComplexJSON Library #

## What is this? ##
The module allows you serialize your python complex objects to json and deserialize it.

## Quick Guide ##

Just use it like python builtin json module

```python
import ComplexJSON
...
json_obj = ComplexJSON.dumps(obj)
...
new_obj = ComplexJSON.loads(json_obj)
```

----------

## Warning ##

If you use this code:

```python
import ComplexJSON

...
a = [1, 2, 3]
b = [a, a]
c = ComplexJSON.dumps(b)
b = ComplexJSON.loads(c)
```

you will get multiple objects instead of 1 object with multiple links:

```python
b[0].pop()
print(b)
>>> [[1, 2], [1, 2, 3]]
```

Although you might expect:
```python
>>> [[1, 2], [1, 2]]
```



### Using ###

There are variants of usage:

```python
import ComplexJSON
...
json_obj = ComplexJSON.dumps(obj)
new_obj = ComplexJSON.loads(json_obj)
...
# or
with open("test.json", "wt", encoding="Utf-8") as f:
    ComplexJSON.dump(obj, f)
...
with open("test.json", "rt", encoding="Utf-8") as f:
    new_obj = ComplexJSON.load(json_obj, f)
# or
import json
json_obj = json.dumps(obj, cls=ComplexJSON.ComplexJSONEncoder)
new_obj = json.loads(json_obj, cls=ComplexJSON.ComplexJSONDecoder)
# or
import json
with open("test.json", "wt", encoding="Utf-8") as f:
    json.dump(obj, f, cls=ComplexJSON.ComplexJSONEncoder)
...
with open("test.json", "rt", encoding="Utf-8") as f:
    new_obj = json.load(json_obj, f, cls=ComplexJSON.ComplexJSONDecoder)
```

#### Change json module ####

This module by default uses builtin python json module in ComplexJSONEncoder and ComplexJSONDecoder.
You can change it. Set environ "COMPLEX_JSON_MODULE_NAME" to name of your json library ("json" by default).

#### Change codewords for ComplexJSON ####

By default, the codewords for the module and class are "\_\_module\_\_" and "\_\_class\_\_", respectively

```python
class A:
    def __init__(self):
        self.a = 1
        self.b = 2
```

Object of this class will be serialized as:
```python
{"a": 1, "b": 2, "__module__": "__main__", "__class__": "A"}
```

But you can change the codewords for module and class:

```python
from ComplexJSON import vardef

vardef.classword = "_c_"
vardef.moduleword = "_m_"
```

And now A class will be serialized as:
```python
{"a": 1, "b": 2, "_m_": "__main__", "_c_": "A"}
```

----------