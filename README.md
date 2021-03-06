# go-swagger-utils

[![Build Status](https://travis-ci.org/rmohr/go-swagger-utils.svg?branch=master)](https://travis-ci.org/rmohr/go-swagger-utils)

Go generator tools which help creating swagger specs. 

## swagger-doc

This tool can be used to generate descriptions out of struct and field comments
for [go-restful](https://github.com/emicklei/go-restful). See
[go-restful/swagger](https://github.com/emicklei/go-restful/tree/master/swagger)
for more details.

Get the tool:

```bash
go get -u github.com/rmohr/go-swagger-utils/swagger-doc
```

Generate the documentation map for structs in a go file and print it to stdout:

```bash
swagger-doc -in description_generator_test.go -out -
```
This will generate the following output:
```golang
package main

func (Address) SwaggerDoc() map[string]string {
	return map[string]string{
		"":         "Address doc",
		"country":  "Country doc",
		"postcode": "PostCode doc",
	}
}

func (Person) SwaggerDoc() map[string]string {
	return map[string]string{
		"":               "Person doc",
		"firstName":      "FirstName doc",
		"LastName":       "LastName doc",
		"middleName":     "MiddleName doc",
		"HeightInPounds": "Field without tag",
	}
}
```

The original structs look like this:

```golang
//Address doc
type Address struct {
	//Country doc
	Country string `json:"country,omitempty"`
	//PostCode doc
	PostCode int `json:"postcode,omitempty"`
}

//Person doc
type Person struct {
	//FirstName doc
	FirstName string `json:"firstName,omitempty"`
	//LastName doc
	LastName string `json:",omitempty"`
	//MiddleName doc
	MiddleName string `json:"middleName"`

	//Field without tag
	HeightInPounds int
}
```

If `-out` is omitted, the full path and name of the input file is taken and a
`_swagger_generated.go` prefix will be added to it.  So if the input file was
`~/wherever/test.go`, the resulting output file will be
`~/wherever/test_swagger_generated.go`.

`swagger-doc` can be integrated into the `go generate` flow like this:

```golang
package mypackage
//go:generate swagger-doc
```

`swagger-doc` will use the `GOFILE` environment variable to find out on which
file it is invoked by `go generate`.
