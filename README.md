# cdda-json-formatter-emcc-build

This repository builds JSON formatter for Cataclysm-DDA project into emscripten javascript module and uploads to npm:

https://www.npmjs.com/package/@cdda-toys/cdda-json-formatter-emcc-build

Used by a VS Code extension:

https://github.com/cdda-toys/cdda-json-formatter-vscode-extension/

## License

Keep in mind the license is non-trivial - see LICENSE.txt:
* License for contents of this repository is your choice of public domain or MIT.
* Build output is additionally under license of the Cataclysm-DDA project.

The Cataclysm-DDA project license is appended to the LICENSE.txt in build output.

## Using

Example using typescript ( for javascript strip the typing parts ):

Install types for emscripten (EmscriptenModule, cwrap etc.):
```shell
npm install --save @types/emscripten
```

```typescript
interface FormatterModule extends EmscriptenModule { cwrap: typeof cwrap; }

// possible values for this are in the json_error_output_colors_t enum in CDDA's json.h
const json_error_output_colors_t_no_colors = 1;

const getCddaJsonFormatterModule: () => Promise<FormatterModule> =
    require("@cdda-toys/cdda-json-formatter-emcc-build");

const formatterModule = getCddaJsonFormatterModule();

formatterModule.then(module => {
    const applyFormatter = formatterModule.cwrap("json_format", "string", ["string", "int"]);
    let formattedJson = applyFormatter(unformattedJson, json_error_output_colors_t_no_colors);
}
```

A `FormatterModule` will also have standard stuff of an emscripten module, for more info [see emscripten docs](https://emscripten.org/docs/porting/connecting_cpp_and_javascript/Interacting-with-code.html#interacting-with-code)

## Installing

```shell
npm install --save @cdda-toys/cdda-json-formatter-emcc-build
```

## Compiling

For manual compiling execute (mostly) the same steps as the builder agent

[.github/workflows/build.yml](.github/workflows/build.yml)

