# RESTful JSON
> Because adding links in JSON should be easy

## Specification

For JSON formats conforming to [RFC 4627](https://tools.ietf.org/html/rfc4627),
follow the following guidelines.

1. JSON objects MAY include a `url` property for a link to itself
2. JSON objects MAY append `_url` to properties for related links
3. JSON objects MAY append `_urls` to properties for lists of related links

All URLs SHOULD conform to [RFC 3986](https://tools.ietf.org/html/rfc3986).

### Example

The JSON below shows a representation for an article.

``` json
{
    "url": "http://example.com/article/17",
    "title": "Article Title",
    "body": "The body of the article."
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

This example shows how `url` is used for an object for the article and
categories, how it's used for a list of URLs with `author_urls`, and how it's used
for a single URL with `docs_url`.

As an API designer, you SHOULD document what these URLs mean.

## Usage

If you are using `application/json`, the easiest way for you to dive in is to
start adding links to your API responses. It may be helpful to add a note in
your API documentation.

> This API uses [RESTful JSON](https://restfuljson.org) by including URLs in the
> responses. Objects in this API MAY include a `url` property for a link to
> itself, MAY append `_url` to properties for related links, and MAY append
> `_urls` to properties for lists of related links.

Custom mime types and JSON schemas can also be a benefit in defining how your
customers interact with the URLs in your responses.

## Why Add Links?

While adding URLs in JSON may increase the payload size, and if not optimized
correctly can result in clients making many requests to fulfill their goals,
there are many benefits that you get when adding them to your API.

### Browseable API

Adding URLs can make your API browseable, self-describing, and discoverable.
People encountering your API will be able to follow links to get a deeper
understanding of your design and a clearer picture of how your resources are
related.

HTTP clients like Postman will even make the links in your JSON clickable, so
people using this client can explore your API easily.

### No Hardcoded URLs

Clients can rely on URLs in the responses rather than hardcoding those URLs.
This can allow you to move resources around in your API without breaking
clients, and can even let you move resources to different APIs.

### Describe What Clients Can (and Can't) Do

A URL lets a client know it has the permissions to follow that link and do
whatever that link may allow. The absence of a URL also tells a client that it
does not have permission to access and interact with that resource. This allows
a API to toggle functionality for clients as the client interacts with the API.

## Influences

This document is influenced by APIs that have pragmatically added URLs to their
APIs in a similar way.

- [GitHub API](https://developer.github.com/v3/)
- [Strip API](https://stripe.com/docs/api)

The [Django REST Framework](http://www.django-rest-framework.org) also includes
URLs when using their hyperlinked serializers.

## Helpful Ideas

The specification section defines all of the rules around using RESTful JSON.
However, there are some ideas and recommendations that may help both the
servers and clients along the way.

### Including or Linking Data

Clients SHOULD NOT expect for a property to be in a response. Instead clients
SHOULD check the property is there before moving on. Additionally, a client
should look for included data first before requesting a URL.

For example, if a client receives this response below, and it's look for a
`book`, it should use the included `book` property.

``` json
{
    "book": {
        "title": "The Hobbit",
        "author": "J. R. R. Tolkien"
    }
}
```

However, if the client does not find the `book` property, it SHOULD look for the
`book_url` next.

``` json
{
    "book_url": "http://example.com/books/413"
}
```

### Conditional Actions for a Client 

To go beyond linking data in an API, an API can also provide actions that a
client might take in a given scenario. For example, if a client receives a todo
resource like the one below, it knows that the todo is incomplete and that it
has permission to mark it complete.

``` json
{
    "title": "Get milk",
    "mark_complete_url": "http://example.com/todos/4/mark-complete"
}
```

The documentation would define what method to use and what properties to send
when marking a todo complete. Once complete, a client might see a response like
this.


``` json
{
    "title": "Get milk",
    "mark_incomplete_url": "http://example.com/todos/4/mark-incomplete"
}
```

This tells the client that it can mark the todo as incomplete again based on the
instructions provided in the documentation.

## About

This document was authored by two recovering
RESTafarians, [Stephen Mizell](https://twitter.com/Stephen_Mizell)
and [Mark Foster](https://twitter.com/fosrias), in an effort to lower the
barrier to entry for building hypermedia APIs. This document is licensed under
the MIT license. The requirements here conform
to [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).
