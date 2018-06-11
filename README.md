<p align="center">
 	<img src="https://indradb.github.io/logo.png">
</p>

# [IndraDB](https://indradb.github.io) [![Build Status](https://travis-ci.org/indradb/indradb.svg?branch=master)](https://travis-ci.org/indradb/indradb) [![Coverage Status](https://coveralls.io/repos/github/indradb/indradb/badge.svg?branch=master)](https://coveralls.io/github/indradb/indradb?branch=master)

A graph database written in rust. This software is in the alpha state.

IndraDB consists of a GraphQL server and an underlying library. Most users would use the GraphQL server, which, for convenience, is available as pre-compiled binaries as releases. But if you're a rust developer that wants to embed a graph database directly in your application, you can use the [library](https://github.com/indradb/indradb/tree/master/lib).

## Features

* Support for directed and typed graphs.
* GraphQL support.
* Support for metadata: key/value data tied to graph items that can be used for supporting things like caching results from graph processing algorithms executed offline.
* Pluggable underlying datastores, with built-in support for in-memory-only and [rocksdb](https://github.com/facebook/rocksdb).
* Written in rust!

IndraDB's original design is heavily inspired by [TAO](https://www.cs.cmu.edu/~pavlo/courses/fall2013/static/papers/11730-atc13-bronson.pdf), facebook's graph datastore. In particular, IndraDB emphasizes simplicity of implementation, and is similarly designed with the assumption that it may be representing a graph large enough that full graph processing is not possible. IndraDB departs from TAO (and most graph databases) in its support for metadata.

For more details, see the [homepage](https://indradb.github.io).

## Getting started

* [Download the latest release for your platform.](https://github.com/indradb/indradb/releases)
* Add the binaries to your `PATH`.
* Start the app: `indradb`

This should start an in-memory-only datastore, where all work will be wiped out when the server is shutdown. You can persist your work with one of the alternative datastores.

### RocksDB

If you want to use the rocksdb-backed datastore, follow these steps:

* Start the server: `DATABASE_URL=rocksdb://database.rdb PORT=8000 indradb-server`.
* Make a sample GraphQL request to `http://localhost:8000`.

## Applications

There's two applications:

* `indradb-server`: For running the GraphQL server.
* `indradb-admin`: For managing databases.

## Environment variables

Applications are configured via environment variables:

* `DATABASE_URL`: The connection string to the underlying database.
* `PORT`: The port to run the server on. Defaults to `8000`.
* `INDRADB_SCRIPT_ROOT`: The directory housing the lua scripts. Defaults to `./scripts`.
* `INDRADB_MAP_REDUCE_QUERY_LIMIT`: How many vertices to query at a time when executing mapreduce tasks. Higher values will consume more memory. Defaults to `10000`.
* `POOL_SIZE`: The number of threads used for various thread pools

## Install from source

If you don't want to use the pre-built releases, you can build/install from source:

* Install [rust](https://www.rust-lang.org/en-US/install.html). IndraDB should work with any of the rust variants (stable, nightly, beta.)
* Make sure you have gcc 5+ installed.
* Clone the repo: `git clone git@github.com:indradb/indradb.git`.
* Build/install it: `cargo install`.

## Running tests

Use `make test` to run the test suite. Note that this will run the full test suite across the entire workspace, including tests for all datastore implementations. Similarly, you can run `make bench` to run the full benchmarking suite.

You can filter which tests run via the `TEST_NAME` environment variable. e.g. `TEST_NAME=create_vertex make test` will run tests with `create_vertex` in the name across all datastore implementations.
