## OpenAPI Patcher

The OpenAPI patcher can be used to fix all currently known issues within the MOBIDROM Datenplattform JSON schema.

Fixing the schema allows for several use case. Most important ones:

* JSON schema validation
* Automatically generated models for handling data objects

### Preperation

As the `openapi-patcher` will generate both JSON schema and an OpenAPI specification, both files are required.

Notice that the `openapi-patcher` assumes, that OpenAPI specification contains the actions, while the JSON schema files contains the schemas. This follows the current state of the source repository.

Paths can be specified by command line with default paths:

* `api.yaml`: OpenAPI specification of the KStore. This should't change between releases.
* `schema.json`: Latest JSON schema of the `data-model`. This might change every release.

### Bereinigung des Schemas via openapi-patcher

Da das offizielle JSON-Schema der DatenPlattform (Materna) Korrekturen benötigt, kann es nicht direkt verwendet werden. Beispielsweise wird `mobidp.common.Geometry` in der Datenplattform nicht direkt genutzt, sondern durch LocationTech [JTS (Java Topology Suite)](https://github.com/locationtech/jts) ersetzt, weshalb das Objekt im Ursprungsschema nicht korrekt definiert ist.
Das Schema muss daher vorab über das Tool `openapi-patcher` angepasst werden.

#### Ausführung von openapi-patcher

Die Parameterübergabe erfolgt über `sbt run`. Nachfolgende Argumente überschreiben die Standardwerte (`-a api.yaml -j schema.json -o patched-`):

| Langer Parameter | Kurzer Parameter | Standardwert | Beschreibung |
| --- | --- | --- | --- |
| `--openapi-spec` | `-a` | `api.yaml` | Pfad zur OpenAPI-Spezifikationsdatei |
| `--json-schema` | `-j` | `schema.json` | Pfad zur JSON-Schema-Datei |
| `--output` | `-o` | `patched-` | Präfix oder Ausgabe-Verzeichnis |

##### Aufrufbeispiele

* Lange Option: `sbt "run --openapi-spec my-api.yaml --json-schema my-schema.json --output out/"`
* Kurze Option: `sbt "run -a api.yaml -j schema.json -o patched-"`

### Usage

This is a normal sbt project. You can compile code with `sbt compile`, run it with `sbt run`, and `sbt console` will start a Scala 3 REPL.

For more information on the sbt-dotty plugin, see the
[scala3-example-project](https://github.com/scala/scala3-example-project/blob/main/README.md).

#### Binary

The project is configured to compile using Scala Native.
Scala Native will therefore generate a native binary (`target/scala-<version>/openapi-patcher`), that does not require a Java Runtime.

As the OpenAPI patcher shouldn't change frequently, the binary can be built once and used to patch multiple JSON schema files.
