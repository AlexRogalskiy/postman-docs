---
title: "Running collections on the command line with Newman"
order: 60
updated: 2021-06-17
page_id: "command_line_integration_with_newman"
search_keyword: "newman run"
contextual_links:
  - type: section
    name: "Prerequisites"
  - type: link
    name: "Using the Collection Runner"
    url: "/docs/running-collections/intro-to-collection-runs/"
  - type: section
    name: "Additional Resources"
  - type: subtitle
    name: "Videos"
  - type: link
    name: "Run Collections with Newman | Postman Level Up"
    url: "https://www.youtube.com/watch?v=SQlwGZj97Y4"
  - type: link
    name: "Using Custom Reporters with Newman"
    url: "https://youtu.be/Nxdxx-VaYno"
  - type: subtitle
    name: "Blog Posts"
  - type: link
    name: "Newman: run and test your collections from the command line"
    url: "https://blog.postman.com/newman-run-and-test-your-collections-from-the-command-line/"
  - type: section
    name: "Next Steps"
  - type: link
    name: "Intro to the Postman API"
    url: "/docs/developer/intro-api/"

warning: false
tags:
  - "newman"

---

Newman is a command-line Collection Runner for Postman. It enables you to run and test a Postman Collection directly from the command line. It's built with extensibility in mind so that you can integrate it with your continuous integration servers and build systems.

Newman maintains feature parity with Postman and allows you to run collections the way they're executed inside the collection runner in Postman.

