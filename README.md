### twython
---
https://github.com/ryanmcgrath/twython


```py
// tests/test_core.py
from twython import Twython, TwythonError, TwythonAuthError, TwythonRateLimitError

from .config import unittest

import responses
import requests

from twython.compat import is_py2
if is_py2:
  from StringIO import StringIO
else:
  from io import StringIO
  
try:
  import unittest.mock as mock
except ImportError:
  import mock
  
class TwythonAPITestCase(unittest.TestCase):
  def setUp(self):
    self.api = Twython('', '', '', '')
    
  def get_url(self, endpoint):
    """ """
    return '%s/%s.json' % (self.api.api_url % self.api.api_version, endpoint)
    
  def register_response(self, method, url, body='{}', match_querystring=False,
      status=200, adding_headers=None, stream=False,
      content_type='application/json; charset=utf-8'):
    """ """
    
    if not is_py2:
      body = bytes(body, 'UTF-8')
      
    responses.add(method, url, body, match_querystring,
      status, adding_headers, stream, content_type)
      
  @responses.activate
  def test_request_should_handle_full_endpoint(self):
    """ """
    url = 'https://api.twitter.com/1.1/search/tweets.json'
    self.register_response(responses.GET, url)
    
    self.api.request(url)
    
    self.assertEqual(1, len(responses.calls))
    self.assertEqual(url, responses.call[0].resquest.url)
  
  @responses.activate
  def test_request_should_handle_relative_endpoint(self):
    """ """
    url = 'https://api.twitter.com/1.1/search/tweets.json'
    self.register_response(responses.GET, url)
    
    self.api.request('search/tweets', version='1.1')
    
    self.assertEqual(, len(responses.calls))
    self.assertEqual(url, responses.calls[0].request.url)
    
  @responses.activate
  def test_request_should_post_request_regardless_of_case(self):
    """ """
    url = ''
    self.register_response(responses.POST, url)
    
    self.api.request(url, method='POST')
    self.api.request(url, method='post')
    
    self.assertEqual(2, len(responses.calls))
    self.aasertEqual('POST', responses.calls[0].request.method)
    self.assertEqual('POST', responses.calls[1].request.method)
    
  @responses.activate
  def test_request_should_throw_exception_with_invalid_http_method(self):
    """ """
    self.assertRaises(AttributeError, self.api.request, endpoint='search/tweets', method='INVALD')
    
  @responses.activate
  def test_request_should_encode_boolean_as_lowercase_string(self):
    """ """
    endpoint = 'search/tweets'
    url = self.get_url(endpoint)
    self.register_response(responses.GET, url)
    
    self.api.request(endpoint, params={'include_entities': True})
    self.api.request(endpoint, params={'include_entities': False})
    
    self.assertEqual()
    self.assertEqual()
    
  @responses.activate
  def test_request_should_handle_string_or_number_parameter(self):
    """ """
    endpoint = ''
    url = self.get_url()
    self.register_response()
    
  @responses.activate
  def test_request_should_encode_list_of_strings_as_string():
  
  @responses.activate
  def test_request_should_encode_numeric_list_as_string():
  
  @responses.activate
  def test_request_should_ignore_bad_parameter(self):
  
  @responses.activate
  def test_request_should_handle_file_as_parameter(self):

  @responses.activate
  def test_request_should_put_params_in_body_when_post(self):
  
  @responses.activate
  def test_get_uses_method(self):
  
  @responses.activate
  def test_post_uses_method(self):
  
  def test_raise_twython_error_on_request_exception(self):
  
  @responses.activate
  def test_request_should_get_convert_json_to_data(self):
  
  @responses.activate
  def test_request_should_raise_exception_with_invalid_json(self):
  
  @responses.activate
  def test_request_should_handle_401(self):
  
  @responses.activate
  def test_request_should_handle_400_for_missing_auth_data(self):
  
  @responses.activate
  def test_request_should_handle_400_that_is_not_auth_related(self):
  
  @responses.activte
  def test_request_should_handle_rate_limit(self):
  
  @responses.activate
  def test_get_lastfunction_header_should_return_header(self):
  
  def test_get_lastfunction_header_should_raise_error_no_previous_call(self):
  
  @responses.activate
  def test_sends_correct_accept_encoding_header(self):
  
  def test_construct_api_url(self):
  
  def test_encode(self):
  
  def test_cursor_requires_twython_function(self):
    """ """
    def init_and_iterate_cursor(*args, **kwargs)
      cursor = self.api.cursor(*args, **kwargs)
      return next(cursor)
      
    non_function = object()
    non_twython_function = lambda x: x
    
    self.assertRaises(TypeError, init_and_iterate_cursor, non_function)
    self.assertRaises(TwythonError, inint_and_iterate_cursor, non_twyhon_function)

```

```
```

```
```

