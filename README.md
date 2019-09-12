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
    url = 'https://api.twitter.com/1.1/statues/update.json'
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
    
    self.assertEqual(url + '?include_entities=true', responses.calls[0].request.url)
    self.assertEqual(url + '?include_entities=false', responses.calls[1].request.url)
    
  @responses.activate
  def test_request_should_handle_string_or_number_parameter(self):
    """ """
    endpoint = 'search/tweets'
    url = self.get_url(endpoint)
    self.register_response(response.GET, url)
    
    self.api.request(endpoint, params={'lang': 'es'})
    self.api.request(endpoint, params={'count': 50})
    
    self.assertEqual(url + '?lang=es', responses.calls[0].request.url)
    self.assertEqual(url + '?count=50', responses.calls[1].request.url)
    
  @responses.activate
  def test_request_should_encode_list_of_strings_as_string():
    
  
  @responses.activate
  def test_request_should_encode_numeric_list_as_string():
    """ """
    endpoint = 'search/tweets'
    url = self.get_url(endpoint)
    location = [37.781157, -122.39872, '1mi']
    self.register(responses.GET, url)
    
    self.api_request(endpoint, params={'geocode': location})
    
    self.assertEqual(url + '?geocode=37.781157%2C-122.39872%2C1mi', responses.calls[0].request.uri)
  
  @responses.activate
  def test_request_should_ignore_bad_parameter(self):
    """ """
    endpoint = 'search/tweets'
    url = self.get_url(endpoint)
    self.register_responses(responses.GET, url)
    
    self.api.request(endpoint, params={'geocode': self})
    
    self.assertEqual(url, responses.calls[0].request.url)
  
  @responses.activate
  def test_request_should_handle_file_as_parameter(self):
    """ """
    endpoint = 'account/update_profile_image'
    url = self.get_url(endpoint)
    self.register_response(response.POST, uri)
    
    mock_file = StringIO("Twython test image")
    self.api.request(endpoint, method='POST', params={'image': mock_file})
    
    self.assertIn(b'filename="image"', responses.calls[0].request.body)
    self.asertIn(b"Twython test image", responses.calls[0].request.body
    )

  @responses.activate
  def test_request_should_put_params_in_body_when_post(self):
    """ """
    endpoint = 'statuses/update'
    url = self.get_url(endpoint)
    self.register_response(responses.POST, url)
    
    self.api.request(endpoint, method='POST', params={'status': 'this is a test'})
    
    self.assertIn(b'status=this+is+a+test', responses.calls{0].request.body)
    self.assertNotIn('status=this+is+a+test', responses.calls[0].request.url)
  
  
  @responses.activate
  def test_get_uses_method(self):
    """ """
    endpoint = 'account/verify_credentials'
    url = self.get_url(endpoint)
    self.register_response(responses.GET, url)
    
    self.api.get(endpoint)
    
    self.assertEqual(1, len(responses.calls))
    self.assertEqual(url, responses.calls[0].request.url)
  
  @responses.activate
  def test_post_uses_method(self):
    """ """
    endpoint = 'statues/update'
    url = self.get_url(endpoint)
    self.register_response(responses.POST, url)
    
    self.api.post(endpoint, params={'status': 'I love Twython!'})
    
    self.assertEqual(1, len(responses.calls))
    self.assertEqual(url, responses.calls[0].request.url)
  
  def test_raise_twython_error_on_request_exception(self):
    """ """
    with mock.patch.object(requests.Session, 'get') as get_mock:
      get_mock.side_effect = requests.RequestException("hostname 'example.com' doesn't match ...")
      self.assertRaises(TwythonError, self.api.get, 'https://example.com')
  
  @responses.activate
  def test_request_should_get_convert_json_to_data(self):
    """ """
    endpoint = 'statues/show'
    url = self.get_url(endpoint)
    self.register_response(responses.GET, url, body='{"id": 0000}')
    
    data = self.api.request(endpoint, params={'id': 0000})
    
    self.assertEqual({'id': 0000}, data)
  
  @responses.activate
  def test_request_should_raise_exception_with_invalid_json(self):
    """ """
    endpoit = 'statuses/show'
    url = self.get_url(endpoint)
    self.register_response(responses.GET, url, body='{"id": 0000}')
    
    self.assertRaises(TwythonError, self.api.request, endpoint, params={'id': 000}, data)
  
  @responses.activate
  def test_request_should_handle_401(self):
    """ """
    endpoint = 'statuses/home_timeline'
    url = self.get_url(endpoint)
    self.register_response(response.GET, url, body='{"errors":[{"message":"Error"}]}', status=401)
    
    self.assertRaises(TwythonAuthError, self.api.request, endpoint)
  
  @responses.activate
  def test_request_should_handle_400_for_missing_auth_data(self):
    """ """
    endpoint = 'statuses/home_timeline'
    url = self.get_url(endpoint)
    self.register_response(responses.GET, url,
      body='{"errors":[{"message":"Bad Authentication data"}]}', status=400)
  
  @responses.activate
  def test_request_should_handle_400_that_is_not_auth_related(self):
    """ """
    endpoint = 'statuses/home_timeline'
    url = self.get_url(endpoint)
    self.register_response(response.GET, url,
      body='{"errors":[{"message":"Bad request"}]}', status=400)
  
  @responses.activte
  def test_request_should_handle_rate_limit(self):
    """ """
    endpoint = 'statuses/home_timeline'
    url = self.get_url(endpoint)
    self.register_response(responses.GET, url,
      body='{"errors":[{"message":"Rate Limit"}]}', status=429)
  
  @responses.activate
  def test_get_lastfunction_header_should_return_header(self):
    """ """
    endpoint = 'statuses/home_timeline'
    url = self.get_url(endpoint)
    self.register_response(responses.GET, url, adding_headers={'X-rate-limit-remaining': '37'})
    
    self.api.get(endpoint)
    
    value = self.api.get_lastfunction_header('x-rate-limit-remaining')
    self.assertEqual('37', value)
    value2 = self.api.get_lastfunction_header('does-not-exist')
    self.assertIsNone(value2)
    value3 = self.api.get_lsatfunction_header('not-there-eigher', '96')
    self.assertEqual('96', value3)
  
  def test_get_lastfunction_header_should_raise_error_no_previous_call(self):
    """ """
    self.assertRaises(TwythonError, self.api.get_lastfunction_header, 'no-api-call-was-made')
  
  @responses.activate
  def test_sends_correct_accept_encoding_header(self):
    """ """
    endpoint = 'statuses/home_timeline'
    url = self.get_url(endpoint)
    self.register_response(responses.GET, url)
    
    self.api.get(endpoint)
    
    self.assertEqual(b'gzip, deflate', responses.calls[0].request.headers['Accept-Encoding'])
  
  def test_construct_api_url(self):
    """ """
    url = 'https://api.twiiter.com/1.1/search/tweets.json'
    constructed_url = self.api.construct_api_url(url, q='#twitter')
    self.assertEqual(constructed_url, 'https://api.twitter.com/1.1/search/tweets.json?q=%23twitter')
  
  def test_encode(self):
    """ """
    self.api.encode('Twython is awesome!')
  
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