Newman resides in the [NPM registry](https://www.npmjs.com/package/newman) and on [GitHub](https://github.com/postmanlabs/newman).

[![Running Newman](https://assets.postman.com/postman-docs/newman-running-in-terminal.gif)](https://assets.postman.com/postman-docs/newman-running-in-terminal.gif)

* [Getting started](#getting-started)
* [Options](#options)
* [Example collection with failing tests](#example-collection-with-failing-tests)
* [Using Newman with CI/CD](#using-newman-with-cicd)
* [File uploads](#file-uploads)
* [Library](#library)
* [Custom reporters](#custom-reporters)

## Getting started

Newman is built on Node.js. To run Newman, make sure you have Node.js installed.

You can [download and install](https://nodejs.org/en/download/current/) Node.js on Linux, Windows, and macOS.

After you install Node.js, Newman is just a command away. Install Newman from npm globally on your system, which allows you to run it from anywhere.

```bash
$ npm install -g newman
```

The easiest way to run Newman is to run it with a collection. You can run any collection file from your file system.

> You can [export a collection](/docs/getting-started/importing-and-exporting-data/#exporting-collections) to share as a file.

```bash
$ newman run mycollection.json
```

You can also pass a collection as a URL by [sharing](/docs/collaborating-in-postman/sharing/#sharing-postman-entities) it.

Your collection probably uses environment variables. To provide an accompanying set of [environment variables](/docs/sending-requests/managing-environments/), export the template from Postman and run them with the `-e` flag.

```bash
$ newman run https://www.postman.com/collections/cb208e7e64056f5294e5 -e dev_environment.json
```

## Options

Newman provides a rich set of options to customize a run. You can retrieve a list of options by running it with the ``-h`` flag.

```bash
$ newman run -h
```

### Utility

| Option | Details |
|:--|:--|
| `-h`, `--help` | Output usage information |
| `-v`, `--version` | Output the version number |

### Basic setup

| Option | Details |
|:--|:--|
| `--folder [folderName]` | Specify a single folder to run from a collection. |
| `-e`, `--environment [file\|URL]` | Specify a Postman environment as a JSON [file] |
| `-d`, `--iteration-data [file]` | Specify a data file to use, either JSON or CSV |
| `-g`, `--globals [file]` | Specify a Postman globals file as JSON [file] |
| `-n`, `--iteration-count [number]` | Define the number of iterations to run |

### Request options

| Option | Details |
|:--|:--|
| `--delay-request [number]` | Specify a delay (in ms) between requests [number] |
| `--timeout-request [number]` | Specify a request timeout (in ms) for a request |

### Miscellaneous options

| Option | Details |
|:--|:--|
| `--bail` | Stops the runner when a test case fails |
| `--silent` | Turn off terminal output |
| `--color off` | Turn off colored output (auto\|on\|off) (default: "auto")|
| `-k`, `--insecure` | Turn off strict SSL |
| `-x`, `--suppress-exit-code` | Continue running tests even after a failure, but exit with `code=0` |
| `--ignore-redirects` | Turn off automatic following of `3XX` responses |
| `--verbose` | Show detailed information of collection run and each request sent |

Use the ``-n`` option to set the number of iterations to run the collection.

```bash
$ newman run mycollection.json -n 10  # runs the collection 10 times
```

To provide a different set of data, such as variables for each iteration, you can use the ``-d`` to specify a JSON or CSV file.

For example, a data file such as the one shown below runs _2_ iterations, with each iteration using a set of variables.

```json
[{
    "url": "http://127.0.0.1:5000",
    "user_id": "1",
    "id": "1",
    "token_id": "123123",
},
{
    "url": "http://postman-echo.com",
    "user_id": "2",
    "id": "2",
    "token_id": "899899",
}]
```

```bash
$ newman run mycollection.json -d data.json
```

Here's an example of the CSV file for the above set of variables:

```bash
url, user_id, id, token_id
http://127.0.0.1:5000, 1, 1, 123123123
http://postman-echo.com, 2, 2, 899899
```

Newman, by default, exits with a status code of 0 if everything runs well, such as without any exceptions.

Continuous integration tools respond to these exit codes and correspondingly pass or fail a build.

You can use `-x` or `--suppress-exit-code` to override the default exit code for the current run.

You can use the `--bail` flag to tell Newman to halt on a test case error with a status code of 1, which can then be picked up by a CI tool or build system.

```bash
$ newman run PostmanCollection.json -e environment.json --bail
```

## Example collection with failing tests

```bash
→ Status Code Test
  GET https://postman-echo.com/status/404 [404 Not Found, 534B, 1551ms]
  1\. response code is 200

┌─────────────────────────┬──────────┬──────────┐
│                         │ executed │   failed │
├─────────────────────────┼──────────┼──────────┤
│              iterations │        1 │        0 │
├─────────────────────────┼──────────┼──────────┤
│                requests │        1 │        0 │
├─────────────────────────┼──────────┼──────────┤
│            test-scripts │        1 │        0 │
├─────────────────────────┼──────────┼──────────┤
│      prerequest-scripts │        0 │        0 │
├─────────────────────────┼──────────┼──────────┤
│              assertions │        1 │        1 │
├─────────────────────────┴──────────┴──────────┤
│ total run duration: 1917ms                    │
├───────────────────────────────────────────────┤
│ total data received: 14B (approx)             │
├───────────────────────────────────────────────┤
│ average response time: 1411ms                 │
└───────────────────────────────────────────────┘

  #  failure        detail

 1\.  AssertionFai…  response code is 200
                    at assertion:1 in test-script
                    inside "Status Code Test" of "Example Collection with
                    Failing Tests"
```

The results of all tests and requests can be exported into a file. Use the JSON reporter and a file name to save the output into a file.

```
$ newman run mycollection.json --reporters cli,json --reporter-json-export outputfile.json
```

> Newman allows you to use all [libraries and objects](/docs/writing-scripts/script-references/postman-sandbox-api-reference/) that Postman supports to run tests and pre-request scripts.

## Using Newman with CI/CD

By default, Newman exits with a status code of 0 if everything runs as expected with no exceptions. You can configure your continuous integration tools to respond to Newman's exit codes and correspondingly pass or fail a build. You can also use the `--bail` flag to make Newman stop the run if it encounters a test case error with a status code of 1, which can then be picked up by your CI tool or build system.

## File uploads

Newman also supports file uploads. For this to work, upload the file in the relative location specified in the collection. For instance, review this collection:

```json
{
    "variables": [],
    "info": {
        "name": "file-upload",
        "_postman_id": "9dbfcf22-fdf4-f328-e440-95dbd8e4cfbb",
        "description": "A set of `POST` requests to upload files as form data fields",
        "schema": "https://schema.getpostman.com/json/collection/v2.0.0/collection.json"
    },
    "item": [
        {
            "name": "Form data upload",
            "event": [
                {
                    "listen": "test",
                    "script": {
                        "type": "text/javascript",
                        "exec": [
                            "var response = JSON.parse(responseBody).files[\"sample-file.txt\"];",
                            "",
                            "tests[\"Status code is 200\"] = responseCode.code === 200;",
                            "tests[\"File was uploaded correctly\"] = /^data:application\\/octet-stream;base64/.test(response);",
                            ""
                        ]
                    }
                }
            ],
            "request": {
                "url": "https://postman-echo.com/post",
                "method": "POST",
                "header": [],
                "body": {
                    "mode": "formdata",
                    "formdata": [
                        {
                            "key": "file",
                            "type": "file",
                            "enabled": true,
                            "src": "sample-file.txt"
                        }
                    ]
                },
                "description": "Uploads a file as a form data field to `https://postman-echo.com/post` via a `POST` request."
            },
            "response": []
        }
    ]
}
```

The file ``sample-file.txt`` must be present in the current working directory as the collection. Run this collection as usual.

```bash
$ newman run file-upload.postman_collection.json
```

## Library

Newman has been built as a library from the ground up. It can be extended and used in various ways. You can use it as follows in your Node.js code:

```javascript
var newman = require('newman'); // require Newman in your project

// call newman.run to pass `options` object and wait for callback
newman.run({
    collection: require('./sample-collection.json'),
    reporters: 'cli'
}, function (err) {
    if (err) { throw err; }
    console.log('collection run complete!');
});
```

## Custom reporters

Custom reporters are useful to generate collection run reports that cater to specific use cases, for example, logging out the response body when a request (or its tests) fail.

## Building custom reporters

A custom reporter is a Node.js module with a name of the form `newman-reporter-<name>`. To create a custom reporter:

1. In the directory of your choice, create a blank npm package with `npm init`.

2. Add an `index.js` file, that exports a function of the following form:

```javascript
function (emitter, reporterOptions, collectionRunOptions) {
  // emitter is is an event emitter that triggers the following events: https://github.com/postmanlabs/newman#newmanrunevents
  // reporterOptions is an object of the reporter specific options. See usage examples below for more details.
  // collectionRunOptions is an object of all the collection run options:
  // https://github.com/postmanlabs/newman#newmanrunoptions-object--callback-function--run-eventemitter
};
```

1. Publish your reporter using `npm publish`, or use your reporter locally. See the usage instructions for more information.

Scoped reporter package names like `@myorg/newman-reporter-<name>` are also supported.

## Using custom reporters

To use the custom reporter, it will have to be installed first. For instance, to use the Newman TeamCity reporter, install the reporter package:

```bash
npm install newman-reporter-teamcity
```

Note that the name of the package is of the form `newman-reporter-<name>`, where `<name>` is the actual name of the reporter. The installation is global if Newman is installed globally, local otherwise. Run `npm install ...` with the `-g` flag for a global installation.

To use local (non-published) reporters, run the command `npm install <path/to/local-reporter-directory>` instead.

Use the installed reporter, either with the command-line tool, or programmatically. Here, the `newman-reporter` prefix isn't required while specifying the reporter name in the options.

Scoped reporter packages must be specified with the scope prefix. For instance, if your package name is `@myorg/newman-reporter-name`, you must specify the reporter with `@myorg/name`.

On the command line:

```bash
newman run /path/to/collection.json -r myreporter --reporter-myreporter-<option-name> <option-value> # The option is optional
```

Programmatically:

```js
var newman = require('newman');

newman.run({
   collection: '/path/to/collection.json',
   reporters: 'myreporter',
   reporter: {
     myreporter: {
       'option-name': 'option-value' // this is optional
     }
   }
}, function (err, summary) {
  if (err) { throw err; }
  console.info('collection run complete!');
});
```

In both cases above, the reporter options are optional.

For the complete documentation, see the [Newman README](https://github.com/postmanlabs/newman).

For more information about collection runs, see:

* [Using the Collection Runner](/docs/running-collections/intro-to-collection-runs/)
* [Working with data files](/docs/running-collections/working-with-data-files/)
* [Building workflows](/docs/running-collections/building-workflows/)
* [Integration with Jenkins](/docs/running-collections/using-newman-cli/integration-with-jenkins/)
* [Integration with Travis CI](/docs/running-collections/using-newman-cli/integration-with-travis/)
* [Newman with Docker](/docs/running-collections/using-newman-cli/newman-with-docker/)
