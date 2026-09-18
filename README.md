## OpenAPI Patcher

The OpenAPI patcher can be used to fix all currently known issues within the MOBIDROM Datenplattform JSON schema.

Fixing the schema allows for several use case. Most important ones:

* JSON schema validation
* Automatically generated models for handling data objects

### Preparation

As the `openapi-patcher` will generate both JSON schema and an OpenAPI specification, both files are required.

Notice that the `openapi-patcher` assumes, that OpenAPI specification contains the actions, while the JSON schema files contains the schemas. This follows the current state of the source repository.

Paths can be specified by command line with default paths:

* `api.yaml`: OpenAPI specification of the KStore. This shouldn't change between releases.
* `schema.json`: Latest JSON schema of the `data-model`. This might change every release.

### Cleaning the Schema via openapi-patcher

Since the official JSON schema of the DatenPlattform (Materna) requires corrections, it cannot be used directly. For example, `mobidp.common.Geometry` is not used directly in the data platform, but is replaced by LocationTech [JTS (Java Topology Suite)](https://github.com/locationtech/jts), which is why the object is not correctly defined in the original schema.
Therefore, the schema must be adjusted beforehand using the `openapi-patcher` tool.

### Usage

This is a normal sbt project. You can compile code with `sbt compile`, run it with `sbt run`, and `sbt console` will start a Scala 3 REPL.

For more information on the sbt-dotty plugin, see the
[scala3-example-project](https://github.com/scala/scala3-example-project/blob/main/README.md).

#### Running openapi-patcher

Parameters are passed via `sbt run`. The following arguments override the default values (`-a api.yaml -j schema.json -o patched-`):

| Long Parameter | Short Parameter | Default Value | Description |
| --- | --- | --- | --- |
| `--openapi-spec` | `-a` | `api.yaml` | Path to the OpenAPI specification file |
| `--json-schema` | `-j` | `schema.json` | Path to the JSON schema file |
| `--output` | `-o` | `patched-` | Prefix or output directory |

##### Usage Examples

* Long option: `sbt "run --openapi-spec my-api.yaml --json-schema my-schema.json --output out/"`
* Short option: `sbt "run -a api.yaml -j schema.json -o patched-"`

#### Binary

The project is configured to compile using Scala Native.
Scala Native will therefore generate a native binary (`target/scala-<version>/openapi-patcher`), that does not require a Java Runtime.
To build the binary run:

```sh
sbt nativeLinkReleaseFull
```

As the OpenAPI patcher shouldn't change frequently, the binary can be built once and used to patch multiple JSON schema files.

For a less optimized build use `sbt nativeLink` instead.
