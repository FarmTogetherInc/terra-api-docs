# GET /api-reference

Machine-readable reference of all facade endpoints: URI patterns, HTTP methods,
query parameters and JSON Schemas (draft-07) of request payloads and responses.
Generated at runtime from Spring handler mappings (cached after first call).

## Query parameters

None.

## Response

`List<EndpointInfo>`:

| Field | Type | Notes |
|---|---|---|
| `uri` | string? | single pattern, prefixed with `/api/ro` |
| `uris` | list&lt;string&gt;? | multiple patterns (2+) |
| `methods` | list | HTTP methods (`GET`, `POST`, ...) |
| `queryParams` | list | `{"name", "type"}` — `@RequestParam` (explicit and implicit) and properties of form-bound objects (flattened one level); `Map` catch-alls omitted |
| `payloadSchema` | object? | JSON Schema draft-07 of the `@RequestBody` type |
| `responseSchema` | object? | JSON Schema draft-07 of the response type (`ResponseEntity<T>` unwrapped to `T`; `void` omitted) |

Required semantics: a property listed in `required` corresponds to a non-nullable
Kotlin constructor property of the DTO; nullable properties are optional (not
required). JTS geometry fields are labeled compactly as
`{"type": "object", "description": "GeoJSON::Point — https://geojson.org/schema/GeoJSON.json"}`
(the wire format is GeoJSON produced by jackson-datatype-jts; see the linked
canonical schema).

Source of truth: `facade/src/main/kotlin/com/farmtogether/api/reference/ApiReferenceController.kt`.

```kotlin
@RestController
class ApiReferenceController(
    @Qualifier("requestMappingHandlerMapping")
    val handlerMapping: RequestMappingHandlerMapping,
    objectMapper: ObjectMapper,
) {
    data class EndpointQueryParam(
        val name: String,
        val type: String,
    )

    @JsonInclude(JsonInclude.Include.NON_EMPTY)
    data class EndpointInfo(
        val uri: String?,
        val uris: List<String>?,
        val methods: List<RequestMethod>,
        val queryParams: List<EndpointQueryParam>,
        /** JSON Schema (draft-07) of the request body */
        val payloadSchema: JsonNode?,
        /** JSON Schema (draft-07) of the response */
        val responseSchema: JsonNode?,
    )

    private val cachedReference: List<EndpointInfo> by lazy { buildApiReference() }

    @GetMapping("/api-reference")
    fun getApiReference(): List<EndpointInfo> = cachedReference
}
```

Notes:

- The list covers all facade controllers (`com.farmtogether.*`), not only the
  read-only allowlist; access is still restricted by the gateway allowlist.
- URIs are shown with the `/api/ro` prefix regardless of exposure — an endpoint
  absent from the gateway allowlist returns `403` for external callers.
- Schemas are built with victools `jsonschema-generator` + `JacksonModule` on the
  application `ObjectMapper` (KotlinModule, JavaTimeModule, JTS/GeoJSON), so they
  match the actual wire format.
- Endpoints whose handler method is annotated `@ExcludeFromApiReference`
  (facade-internal) are omitted from the list.
