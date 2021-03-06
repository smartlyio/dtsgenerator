# dtsgenerator

TypeScript d.ts file generator from JSON Schema file or OpenAPI(Swagger) spec file.

[![nodejs version](https://img.shields.io/node/v/dtsgenerator.svg)](#)
[![npm version](https://badge.fury.io/js/dtsgenerator.svg)](https://www.npmjs.com/package/dtsgenerator)
[![build status](https://travis-ci.org/horiuchi/dtsgenerator.svg?branch=master)](https://travis-ci.org/horiuchi/dtsgenerator)
[![Coverage Status](https://coveralls.io/repos/github/horiuchi/dtsgenerator/badge.svg?branch=master)](https://coveralls.io/github/horiuchi/dtsgenerator?branch=master)
[![Greenkeeper badge](https://badges.greenkeeper.io/horiuchi/dtsgenerator.svg)](https://greenkeeper.io/)
[![npm download count](https://img.shields.io/npm/dt/dtsgenerator.svg)](https://www.npmjs.com/package/dtsgenerator)
[![MIT license](https://img.shields.io/npm/l/dtsgenerator.svg)](#)

## Table of Contents

- [Install](#install)
- [Usage](#usage)
- [Advance Usage](#advance-usage)
- [Development](#development)
- [ChangeLog](#changelog)
- [License](#license)

## Install

    npm install -g dtsgenerator

- [Releases](https://github.com/horiuchi/dtsgenerator/releases)

## Usage

```
$ dtsgen --help
Usage: script [options] <file ... | file patterns using node-glob>

Options:

  -V, --version                output the version number
  --url <url>                  input json schema from the url. (default: )
  --stdin                      read stdin with other files or urls.
  -o, --out <file>             output d.ts filename.
  -n, --namespace <namespace>  use root namespace instead of definitions or components.schema from OpenAPI, or -n "" to suppress namespaces.
  -h, --help                   output usage information

  Examples:

    $ dtsgen --help
    $ dtsgen --out types.d.ts schema/**/*.schema.json
    $ cat schema1.json | dtsgen
    $ dtsgen -o swaggerSchema.d.ts --url https://raw.githubusercontent.com/OAI/OpenAPI-Specification/master/schemas/v2.0/schema.json
    $ dtsgen -o petstore.d.ts -n PetStore --url https://raw.githubusercontent.com/OAI/OpenAPI-Specification/master/examples/v2.0/yaml/petstore.yaml
```

## Advance Usage

For customize the output type name.

### Install for library

    npm install -S dtsgenerator

### Usage for library

For example, it want to add the `I` prefix to the interface name.
This is not usual example...

https://github.com/horiuchi/dtsgenerator/blob/master/example/add-prefix/index.ts
```js
import dtsGenerator, { DefaultTypeNameConvertor, SchemaId } from 'dtsgenerator';
import * as fs from 'fs';

const filePath = '../../test/snapshots/json-schema-draft-04/schema/schema.json';

function typeNameConvertor(id: SchemaId): string[] {
    const names = DefaultTypeNameConvertor(id);
    if (names.length > 0) {
        const lastIndex = names.length - 1;
        names[lastIndex] = 'I' + names[lastIndex];
    }
    return names;
}

async function main(): Promise<void> {
    const content = JSON.parse( fs.readFileSync(filePath, 'utf-8') );
    const result = await dtsGenerator({
        contents: [content],
        typeNameConvertor,
    });
    console.log(result);
}
main();
```

## Development

### Debug

Output debug message by [debug](https://www.npmjs.com/package/debug) library.

    $ DEBUG=dtsgen dtsgen schema/news.json


### Links about JSON Schema and Swagger

- [The home of JSON Schema](http://json-schema.org/)
- [The OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification)

### Supported spec and features

- JSON Schema
    - Draft-04 and before
    - Draft-07 and before
- OpenAPI
    - OpenAPI Specification version 2.0
    - OpenAPI Specification version 3.0

- [supported features in these spec](https://github.com/horiuchi/dtsgenerator/blob/master/SupportedFeatures.md)

## ChangeLog

### [v2.0.1](https://github.com/horiuchi/dtsgenerator/releases/tag/v2.0.1) (2018-12-21)

- fixed
  - A property name cannot contain a "/" #315. Thank you @Reggino :+1:

### [v2.0.0](https://github.com/horiuchi/dtsgenerator/releases/tag/v2.0.0) (2018-10-22)

- features
  - Add to support emit the Request/Response object schema of OpenAPI by #304.

### [v1.2.0](https://github.com/horiuchi/dtsgenerator/releases/tag/v1.2.0) (2018-09-12)

- features
  -  Add `--namespace <namespace>` option to control the emitted namespace. Thank you @DavidBiesack :+1:

### [v1.1.0](https://github.com/horiuchi/dtsgenerator/releases/tag/v1.1.0) (2018-06-01)

- features
    - Add [the supported features document](https://github.com/horiuchi/dtsgenerator/blob/master/SupportedFeatures.md)
    - Support some properties
        - `title`, `const`, `readOnly`

### [v1.0.0](https://github.com/horiuchi/dtsgenerator/releases/tag/v1.0.0) (2018-03-22)

- features
    - Support JSON Schema Draft-07 and OpenAPI v3
    - Add the library interface for customize type name
    - Remove some unusual command line options
    - And rebuild the architecture
- others
    - Remove the node v4 support

### other versions

[ChangeLogs](https://github.com/horiuchi/dtsgenerator/blob/master/CHANGELOG.md)

## License

`dtsgenerator` is licensed under the MIT license.

Copyright &copy; 2016-2018, Hiroki Horiuchi

