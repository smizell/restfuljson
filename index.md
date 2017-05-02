# RESTful JSON
> Because adding links in JSON should be easy

RESTful JSON is a minimal and pragmatic specification for using URLs to build
expressive and evolvable APIs.

## Specification

For JSON formats conforming to [RFC 4627](https://tools.ietf.org/html/rfc4627),
apply the following guidelines:

1. JSON objects MAY include a `url` property to indicate a link to itself
2. JSON objects MAY append `_url` to properties to indicate related links
3. JSON objects MAY append `_urls` to properties to indicate lists of related
   links

All URLs SHOULD conform to [RFC 3986](https://tools.ietf.org/html/rfc3986). API
providers MAY use camel case rather than snake case where applicable.

### Example

The JSON below shows a representation for an article.

``` json
{
    "url": "http://example.com/article/17",
    "title": "Article Title",
    "body": "The body of the article.",
    "author_urls": [
        "http://example.com/authors/42",
        "http://example.com/authors/75"
    ],
    "categories": [
        {
            "url": "http://example.com/categories/29",
            "name": "Category A"
        },
        {
            "url": "http://example.com/categories/33",
            "name": "Category B"
        }
    ],
    "docs_url": "http://example.com/docs/article"
}
```

This example shows how the `url` is used for an object for the article and
categories, how it's used for a list of URLs with `author_urls`, and how it's
used for a single URL with `docs_url` (which is an example of a way to point to
documentation from within a response).

The API provider SHOULD document what these URLs mean, any actions related to
them, and reference the documentation in the response.

## Usage and Guidelines

### API Providers (Server-Side)

**At design time**, all URLs that comply with the specification above SHOULD be
documented. API providers SHOULD consider adding the note below to their
documentation to describe how URLs are used.

> This API uses [RESTful JSON](https://restfuljson.org) by including URLs in the
> responses. Objects in this API MAY include a `url` property for a link to
> itself, MAY append `_url` to properties for related links, and MAY append
> `_urls` to properties for lists of related links.

**At runtime**, only URLs the client is allowed to invoke or interact with SHOULD be
included in the API responses. 

### API Consumers (Client-Side)

**At design time**, the client SHOULD NOT hardcode any URLs or URL patterns and
SHOULD NOT ascribe any meaning to the contents of a URL.

**At runtime**, the client SHOULD use the URLs in API responses for retrieving
resources. The client SHOULD rely on the URLs to know what it can or cannot do
at runtime, allowing the server to retain all logic for what a client may or may
not do in a given context. The client SHOULD ignore any URLs it was not designed
to use, allowing the server and client to evolve over time.

## Influences

This document is influenced by APIs that have pragmatically added URLs to their
APIs in a similar way.

- [GitHub API](https://developer.github.com/v3/)
- [Strip API](https://stripe.com/docs/api)
- [Medium API](https://github.com/Medium/medium-api-docs)
- [Basecamp API](https://github.com/basecamp/bc3-api)
- [Trello API](https://developers.trello.com/advanced-reference)

The [Django REST Framework](http://www.django-rest-framework.org) also includes
URLs when using their hyperlinked serializers.

## About

This document was authored
by [Stephen Mizell](https://twitter.com/Stephen_Mizell)
and [Mark Foster](https://twitter.com/fosrias) in an effort to lower the barrier
to entry for building hypermedia APIs. This document is licensed under the MIT
license. The requirements here conform
to [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).
