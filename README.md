# Apache JMeter Benchmark on Saft-Property-Helper with Docker-Compose

This Docker composition contains: a Virtuoso store, a Loader to import some RDF data, a TitleCache (based on (based on [Saft](http://safting.github.io/) and [Zend Cache](https://zendframework.github.io/zend-cache/)) with several Cache-Backends (Memcached, MongoDB, APC and Redis) and the Apache JMeter Benchmarking Tool.

Run JMeter Benchmark on the TitleCache with:

    docker-compose up

This will start Virtuoso, import data from `./data/import/`, init the TitleCache and start a Jmeter Benchmark.

Currently JMeter tests two things:

1. createindex - call the TitleCache with the create action, to create a cache
2. fetchvalues - fetch titles for uris. 

## Multiple Tests

To automatic run multiple tests, you can use the `test.sh` script. For example, run:

`./test.sh 10`

to start 10 tests. All results will moved to `./data/results/test-[number]`


## Set the tested Cache backends

Edit the environment variable `CACHES` in  `docker-compose.yml` at section jmeter. The following backends are possible:

```
  environment:
     - CACHES=filesystem memcached apc mongodb redis
```


## Import RDF Data

Copy your RDF data (nt, ttl, ...) file to `./data/import/`. By default the Graph-URI get extracted from the filename. If you need a specific Uri, create a file `[filename].[extension].graph` which containg just the graph uri (see example `swdf.nt.graph`).

## Set fetched titles

Edit the payload files in `./data/config/jmeter/` for your requirements. Each line contains an uri to fetch its title.

## Read the results

See `./data/results/` for the JMeter Benchmark results.

The most important columns here are:

- `elapsed` - Time elapsed for the request in ms
- `label` - Label for the request (contains the action:createindex/fetchvalues and the size of payload)
- `success` - If true, the request successed