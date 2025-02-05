# OpenSRS

[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/voxxit/opensrs?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[![Build Status](https://travis-ci.org/voxxit/opensrs.svg?branch=master)](https://travis-ci.org/voxxit/opensrs)
[![Code Climate](https://codeclimate.com/github/voxxit/opensrs/badges/gpa.svg)](https://codeclimate.com/github/voxxit/opensrs)
[![Coverage Status](https://img.shields.io/coveralls/voxxit/opensrs.svg)](https://coveralls.io/r/voxxit/opensrs)

This (unofficial) OpenSRS gem provides basic support to connect to, and utilize the [OpenSRS API.](http://www.opensrs.com/site/resources/documentation/api) This library has been well-tested in high-performance production
environments.

### Requirements

  * Ruby **>= 1.9.2** or **>= 2.0.0** or **>= 2.1.2**

### Installation

You can install this gem by doing the following:

    $ gem install opensrs

You can then include it in a Ruby project, like so:

    require 'opensrs'

For Rails 3.x and above, add it to the `Gemfile`:

    gem 'opensrs'
    gem 'nokogiri'    # if you want to use nokogiri as the XML parser

### Usage

This library provides basic functionality for interacting with the OpenSRS XML API.

- Connection handling
- Error reporting
- XML encoding
- XML decoding

To connect, instantiate a new <tt>OpenSRS::Server</tt> object:

    server = OpenSRS::Server.new(
      server:   "https://rr-n1-tor.opensrs.net:55443/",
      username: "testing",
      password: "53cr3t",
      key:      "c633be3170c7fb3fb29e2f99b84be2410..."
    )

**NOTE:** Connecting to OpenSRS requires that you add the IP(s) you're connecting from to their whitelist. Log in to the testing or production servers, and add your IP(s) under Profile Management > Add IPs for Script/API Access. IP changes take about one hour to take effect.

Since OpenSRS only allows up to 5 IPs to be whitelisted, you may wish to use a proxy. `OpenSRS::Server` uses `Net::HTTP`, so you can configure an unauthenticated proxy with the `http_proxy` environment variable. If the proxy requires authentication, you must supply it explicitly:

    server = OpenSRS::Server.new(
      server:   "https://rr-n1-tor.opensrs.net:55443/",
      username: "testing",
      password: "53cr3t",
      key:      "c633be3170c7fb3fb29e2f99b84be2410...",
      proxy:    "http://user:password@example.com:8080",
    )

If you would like, you can provide OpenSRS::Server with a logger to write the contents of the requests and responses
with OpenSRS. The assumption is you are using a Rails-like logger, but as long as your logger has an info method you
are fine. You can simply assign the logger or pass it in as an initialization option:

    server = OpenSRS::Server.new(
      server:        "https://rr-n1-tor.opensrs.net:55443/",
      username:      "testing",
      password:      "53cr3t",
      key:           "c633be3170c7fb3fb29e2f99b84be2410...",
      logger:        Rails.logger,
      sanitize_logs: false
    )

or, if you can change it later:

    server.logger = Rails.logger

Once you have a server connection class, you can build from this to create the methods that you need. For instance, let's say we want to grab our account balance. The OpenSRS XML API takes a couple of attributes for all commands. You can include those here:

    def get_balance
      server.call(
        action: "GET_BALANCE",
        object: "BALANCE"
      )
    end

Sometimes you might need to include attributes for the command, such as a cookie, or the data attributes themselves. You can do this, too:

    def create_nameserver(nameserver)
      server.call(
        action: "CREATE",
        object: "NAMESERVER",
        cookie: "366828736:3210384",
        attributes: {
          name: nameserver.hostname,
          ipaddress: "212.112.123.11"
        }
      )
    end

Responses from OpenSRS are returned in an OpenSRS::Response object, which gives you access to a multitude of things.

  Method | Description
  ---|---
  `response.response` | This gives you the response in a Hash form, which is highly accessible to most other actions in your application.
  `response.response` | This gives you the response in a Hash form, which is highly accessible to most other actions in your application.
  `response.errors` | If there are errors which come back from OpenSRS, they are returned here. If not, it returns nil.
  `response.success?` | Returns true if the response was labeled as successful. If not, it returns false.
  `response.request_xml` | Returns raw request XML.
  `response.response_xml` | Returns raw response XML.

### Bugs/Feature Requests

If you have any bugs or feature requests for this gem, feel free to [open an issue.](http://github.com/voxxit/opensrs/issues/new)

### How To Contribute

  * Fork the project.
  * Make your feature addition or bug fix.
  * Add specs for it.
  * Commit, but **do not mess with Rakefile or version.**
  * Send me a pull request. Bonus points for topic branches!

### Contributors (in order of appearance)

* [Joshua Delsman](http://github.com/voxxit)
* Glenn Roberts

### Copyright

Copyright (c) 2010-2016 Joshua Delsman.

Distributed under the MIT license. See `LICENSE` for details.


[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/voxxit/opensrs/trend.png)](https://bitdeli.com/free "Bitdeli Badge")
