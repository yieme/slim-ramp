# slim-ramp <img src="logo/slim-ramp-128.png" align="right">

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

and return [JSON Schema](http://json-schema.org)

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
  !name,
  !email(320)
  role[user|admin]:user,
  address {
    streetAddress(120),
    city,
    zip.int[00000..99999]
  }
}
```

and get [JSON Schema](http://json-schema.org)

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
          "maxLength": 80
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

## Special Attributes

- ```!fieldname``` field is required
- ```fieldname^``` field is hidden
- ```$fieldname``` define the object as a special reference type

## Types

#### Regular types

- ```.str``` for an 80 character string (default)
- ```.*``` for a raw string
- ```.int`` or ```#``` for an integer
- ```.num``` or ```..``` for a number
- ```.bool``` or ```?``` for boolean
- ```{ }``` wraps objects
- ```[ ]``` wraps arrays unless containing pipe ```|``` lists or numeric ```..``` ranges

#### Special string types

- ```.upper``` for an uppercase string
- ```.lower``` for a lowercase string
- ```.camel``` for a camelcase string
- ```.dash```  for a dasherize string
- ```.under``` for an underscore string
- ```.alpha``` for an alpha string
- ```.alphanum``` for an alphanumeric string
- ```.md``` for a markdown string (default length 8k)
- ```.email``` or ```@``` for a 320 characters with email address regular expression pattern
- ```.url``` or ```/``` for a 255 character string with a URL regular express pattern
- ```.pwd``` for a 40 character password
- ```.tel``` for a phone number
- ```.date``` for a date
- ```.time``` for a time
- ```.stamp``` for a date time stamp
- ```.color``` for a css color
- ```.ssn``` for a social security number string
- ```.country``` 2 character country validated ISO country code
- ```.base64``` base64 encoded string

#### Special object types

- ```.geo``` for object ```{ latitude:num, longitude:num```}
- ```.usaddr``` for object ```{ !address1, address2, !city, !state, !zip[00000..99999] }```
- ```.address``` for object ```{ !address1, address2, !city, region, postal, !country }```

> Note: Strings include injection filtering unless otherwise constrained

## Special field names

Field name that contain the following words will have the specified defaults

- ```address``` will have a default length of 120
- ```email``` will have a default type of ```.email```
- ```phone``` will have a default type of ```.tel```
- ```sms``` will have a default type of ```.tel```
- ```date``` will have a default type of ```.date```
- ```time``` will have a default type of ```.time```
- ```markdown``` will have a default type of ```.md```
- ```country``` will have a default type of ```.country```

## License

MIT
