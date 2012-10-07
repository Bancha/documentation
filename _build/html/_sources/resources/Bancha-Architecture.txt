Bancha Architecture
===================

.. figure:: images/image07.png
   :align: center
   :alt: BanchaArchitecture

   BanchaArchitecture

On the server-side Bancha transforms and marshalls data from the CakePHP
framework and sends it to the front-end (browser). On the frontend the
Bancha singleton unmarshalls the data and use it as model
configurations.

Bancha Front Controller
-----------------------

Every CakePHP application is initialized by the front controller, which
is stored in the index.php file in the webroot directory. The front
controller initializes the dispatcher. Because Bancha needs to
initialize its own dispatcher (see section Bancha Architecture) and
needs to load its own bootstrap file, Bancha is initialized by its own
front controller. Because of CakePHP limitations the user needs to copy
the front controller file bancha.php from the plugin into his webroot
directory.

Exception Handling
------------------

When exceptions occurs, Bancha catches them and sends them back to the
user as a ResponseCollection. If the debug level of CakePHP is greater
than zero Bancha will send no information about the exception, in
development it will send the message and the full trace. We made this
decision, because CakePHP usually renders the thrown exception and we
wanted a possibility to control thrown exceptions. Basically there are
three possibilities to implement an exception handler. By creating an
own exception handler, using AppController::appError(), or by using a
custom renderer with Exception.renderer. For Bancha, the first one was
used, because this gives full access for error handling. The exception
is loaded in the config file bootstrap.php, which overwrites the
standard Exception handler of CakePHP. To sum up, the error is being
catched in the dispatching process, and the response is added as an
exception.

BanchaController
----------------

BanchaController is a CakePHP controller and supplies required methods
for ExtJS. These are a method for initializing ExtJSs Ext.Direct by
sending the API definition and a method for acquiring the model
metadata. The complexity derives from the different ways and
interpration the data and parameters have in CakePHP and ExtJS. The
controller relies on the BanchaBehavior for extracting the data.

How to use it
~~~~~~~~~~~~~

There are 3 ways to use this controller. Just accessing the Controller
with a URL like http://localhost/Bancha.js returns the Bancha API
including controller actions but no ModelMetaData. You can also pass
models as parameters to the index method like this
http://localhost/Bancha.js?models=[Article,User] , this will give us the
meta data for User and the Tag model.

Bancha internally uses this mechanism, so you will normally NOT have to
use any BanchaController methods.

Structure
~~~~~~~~~

So here's an example request when the BanchaController::loadMetaData
method is executed from Ext Direct.

::

        {
            "action":"Bancha",
            "method":"loadMetaData",
            "data":'[["User"]],
            "type":"rpc",
            "tid":1
        }

Followed by the Response:

.. raw:: html

   <pre>
   [{
       "type":"rpc",
       "tid":1,
       "action":"Bancha",
       "method":"loadMetaData",
       "result":{
           "User":{
               "idProperty":"id",
               "fields":[{
                   "name":"id",
                   "type":"int"
               },{
                   "name":"name",
                   "type":"string"
               },{
                   "name":"login",
                   "type":"string"
               },{
                   "name":"created",
                   "type":"date"
               },{
                   "name":"email",
                   "type":"string"
               },{
                   "name":"avatar",
                   "type":"string"
               },{
                   "name":"weight",
                   "type":"float"
               },{
                   "name":"heigth",
                   "type":"int"
               }],
               "validations":[{
                   "type":"numberformat",
                   "name":"id",
                   "precision":0
               },{
                   "type":"presence",
                   "name":"name"
               },{
                   "type":"length",
                   "name":"name",
                   "min":3
               },{
                   "type":"length",
                   "name":"name",
                   "max":64
               },{
                   "type":"length",
                   "name":"login",
                   "min":3
               },{
                   "type":"length",
                   "name":"login",
                   "max":64
               },{
                   "type":"format",
                   "name":"login",
                   "matcher":"banchaAlphanum"
               },{
                   "type":"format",
                   "name":"email",
                   "matcher":"banchaEmail"
               },{
                   "type":"numberformat",
                   "name":"weight",
                   "precision":2
               },{
                   "type":"numberformat",
                   "name":"height",
                   "precision":0
               },{
                   "type":"numberformat",
                   "name":"height",
                   "min":50,
                   "max":300
               },{
                   "type":"file",
                   "name":"avatar"
                   "extension": [
                       "jpg",
                       "jpeg",
                       "gif",
                       "png"
                   ]
               }],
               "associations":[{
                   "type":"hasMany",
                   "model":"Article",
                   "name":"articles"
               }],
               "sorters":[]
           }
       }
   }]
   </pre>

Listing 2.3.2 shows the JSON encoded object that will be returned by
BanchaController::index.

BanchaBehavior
--------------

In CakePHP a behavior class consists of code that is shared between
models. BanchaBehavior provides the function extractBanchaMetaData()
which returns the meta data in the way ExtJS needs it. This is done
using four private functions for better handling of the CakePHP model
structure.

The meta data can be grouped into four categories:

-  associations
-  column types
-  validations
-  sorters

Form Handling
-------------

After discussing many solutions, we figured out the best way is to just
give the cake developer the oppertunity to choose how to handle files.
So for the cake developer ExtJS file uploads will look just like normal
file uploads in cake. On server side, we extract every parameter of the
requests, and if a ”extUpload” parameter is detected, the response is
going to be sent back as a HTML textarea with the JSON encoded string
(Ext.Direct Spec):

::

          <code><html><body><textarea>{BANCHA EXT DIRECT JSON RESPONSE HERE}</textarea></body></html> </code>


