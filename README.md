# contexte-api

Documentation of the public, read-only API of [Contexte Tech](https://contextetech.com/en/): a catalogue of resources to adapt AI models (prompts, datasets, guardrails, evaluations, MCP servers). No account and no key are needed to read.

Base URL: `https://contextetech.com/api`

## Examples

Search resources (filters: `q`, `kind`, `clang`, `tag`, `license`, `task`, `sort`, `page`):

```bash
curl "https://contextetech.com/api/resources?q=gdpr&kind=dataset"
```

Read one resource by its identifier, or by the short code at the end of its page address:

```bash
curl https://contextetech.com/api/resources/contextetech--anonymisation-rgpd-fr
curl https://contextetech.com/api/resources/by-short/jr6t0n
```

Download the file of a resource (JSONL for a dataset, JSON otherwise):

```bash
curl -L -o data.jsonl https://contextetech.com/api/resources/contextetech--anonymisation-rgpd-fr/download
```

Estimated cost per model, dataset quality, versions:

```bash
curl https://contextetech.com/api/resources/contextetech--anonymisation-rgpd-fr/cost
curl https://contextetech.com/api/resources/contextetech--anonymisation-rgpd-fr/quality
curl https://contextetech.com/api/resources/contextetech--anonymisation-rgpd-fr/versions
```

The same calls in Python, without dependencies:

```python
import json, urllib.request

url = "https://contextetech.com/api/resources?q=gdpr&kind=dataset"
page = json.load(urllib.request.urlopen(url))
for item in page["items"]:
    print(item["id"], "-", item["description"])
```

For Python projects, the [contexte](https://pypi.org/project/contexte/) package wraps these calls (`pip install contexte`).

## Endpoints

| Method | Path | Description |
|---|---|---|
| GET | `/resources` | Search and list resources |
| GET | `/resources/{id}` | One resource (add `?version=` for an older version) |
| GET | `/resources/by-short/{code}` | One resource from its short code |
| GET | `/resources/{id}/download` | File of the resource |
| GET | `/resources/{id}/cost` | Input tokens and estimated cost per model |
| GET | `/resources/{id}/quality` | Dataset checks: duplicates, empty answers, lengths, languages |
| GET | `/resources/{id}/versions` | Versions, newest first |
| GET | `/resources/{id}/versions/{n}/diff` | Changes of a version |
| GET | `/resources/{id}/lineage` | Origin and duplicates of a resource |
| GET | `/facets`, `/tags` | Available filters and tags |
| GET | `/prices` | Public model prices used for cost estimates |

The full specification is in [openapi.json](openapi.json) (OpenAPI 3). Publishing requires a personal token and is described in the specification.

## License

The documentation in this repository is under the MIT license. Each resource of the catalogue has its own license, shown in its `license` field.
