#summary design/how to use the library

# Introduction #

The library is divided into several modules:

  * **core** base classes for API authentication and network transport;
  * **util** helper class (validation, formatting);
  * **requests** all the implemented NVP methods;
  * **fields** contains all the fields which are/can be used in a request.

To add new request, extend the `core.Request` abstract class.

To create a new field, extend the `fields.RequestFields` abstract class.

# How to install the library #

Download the latest tar.gz (something like pypal-nvp-0.5.0.tar.gz) from the [Downloads](http://code.google.com/p/pypal-nvp/downloads/list) on the front page.

On unix systems (linux, BSD, Mac OS X, cygwin, etc.): extract it with the command
```
tar xzf pypal-nvp-0.5.0.tar.gz
```
go in the extracted folder and type:
```
python setup.py install
```

# How to use the library #

To use the library, you need to authenticate with your API login and signature so first create a user.<br>
Then you can use BaseProfile class, or if you need more complexity, you can extends <code>core.Profile</code> class.<br>
<br>
<pre><code># set user - these are your credentials from paypal<br>
user = gk.paypal.core.BaseProfile(<br>
   username=self.settings['paypal_login'], <br>
   password=self.settings['paypal_password'] )<br>
   user.set_signature( self.settings['paypal_signature'] )<br>
</code></pre>

<pre><code># then create the request<br>
balance = gk.paypal.requests.GetBalance()<br>
balance.set_all_currencies( False )<br>
</code></pre>

<pre><code># instantiate PayPal class, send the request and set response.<br>
paypal = gk.paypal.core.PayPal( user )<br>
paypal.set_response( balance )<br>
</code></pre>

<a href='https://www.paypalobjects.com/en_US/ebook/PP_NVPAPI_DeveloperGuide/toc.html'>PayPal</a> class does not have send method, but it has set_response method, which sets response on supplied Request object. This way, the request and response are together in the appropriate object, in this case inside balance object.<br>
<br>
Every Request instance has:<br>
<ul><li>get_nvp_request method, which returns a <code>dict</code> - name value pair request to be sent to <a href='https://www.paypalobjects.com/en_US/ebook/PP_NVPAPI_DeveloperGuide/toc.html'>PayPal</a>;<br>
</li><li>set_nvp_response method, which is used by <a href='https://www.paypalobjects.com/en_US/ebook/PP_NVPAPI_DeveloperGuide/toc.html'>PayPal</a> instance to set response from <a href='https://www.paypalobjects.com/en_US/ebook/PP_NVPAPI_DeveloperGuide/toc.html'>PayPal</a>;<br>
</li><li>get_nvp_response method, which returns <code>None</code> if request has not been sent or no reponse has been returned. This way every request knows its request parameters and response paramenters.</li></ul>

<pre><code>response = balance.get_nvp_response()  <br>
print response<br>
</code></pre>