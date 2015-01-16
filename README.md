## Data validation library for `golang´

This library can be used to validate [most data types](https://godoc.org/github.com/gima/govalid/v1) supported by golang. Custom validators can be used where the supplied ones are not enough.

#### Documentation

* Introduction below
* [![godoc](https://img.shields.io/badge/godoc-reference-5976b1.svg?style=flat-square)](https://godoc.org/github.com/gima/govalid/v1)

You can also take a look at the "[tests/](https://github.com/gima/govalid/tree/master/v1/tests)" folder. (Sorry. If you feel more documentation is needed, please open an issue.)

#### Import

```go
import v "github.com/gima/govalid/v1"
```

#### Intro & Usage

Create validator:

```go
schema := v.Object(
	v.ObjKV("status", v.Boolean()),
	v.ObjKV("data", v.Object(
		v.ObjKV("token", v.Function(myValidatorFunc)),
		v.ObjKV("debug", v.Number(v.NumMin(1), v.NumMax(99999))),
		v.ObjKV("items", v.Array(v.ArrEach(v.Object(
			v.ObjKV("url", v.String(v.StrMin(1))),
			v.ObjKV("comment", v.Optional(v.String())),
		)))),
		v.ObjKV("ghost", v.Optional(v.String())),
		v.ObjKV("ghost2", v.Optional(v.String())),
		v.ObjKV("meta", v.Object(
			v.ObjKeys(v.String()),
			v.ObjValues(v.Or(v.Number(v.NumMin(.01), v.NumMax(1.1)), v.String())),
		)),
	)),
)
```

Validate some data using the schema:

```go
if path, err := schema.Validate(data); err == nil {
	t.Log("Validation passed.")
} else {
	t.Fatalf("Validation failed at %s. Error (%s)", path, err)
}
```

```go
// Example of failed validation:

// Validation failed at Object->Key[data].Value->Object->Key[debug].Value->Number.
// Error (expected (*)data convertible to float64, got bool)
```

## Similar libraries

`Go` [check](https://github.com/pengux/check)  
`Javascript` [js-schema](https://github.com/molnarg/js-schema), [jsonvalidator](https://code.google.com/p/jsonvalidator/)  
`Python` [voluptuous](https://pypi.python.org/pypi/voluptuous), [json_schema](https://pypi.python.org/pypi/json_schema)  
`Ruby` [json-schema](https://rubygems.org/gems/json-schema)

Original idea for jsonv (version 0 of this library, before rename) loosely based on [js-schema](https://github.com/molnarg/js-schema), thank you.

## License

http://unlicense.org (see UNLICENSE.txt). Mention of origin would be appreciated.

*jsonv, jsonv2, json validator, json validation, alternative, go, golang*
