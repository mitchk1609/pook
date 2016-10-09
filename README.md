# pock [![Build Status](https://travis-ci.org/h2non/pock.svg?branch=master)](https://travis-ci.org/h2non/pock) [![PyPI](https://img.shields.io/pypi/v/pock.svg?maxAge=2592000?style=flat-square)](https://pypi.python.org/pypi/pock) [![API](https://img.shields.io/badge/api-docs-green.svg)](https://h2non.github.io/pock) [![Documentation Status](https://readthedocs.org/projects/pock/badge/?version=latest)](http://pock.readthedocs.io/en/latest/?badge=latest)

Simply and expressive utility library for mocking and expectations for HTTP traffic in [Python](http://python.org).

Small and dependency-free package to infer file type and MIME type checking the [magic numbers](https://en.wikipedia.org/wiki/Magic_number_(programming)#Magic_numbers_in_files) signature of a file or buffer.

pock is heavily inspired by [gock](https://github.com/h2non/gock).

**Note**: this is a work in progress.

## Features

- Simple, expressive and fluent API
- Full-featured, idiomatic HTTP expectations.
- JSON schema based body matching.
- Extensible: write your own HTTP expections.
- HTTP client agnostic (works with most popular HTTP libraries).
- Pluggable hackable API.
- Compatible with Python 2 and 3.

## Supported HTTP clients

- [x] urllib3
- [x] requests
- [ ] urllib
- [ ] aiohttp
- [ ] pycurl

## Installation

Using `pip` package manager:
```bash
pip install pock
```

Or install the latest sources from Github::
```bash
pip install -e git+git://github.com/h2non/pock.git#egg=pock
```

## API

See [annotated API reference](https://h2non.github.io/pock).

## Examples

#### Basic mocking

```python
import pock
import requests

@pock.activate
def test_my_api():
    mock = pock.get('http://twitter.com/api/1/foobar',
                    type='application/json',
                    json={'error': 'not found'})
    mock.reply(404, json={'error': 'foo'})

    resp = requests.get('http://twitter.com/api/1/foobar')
    assert resp.json() == {"error": "not found"}
    assert len(mock.calls) == 1
    assert mock.calls[0].request.url == 'http://twitter.com/api/1/foobar'
    assert mock.calls[0].response.text == '{"error": "not found"}'
```

#### Using the fluent API

```python
import pock
import requests

@pock.activate
def test_my_api():
    mock = pock.get('http://twitter.com/api/1/foobar'). \
           status(404). \
           json({'error': 'not found'})

    resp = requests.get('http://twitter.com/api/1/foobar')
    assert resp.json() == {"error": "not found"}
    assert len(mock.calls) == 1
    assert mock.calls[0].request.url == 'http://twitter.com/api/1/foobar'
    assert mock.calls[0].response.text == '{"error": "not found"}'
```


## License

MIT - Tomas Aparicio
