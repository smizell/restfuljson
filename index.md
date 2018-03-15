---
layout: default
title: Home
---

<div class="tagline">
RESTful JSON is a minimal and pragmatic design pattern for using links to build expressive and evolvable APIs.
</div>

## Overview

For JSON formats conforming to [RFC 7159][], apply the following guidelines:

1. JSON objects MAY include a `url` property to indicate a link to itself
2. JSON objects MAY append `_url` to properties to indicate related links
3. JSON objects MAY append `_urls` to properties to indicate an array of related links

All URLs SHOULD conform to [RFC 3986][]. API providers MAY use camel case rather
than snake case where applicable. A profile link conforming to [RFC 6906][], as
indicated with either a property name of `profile_url` or `profileUrl`, MAY be used to link to
additional documentation about the resource.

The media
type `application/vnd.restful+json` has been registered for this design pattern.

### Example

The JSON below shows a representation for an article.

``` json
{
  "url": "http://example.com/customers/777",
  "first_name": "John",
  "last_name": "Doe",
  "orders": [
    {
      "url": "http://example.com/orders/23222",
      "order_num": "43341",
      "total": "398.55",
      "currency": "USD",
      "address_url": "http://example.com/addresses/337474",
      "product_urls": [
        "http://example.com/product/12359",
        "http://example.com/product/3124",
        "http://example.com/product/98351"
      ]
    }
  ],
  "profile_url": "http://example.com/profile/customer"
}
```

This example demonstrates using `url` in a customer resource object and included 
`orders`. Each order has a linked address as `address_url` and linked products under `product_urls`. The `profile_url` links to more documentation for a customer resource.

## Usage and Guidelines

### API Providers (Server-Side)

**At design time**, all links that comply with the recommendations above SHOULD be 
documented indicating what each link means including any and all associated methods, 
query parameters, [RFC 6570](https://tools.ietf.org/html/rfc6570) URI templates and body
properties. 

API providers SHOULD consider adding the note below to their documentation to describe how links are used.

> This API uses [RESTful JSON](http://restfuljson.org) by including links in the responses 
> to guide client interactions. Objects in this API MAY include a `url` property for a 
> link to itself and MAY append `_url` to properties for related links.

API providers SHOULD reference the documentation in a response.

**At runtime**, only links a client is allowed to interact with SHOULD be present in API
responses. If multiple methods are associated with a particular link, they MUST all be 
allowed if the link is present.

### API Consumers (Client-Side)

**At build time**, the client SHOULD NOT infer any meaning from nor hard-code any expectation 
of a link's URL structure. A client MAY introspect link URLs to populate documented query
parameters or URI templates.

**At runtime**, the client SHOULD use links in API responses for interacting with resources.
The client SHOULD rely on the presence or absence of links to know what it may or may not
do at runtime. The client SHOULD ignore any resource links or properties it was not designed
to use, allowing the server and client to evolve independently over time.

## Influences

This document is influenced by APIs and tools that have pragmatically added links to their
APIs in a similar way: [GitHub API][github], [Stripe API][stripe], [Medium API][medium], [Basecamp API][basecamp], [Trello API][trello] and [Django REST Framework][django].

## About

This document was authored
by [Stephen Mizell](https://twitter.com/Stephen_Mizell)
and [Mark W. Foster](https://twitter.com/fosrias) in an effort to lower the barrier
to entry for implementing and consuming hypermedia APIs. This document is licensed 
under the MIT license. The requirements here conform
to [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

RESTful JSON can be found on [GitHub](https://github.com/smizell/restfuljson).


[github]: https://developer.github.com/v3/
[stripe]: https://stripe.com/docs/api
[medium]: https://github.com/Medium/medium-api-docs
[basecamp]: https://github.com/basecamp/bc3-api
[trello]: https://developers.trello.com/advanced-reference
[django]: http://www.django-rest-framework.org/tutorial/5-relationships-and-hyperlinked-apis/
[RFC 6906]: https://tools.ietf.org/html/rfc6906
[RFC 7159]: https://tools.ietf.org/html/rfc7159
[RFC 3986]: https://tools.ietf.org/html/rfc3986