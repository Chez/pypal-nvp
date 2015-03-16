An implementation of [PayPal](https://www.paypalobjects.com/en_US/ebook/PP_NVPAPI_DeveloperGuide/toc.html) Express Checkout API written in Python; with emphasis on ease-of-use and simplicity.

Design and implementation based on [PayPal NVP Java Library](http://paypal-nvp.sourceforge.net/index.htm).

```
# import the required modules
import gk.paypal.core
import gk.paypal.requests
import gk.paypal.fields
```

```
# set user - these are your credentials from paypal
user = gk.paypal.core.BaseProfile(
   username=self.settings['paypal_login'], 
   password=self.settings['paypal_password'] )
   user.set_signature( self.settings['paypal_signature'] )
```

```
# create items (items from a shopping basket)
item1 = gk.paypal.fields.PaymentItem()
item1.set_name( 'Tazza da caffe' )
item1.set_description('Splendida tazza da caffe bianca')
item1.set_amount( '8.00' );

item2 = gk.paypal.fields.PaymentItem()
item2.set_name( 'Biscottiera' )
item2.set_description('Pregiata biscottiera in porcellana Ming')
item2.set_amount( '24.00' );
```

```
# create payment payment from the items
payment = gk.paypal.fields.Payment( items=[item1, item2] )
payment.set_currency( 'EUR' )
```

```
# create set express checkout - the first paypal request
set_ec = gk.paypal.requests.SetExpressCheckout( 
    payment, self.settings['paypal_success_url'], 
    self.settings['paypal_cancel_url'] )
```

```
# create new instance of paypal nvp, send request e set response
paypal = gk.paypal.core.PayPal( user )
paypal.set_response( set_ec )	

response = set_ec.get_nvp_response()
```