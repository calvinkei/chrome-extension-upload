# chrome-extension-upload
This Action allows you to automatically upload and publish browser extensions to the Chrome web store.

## Input variables

You must provide:

- `file-path`
  - The path to the zip file. ex) `dist/hoge.zip`
- `extension-id`
- `client-id`
- `refresh-token`

Optional Arguments

- `glob`
  - If you set it to true, you can specify the file as a glob pattern.
  - Please note that only the first match will be uploaded.


Want to know how to make a CLIENT ID, etc.?  
[Reference link](https://github.com/DrewML/chrome-webstore-upload/blob/master/How%20to%20generate%20Google%20API%20keys.md)

## Usage

Simple example:

```yaml
name: Publish

on:
  push:
    tags:
      - '*'

jobs:
  build:
    name: Publish webextension
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v1
    - uses: actions/setup-node@v1
      with:
        node-version: 12
    - name: Build
      run: |
        npm ci
        npm run build
    - name: Upload & release
      uses: mnao305/chrome-extension-upload@1.1.1
      with:
        file-path: dist/file.zip
        extension-id: hogefuga(extension id)
        client-id: ${{ secrets.CLIENT_ID }}
        refresh-token: ${{ secrets.REFRESH_TOKEN }}
```

Example with `glob`:

```yaml
name: Publish

on:
  push:
    tags:
      - '*'

jobs:
  build:
    name: Publish webextension
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v1
    - uses: actions/setup-node@v1
      with:
        node-version: 12
    - name: Build
      run: |
        npm ci
        npm run build
    - name: Upload & release
      uses: mnao305/chrome-extension-upload@1.1.1
      with:
        file-path: dist/*.zip
        extension-id: hogefuga(extension id)
        client-id: ${{ secrets.CLIENT_ID }}
        refresh-token: ${{ secrets.REFRESH_TOKEN }}
        glob: true
```
