# BARTOC.org

> Relaunch of BARTOC.org web interface

This repository contains the [new web interface of BARTOC.org](https://bartoc.org), run by [VZG](https://www.gbv.de/).

## Functional Requirements

* Keep all BARTOC URIs and most URLs
* Provide a nice web interface, possibly multilingual
* Use JSKOS API as backend
* Serve HTML, JSKOS, and RDF

## Install

## Requirements

Requires at least Node.js 10 and an instance of [jskos-server](https://github.com/gbv/jskos-server) to connect to. Additional dependencies are listed in `package.json`.

## Install from sources

~~~sh
git clone https://github.com/gbv/bartoc.org.git
cd bartoc.org
npm install
~~~

## Setup

### Create indexes (in jskos-server)

Only necessary if not performed before.

```bash
npm run import -- --indexes
```

### Import database dump (in jskos-server)

Import a backup/dump of concept schemes, e.g.:

```bash
npm run import -- schemes http://bartoc.org/data/dumps/latest.ndjson
```

### Import auxilary vocabularies

```bash
./bin/import.sh $DIRECTORY_OF_YOUR_JSKOS_SERVER
```

### Transform legacy data

*this step only documented for historical reasons, don't run this*

Before relaunch in October 2020 the Drupal export from September 2020 had to be transformed and (requires Perl >= 5.14 without additional modules):

~~~sh
npm run data
~~~

Then import the resulting file `cache/vocabularies.ndjson` and additional vocabulary files into your jskos-server instance (resetting all stored vocabularies and concepts!):

~~~
./bin/import-legacy.sh $DIRECTORY_OF_YOUR_JSKOS_SERVER
~~~

## Configuration

Basic configuration is located in `config/config.default.json`. Selected fields can be overridden in a local `config/config.json`. The latter should at least include a link to a JSKOS server instance, e.g.:

~~~json
{
  "backend": {
    "provider": "ConceptApi",
    "api": "http://localhost:3000/"
  }
}
~~~

## Run for testing

~~~sh
npm run dev
~~~

The application is made available at <http://localhost:3883/>.

## Deployment

The application is deployed at <http://bartoc.org/>.

Update an existing installation:

~~~sh
git pull
npm install
pm2 restart bartoc.org
~~~

Note that `NODE_ENV` has to be set to `production`, otherwise Vue files will be requested from the dev server. This is given when using `npm run start`.

## Database dumps

To regularly update dumps, add a cronjob with command `npm run dump update`. Dumps will be placed in directory `data/dumps`. To compare two dump files run `npm run dump diff`.
