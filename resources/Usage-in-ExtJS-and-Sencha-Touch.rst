Usage in ExtJS and Sencha Touch
===============================

Bancha works the same for ExtJS and Sencha Touch.

Using models
------------

Bancha provides a simple interface to create models. These models get
all fields, validations and associations and a fully configured proxy
from the server.

You just have to use *Bancha.onModelReady('User', function() {...})* or
*Bancha.onModelReady(['User','Article', function() {...})* and Bancha
does the rest!

Using exposed Controller Methods
--------------------------------

Every controller method, which is `marked as
remotable <./How-to-Expose-Controller-Methods.html>`_
can be accessed like
*Bancha.getStub('MyController').myMethod(param1,param2,callback);*

For more see `How to Expose Controller
Methods <./How-to-Expose-Controller-Methods.html>`_.

See also
--------

-  `Bancha with Sencha Touch
   Screencast <http://vimeo.com/bancha/bancha-for-sencha-touch-2>`_
-  `Bancha JS API
   Documentation <http://api.banchaproject.org/js/index.html#/api/Bancha>`_
-  `Bancha Samples <http://samples.banchaproject.org/>`_

