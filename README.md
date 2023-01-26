# Github Action: Validate JSON
A GitHub Action that validates JSON files based on a JSON Schema.

This project uses [`ajv`](https://github.com/epoberezkin/ajv), fast JSON schema validator, to perform the validation. 

> **Note by masaliev**: 
> In our fork, I made it possible to use it without docker. Collected all files in `/dist/index.js` using [vercel/ncc](https://github.com/vercel/ncc) and specified the path to this file in `action.yml`.
>
> After the changes, you need to 
> - run the `npm run nccBuild` command
> - create a tag `git tag v1.0.4`
> - push it `git push origin v1.0.4`
> - where this action is used, specify this tag. `uses: growave-io/validate-json-action@v1.0.4`

## Usage

### Inputs

- `schema`: Relative file path under the repository of a JSON schema file to validate the other JSON files with. Default is: `'./schema.json'`.
- `jsons`: One or more relative file paths under the repository (seperated by comma) of the JSON files to validate with the schema provided.

> Note: `schema` is **required**, otherwise default will be used.

### Outputs

- `invalid`: One or more of relative file paths of the invalid JSON files, found in the repository (seperated by comma).

### Example Workflow

An example `.github/workflows/validate.yml` workflow to run JSON validation on the repository: 

```yaml
name: Validate JSONs

on: [pull_request]

jobs:
  verify-json-validation:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - name: Validate JSON
        uses: docker://orrosenblatt/validate-json-action:latest
        env:
          INPUT_SCHEMA: /path/to/schema.json
          INPUT_JSONS: /path/to/file.json,/path/to/another/file.json
```


## TODOs

- [x] Initial validation action
- [x] Docker
- [ ] Automate release process
- [ ] Github workflow screenshots
- [ ] Support `exclude` & `include` in PRs (JSONs from pattern)
- [ ] Support `schema` & `jsons` by external reference (from S3/GitHub/etc...)
- [ ] Support `delimiter` input 