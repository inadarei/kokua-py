# Kōkua - Hypermedia Representor. Python implementation

> Kōkua (Hawaiian) - help, aid, assistance

  [![Pypi][pypi-image]][pypi-url]
  [![Build][travis-image]][travis-url]
  [![Update][pyup-image]][pyup-url]

<!-- Pronounciation: http://hawaiian-words.com/ -->

Kōkua (Python) is an implementation of the
[Representor](https://github.com/the-hypermedia-project/charter#representor-pattern)
pattern. It is a Python version of the [reference
implementation](https://github.com/inadarei/kokua) in Node. It allows developers
to represent hypermedia messages in a flexible media type, purpose-designed for
the task: [Hyper](http://hyperjson.io) and automatically outputs messages in a
variety of popular Hypermedia formats such as:

1. [HAL][hal-spec] (application/hal+json)
2. [Siren][siren-spec] (application/vnd.siren+json)
3. [Collection+JSON][coljson-spec] (application/vnd.collection+json)
4. [UBER][uber-spec] (application/vnd.uber+json)
5. [JSON API][jsonapi-spec] (application/vnd.api+json)
5. etc.

## Usage

### Convert a Hyper document to other formats

```Python
  from kokua import representor
  halDoc = representor.render(hyperDoc, representor.mt('hal'))
```

where the first argument to a `representor.render()` call is a JSON document
formatted as a Hyper document, and the second argument is the name of a
supported media-type that we want the message to be translated to.

### Convert a document in another format to Hyper

```Python
  from kokua import representor
  uberDoc = representor.convert(halDoc, representor.mt('hal'))
```

where the first argument to a `representor.convert()` call is a JSON document
formatted in a media type, supported by Kokua, and the second argument is the
name of a supported media-type that we want the message to be translated from.

Please see the official specification for
[Hyper](https://github.com/inadarei/hyper) media type, for more details about
the format.

### Advanced Example

```Python
from kokua import representor

hyper = {
  "h:head": {"curies": {"ea": "http://example.com/docs/rels/"}},
  "h:ref": {"self": "/orders", "next": "/orders?page=2"},
  "currentlyProcessing": 14, "shippedToday": 20,
  "ea:order": [
    {
      "h:ref": {
        "self": "/orders/123",
        "ea:basket": "/baskets/98712",
        "ea:customer": "/customers/7809"
      },
      "total": 30, "currency": "USD", "status": "shipped"
    },
    {
      "h:ref": {
        "self": "/orders/123",
        "ea:basket": "/baskets/98712",
        "ea:customer": "/customers/124234"
      },
      "total": 123, "currency": "USD", "status": "pending"
    }
  ]
}

halDoc = representor.render(hyperDoc, representor.mt('hal'))
halDoc = representor.render(hyperDoc, representor.mt('siren'))
```

## Implementation Status

1. Hyper to [HAL][hal-spec]: in progress
    - Reverse: 0%
1. Hyper to [Siren][siren-spec]: 0%
    - Reverse: 0%
1. Hyper to [UBER][uber-spec]: 0%
    - Reverse: 0%
1. Hyper to [Collection+JSON][coljson-spec]: 0%
    - Reverse: 0%
1. Hyper to [JSONAPI][jsonapi-spec]: 0%
    - Reverse: 0%

### Quick-n-dirty benchmark


### Plugin Development

If you are interested in developing a new plugin to implement translation to a
hypermedia format that is not yet implemented, please refer to
[README-PLUGINDEV](README-PLUGINDEV.md)

[pypi-image]: https://img.shields.io/pypi/v/kokua.svg
[pypi-url]: https://pypi.python.org/pypi/kokua
[travis-image]: https://img.shields.io/travis/inadarei/kokua-py.svg
[travis-url]: https://travis-ci.org/inadarei/kokua-py
[pyup-image]: https://pyup.io/repos/github/inadarei/kokua-py/shield.svg
[pyup-url]: https://pyup.io/repos/github/inadarei/kokua-py/
[hal-spec]: http://stateless.co/hal_specification.html
[siren-spec]: https://github.com/kevinswiber/siren
[uber-spec]: http://uberhypermedia.org
[coljson-spec]: http://amundsen.com/media-types/collection/
[jsonapi-spec]: http://jsonapi.org/format/