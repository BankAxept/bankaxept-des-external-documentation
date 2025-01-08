# BankAxept Issuer DES Web Service Interface v1.2

This project contains two OpenAPI Specification-files for the API served by the Issuer and the DES.
OpenAPI Specification (formerly Swagger Specification) is an API description format for REST APIs. The
filenames are from DES point of view. So the file describing the operations provided by the DES is
named ```Messages-from-the-issuer.yaml```, while the file describing operations provided by the
Issuer is named ```Messages-to-the-issuer.yaml```. The reference for these files can be found
on the BankAxept web page.

The files are found in the ```des-web-service-interface``` directory and can be used as a reference
to integrate with the BankAxept DES. It is also possible to use these files as input to generating
code, both for creating a client to access these interfaces and to create server stubs, which can
be used to create server side implementation.

## des-api-client

This contains client code accessing the api provided by the DES. The code generated is based on
Java and the Jersey REST framework. This code can be used as an example on how to access the
DES Web Service Interface.

## des-api-server-stub

This contains server interfaces defining the operations that provided by the DES. The code generated
is based on java and the Spring MVC framework. This code can be used to set up a mock-implementation
for testing own code.

## issuer-api-client

This contains client code accessing the api provided by the DES. The code generated is based on
Java and the Jersey REST framework. This code can be used as an example on how to access the
operations provided by the issuer.

## issuer-api-server-stub

This contains server interfaces defining the operations that provided by the issuer. The code generated
is based on java and the Spring MVC framework. This code can be used to set up a mock-implementation
for testing own code.

## Release

Snapshot versions are not supported, only releases.

In the pull request:

* Update the code/yaml-files
* Increment version in the `version.txt`
* Update dependencies by executing `mvn versions:use-latest-versions`

After the pull request is approved and merged the pipeline will build and publish a new release version.

## MkDocs

This repository is also used to generate the publicly hosted documentation for the DES API. The documentation is written in Markdown and is generated using
MkDocs.
The documentation is found in the `docs` directory. The documentation is generated and published to GitHub Pages (TODO Task:DOM-1930).

Mkdocs installation instructions can be found [here](https://www.mkdocs.org/user-guide/installation/).

To host the MKDocs locally, run the following command in the root directory of the repository:

```bash
mkdocs build --strict
mkdocs serve
```

Note that depending on your configuration you might have to run Mkdocs in a Python virtual environment.
Often you would do this by running the following commands:

```bash
source ~/.python/bin/activate
```
