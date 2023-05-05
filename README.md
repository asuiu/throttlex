# ThrottleX

Throttle**X** Python library

ThrottleX is a Python library that provides a simple throttling mechanism for rate limiting requests to an API or service. It can be used to limit the number of requests per unit of time, ensuring that your application stays within any usage limits imposed by the API or service provider.

## Installation

To install the library, simply use pip:

```bash
pip install throttlex
```

## Usage

Here's an example of how you can use Throttler in your Python code:

```python
import requests
from throttlex import Throttler
from streamerate.streams import stream

# create a throttler that allows at most 5 requests per 10 seconds
throttler = Throttler(max_req=5, period=10)

# a list of URLs to fetch
urls = ['https://www.google.com/','https://www.amazon.com/','https://www.facebook.com/', 'https://www.apple.com/']

# define a function to fetch a single URL using requests.get, but throttle the request using the throttler
def fetch_url(url):
    # fetch the URL using requests.get
    response = requests.get(url)

    # return the response status code and length
    return f'{url}: status={response.status_code}, length={len(response.content)}'

# fetch the URLs using map() and our fetch_url() function, which is automatically throttled by the throttler
results = map(fetch_url, map(throttler, urls))

# Or in a more elegant way using streamerate
results = stream(urls).map(throttler).map(fetch_url)

# print the results
for result in results:
    print(result)
```
## API

### `Throttler`

The `Throttler` class provides the throttling mechanism.

#### `__init__(self, max_req: int, period: float, time_func: Callable[[], float] = time.time) -> None`

Creates a new `Throttler` instance.

- `max_req`: The maximum number of requests allowed in the specified `period`.
- `period`: The period (in seconds) over which the maximum number of requests is allowed.
- `time_func`: An optional function that returns the current time. By default, this is set to `time.time`.

#### `throttle(self, val: Any) -> Any`

Throttles the specified value.

- `val`: The value to throttle.
- Returns: The throttled value.


## Contributing
If you have any issues or feature requests, please create a new issue on GitHub. Pull requests are also welcome!


## License

MIT License