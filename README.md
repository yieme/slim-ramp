slim-ramp
=========

Ramp a Slim JSON Schema to a [JSON Schema](http://json-schema.org)

While [Schema Ramp](https://github.com/yieme/schema-ramp) would take

```js
{
  "name": "Joe Smith",
  "email": "joe@smith.com",
  "role": "user",
  "address": {
    "streetAddress": "123 Somewhere Ave",
    "city": "Boise",
    "zip": 83702
  }
}
```

and return

```js
{
  "type": "object",
  "properties": {
    "name": {
      "type": "string"
    },
    "email": {
      "type": "string"
    },
    "role": {
      "type": "string"
    },
    "address": {
      "type": "object",
      "properties": {
        "streetAddress": {
          "type": "string"
        },
        "city": {
          "type": "string"
        },
        "zip": {
          "type": "integer"
        }
      }
    }
  }
}
```

Wanting an even simpler road to get started with a [JSON Schema](http://json-schema.org)
and inspired by [Orderly JSONSchema](http://lloyd.io/orderly-jsonschema/) this project was born.

As strings are the most common form of data, the default type is ```string```

```
{
  name,
  email,
  role,
  address {
    streetAddress,
    city,
    zip.int
  }
}
```

Note we know that ```address``` is an object due to the ```{ }```.
Also see the ```.``` on zip, defining the type ```int``` or integer.

As [Lloyd](http://lloyd.io/orderly-jsonschema/) points out, _"JSON Schema is intended to provide validation, documentation, and interaction control of JSON data"._

For default values we add a ```:``` like ```role:member``` so it feels more like regular JSON.

For ```maxLength``` we add ```(```amount```)``` like ```name(80)```.

For ```minimum``` and ```maxium``` we add ```[```minimum```..```maximum```]``` like ```zip[00000..99999]``` the ```..``` helps us distinguish between a range and an array.

For ```enum``` we use ```[```value```|```value```]``` like ```role[user|admin]```where the pipe ```|``` distinguishes between it and an array.

For a ```required``` field we start with ```!``` like ```!name```.

So if we want to be more verbose to begin with we can have:

```
{
  !name(80),
  !email(320)
  role[user|admin]:user,
  address {
    streetAddress(120),
    city(80),
    zip.int[00000..99999]
  }
}
```

and then get

```
{
  "type": "object",
  "name": "root",
  "title": "Root",
  "properties": {
    "address": {
      "type": "object",
      "name": "address",
      "title": "Address",
      "description": "Address",
      "properties": {
        "name": {
          "type": "string",
          "name": "name",
          "title": "Name",
          "description": "Name",
          "minLength": 0,
          "maxLength": 800
        },
        "email": {
          "type": "string",
          "name": "email",
          "title": "Email",
          "description": "Email",
          "minLength": 0,
          "maxLength": 320
        },
        "role": {
          "type": "string",
          "name": "role",
          "title": "Role",
          "description": "Role",
          "enum": ["user", "admin"],
          "default": "user"
        },
        "streetAddress": {
          "type": "string",
          "name": "streetAddress",
          "title": "Street Address",
          "description": "Address Street Address",
          "minLength": 0,
          "maxLength": 120,
          "default": "123 Somewhere Ave"
        },
        "city": {
          "type": "string",
          "name": "city",
          "title": "City",
          "description": "Address City",
          "minLength": 0,
          "maxLength": 80,
          "default": "Boise"
        },
        "zip": {
          "type": "integer",
          "name": "zip",
          "title": "Zip",
          "description": "Address Zip",
          "minimum": 0,
          "maximum": 99999
        }
      },
      "required": [
        "name",
        "email"
      ]
    }
  }
}
```

## Types

Regular types

- ```.str``` for a string (this is the default and include injection filtering)
- ```.*``` for a raw string
- ```.int`` or ```#``` for an integer
- ```.num``` or ```..``` for a number
- ```.bool``` or ```?``` for boolean
- ```{ }``` wraps objects
- ```[ ]``` wraps arrays unless containing pipe ```|``` lists or numeric ```..``` ranges

Special types

- ```.email``` or ```@``` for a 320 character string with email address regular expression pattern
- ```.url``` or ```/``` for a 255 character string with a URL regular express pattern

## License

MIT
