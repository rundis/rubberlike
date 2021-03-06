# Rubberlike

Rubberlike is a [Clojure](http://clojure.org/) library for creating embedded [Elasticsearch](http://www.elasticsearch.org/) servers. Pretty useful for testing if I may say so myself.

Latest version is `[rubberlike "0.2.1"]`.

Rubberlike requires Java >= 1.7 and quite possibly Elasticsearch >= 1.4.x.

Elasticsearch is not included as a dependency in Rubberlike so you would have to pull it in yourself, e.g. `[org.elasticsearch/elasticsearch "1.4.0"]`.

## Usage

All functions reside in the `rubberlike.core` namespace.

`create` creates and starts an embedded Elasticsearch server instance. Invoking `create` with no arguments will create a temporary directory for data storage, as well as bind the server instance to a dynamically allocated port.

Alternatively, `create` can be invoked with a map of configuration parameters. These are:
* `port` to define which port Elasticsearch will bind to.
* `host` to define which host Elasticsearch will bind to.
* `data-dir` pointing to the directory where data is to be stored.
* `temp-data-dir?` which if set to `true` will tell Rubberlike to create a temporary data directory for you.
* `disable-http?` which will stop Rubberlike from launching a HTTP interface for your instance when set to `true`.

The object returned from `create` is to be provided as the sole argument to the following functions.

`port` will give you the server port, which will come in handy if the port is not specified in the call to `create` and thus is dynamically allocated.

There is also a `client` function which will give you an instance of `org.elasticsearch.client.Client` for use with Elasticsearch's native API.

The server can be stopped by calling `stop`. If `temp-data-dir?` in the call to `create` was set to `true`, the data directory will be deleted upon calling `stop`.

Let's summarize this with an example:

```clojure
(require '[rubberlike.core :refer [create stop port]])

;; Create and start a new server.

> (def server (create))
#'user/server

;; Port is dynamically allocated as we didn't specify one. Let's get a hold of it.

> (port server)
9212

;; Assuming we've done our thing with this instance, let's tear it down.

> (stop server)
:stopped
```

## License

Copyright © 2014 Anders Furseth

Distributed under the Eclipse Public License either version 1.0 or (at your option) any later version.
