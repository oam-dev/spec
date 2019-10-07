# Q&A

1. what does `JSON schema expressed as inline YAML` mean for properties in trait spec?

To understand this, you need first understand what is [JSON schema](https://json-schema.org/understanding-json-schema/). To be simple, JSON schema allows you to annotate and validate JSON documents. So the field properties here which `using JSON schema expressed as inline YAML` allows us to annotate and validate YAML documents here for Trait properties.

To clarify, JSON schema expressed as inline YAML just means, using the same way as JSON schema did to annotate and validate properties for Trait Spec by YAML format.

Let's come to know how JSON schema could have this power.

JSON has six primitive types:

1. `object`: { "key1": "value1", "key2": "value2" }
2. `array`: [ "first", "second", "third" ]
3. `number`: 42  3.1415926
4. `string`: "This is a string"
5. `boolean`: true  false
6. `null`: null

With these simple data types, all kinds of structured data can be represented. JSON Schema itself is written in JSON. It is data itself, not a computer program. It’s just a declarative format for “describing the structure of other data”.

The most common thing to do in a JSON Schema is to restrict to a specific type. The `type` keyword is used for that.

For example, in the following, only strings are accepted:

```
{ "type": "string" }
```

Expressed as inline YAML, it could be:

```
type: string
```

You could use regular expressions to express constraints by `pattern` keyword:

```
{
   "type": "string",
   "pattern": "^(\\([0-9]{3}\\))?[0-9]{3}-[0-9]{4}$"
}
```

You can easily express this as inline YAML:

```
type: string
pattern: ^(\\([0-9]{3}\\))?[0-9]{3}-[0-9]{4}$
```

JSON Schema includes a few keywords, `title`, `description`, `default`, `examples` that aren’t strictly used for validation, but are used to describe parts of a schema.

```
{
  "type" : "numner",
  "title" : "Match number",
  "description" : "This is a schema that matches number.",
  "default" : 0,
  "examples" : [
    1,
    4.5
  ]
}
```

In YAML:

```

type: numner
title: Match number
description: This is a schema that matches number.
default: 0
examples:
  - 1
  - 4.5
```

`boolean`, `number`, `null` are almost the same with `string`, so what about `object`? Let's look at this example:

```
{
  "type": "object",
  "properties": {
    "number":      { "type": "number" },
    "street_name": { "type": "string" },
    "street_type": { "type": "string",
                     "enum": ["Street", "Avenue", "Boulevard"]
                   }
  }
}
```

Just like `string` and `number`, use `type` keyword to specify `object` type, The properties (key-value pairs) on an object are defined using the `properties` keyword. The value of `properties` is an object, where each key is the name of a property and each value is a JSON schema used to validate that property.The `enum` here also one of keywords used to restrict values must in these range.

So in this example, we annotate an object with three fields, one of them named `number` is specified with type  `number`, while the other two are specified with type `string`, the field called `street_type` can only have value be one of "Street", "Avenue", "Boulevard".

In YAML:

```
type: object
properties:
  number:
    type: number
  street_name:
    type: string
  street_type:
    type: string
    enum: 
    - Street
    - Avenue
    - Boulevard
```

The last one is `object`, there are two ways in which arrays are generally used in JSON:

* List validation: a sequence of arbitrary length where each item matches the same schema.

```
{
  "type": "array",
  "items": {
    "type": "number"
  }
}
```

```
type: array
items:
  type: number
```

* Tuple validation: a sequence of fixed length where each item may have a different schema. In this usage, the index (or location) of each item is meaningful as to how the value is interpreted.

```
{
  "type": "array",
  "items": [
    {
      "type": "number"
    },
    {
      "type": "string"
    },
    {
      "type": "string",
      "enum": ["Street", "Avenue", "Boulevard"]
    }
  ]
}
```

```
type: array
items
- type: number
- type: string
- type: string
  enum:
  - Street
  - Avenue
  - Boulevard
```

So we have briefly introduce how could JSON schema expressed as inline YAML work, you could learn more from [JSON schema website](https://json-schema.org).