Dispatching in Bancha
=====================

The biggest challenge in designing the architecture of Bancha was the
dispatching process. Before we explain the architecture, we need to take
a look at the request and response handling in CakePHP and ExtJS.

Requests in ExtJS
-----------------

One of the features of Ext Direct is that it can group multiple small
Ajax requests into a single batch request. For example the clients needs
to send two requests A and B within a very short time period to the
server. Ext groups Request A and B into a single request and sends them
to the server. The client expects that the server responds with one HTTP
response, which includes Response A and Response B. The same thing
happens if Ext does not receive a response to Request A. It will resend
Request A together with Request B.

Controllers and actions in ExtJS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ExtJS has different naming conventions for controllers and actions than
CakePHP (and most other server side frameworks). Controllers are named
action and actions are named method.

Also ExtJS does not use the default names for CRUD, but the following:

.. raw:: html

   <table>
    <tr><td> 

Create

.. raw:: html

   </td><td> 

create

.. raw:: html

   </td></tr>
    <tr><td> 

Retrieve

.. raw:: html

   </td><td> 

read

.. raw:: html

   </td></tr>
    <tr><td> 

Update

.. raw:: html

   </td><td> 

update

.. raw:: html

   </td></tr>
    <tr><td> 

Delete

.. raw:: html

   </td><td> 

destroy

.. raw:: html

   </td></tr>
   </table>

Now we want to take a closer look at the format of CRUD request and
responses. The basic structure of an Ext Direct is request is shown
below. Some of these parameters are required, while some are not. The
parameters action and method are required and define which action is
executed on the server. A transaction ID must be passed through the
parameter tid. Notable is also the parameter \_\_\_\_bcid\_\_ which is
optional and can hold the client ID. If this parameter is given Bancha
ensures consistency for this model.

.. raw:: html

   <pre>
   [{ 
       "action":"Person", 
       "method":"update", 
       "result":{data:{…}}
       "tid":6, 
       "type":"rpc" 
   }]
   </pre>

Example of an request:

.. raw:: html

   <pre>
   [{ 
       "action":"Person", 
       "method":"update", 
       "data":[{
           "data":{ 
               "__bcid":"4e2f23e5c6df3", 
               "id":"52", 
               "firstName":"Callie", 
               "lastName":"Winters", 
               "street":"[here are bad words]", 
               "city":"Mayagnez" 
           }
       }], 
       "tid":6, 
       "type":"rpc" 
   }]
   </pre>

And a possible response:

.. raw:: html

   <pre>
   [{ 
       "action":"Person", 
       "method":"update", 
       "result":[{
           "success":true,
           "data":{ 
               "__bcid":"4e2f23e5c6df3", 
               "id":"52", 
               "firstName":"Callie", 
               "lastName":"Winters", 
               "street":"", // server can modify client data like here 
               "city":"Mayagnez" 
           }
       }], 
       "tid":6, 
       "type":"rpc" 
   }]
   </pre>

While the basic structure is the same for every CRUD action, the
following four sub chapters describe the structure of the data element
in the request and the response specific for every CRUD action. ###
5.3.1.2 Create Create request sent by ExtJS:

.. raw:: html

   <pre>
   {
       "action":"User",
       "method":"create",
       "data":[{
           "data":{
               "id":0,
               "name":"asdf",
               "login":"asdf",
               "created":null,
               "email":"adsf@asdf.com",
               "avatar":"",
               "weight":80,
               "heigth":120
           }
       }],
       "type":"rpc",
       "tid":6
   }
   </pre>

The response should contain all data values including the ID generated
by the server. It is also possible to change the value of a field and
ExtJS will adopt the js model data and interface automatically.

Retrieve/Read
~~~~~~~~~~~~~

There is only one type of read action for retrieving a list of entities
and for retrieving a single entity. When a single entity should be
returned, there must be a parameter called id.

Read request for a single entity in ExtJS:

.. raw:: html

   <pre>
   [{ 
       "action":"Person", 
       "method":"read", 
       "result":{
           "data":{
               "id":5
           }
       }
       "tid":6, 
       "type":"rpc" 
   }]
   </pre>

Please note that the ID parameter must be also called id in order to
allow Bancha to pass the request to CakePHP.

Read request for a list of entities in ExtJS.

.. raw:: html

   <pre>
   [{
       "action":"User",
       "method":"read",
       "result":{
           "data":{
               "page":1,
               "start":0,
               "limit":25
           }
       }],
       "type":"rpc",
       "tid":2
   }]
   </pre>

The *page* parameter represents the page number (default: 1).
The *start* parameter is the index (offset) of the first record in the
result and is used for paging (default: 0).
The *limit* parameter is the number of records to read. The *sort*
parameter is an array, where every element is an object with two
elements. property highlights the name of a field and direction the
order direction. It can be ASC or DESC. All parameter are optional.

In both cases, a single entity or a list of entity is returned, the
response looks like this: Response to a read request expected by ExtJS.

.. raw:: html

   <pre>
   [{
       "type":"rpc",
       "tid":2,
       "action":"User",
       "method":"read",
       "result":[{
           "success":true,
           "total": 100, // if paging is used this is the total count of records
           "data": [{
               // record 1
               "id":"2",
               "name":"fdas",
               "login":"fdas",
               "created":"2011-09-05 12:50:44",
               "email":"fdas@fdas.com",
               "avatar":"",
               "weight":"80",
               "heigth":null
           }, {
               // record 2
               ...
           }]
       }]
   }]
   </pre>

Update
~~~~~~

Update request sent by ExtJS.

.. raw:: html

   <pre>
   {
       "action":"User",
       "method":"update",
       "data":[{
           "data":{
               "id":2,
               "name":"newName"
           }
       }],
       "type":"rpc",
       "tid":4
   }
   </pre>

The request contains the ID and all changed values. Values that do not
change are not required to be sent. The response should contain all
values including the ID.

.. raw:: html

   </pre>

Delete
~~~~~~

Delete request sent by ExtJS.

.. raw:: html

   <pre>
   {
       "action":"User",
       "method":"destroy",
       "data":[{
           "data":{
               "id":2
           }
       }],
       "type":"rpc",
       "tid":5
   }
   </pre>

For a delete request ExtJS only sends the ID of the entity to delete. It
expects a response with only the *success* property inside the results
element.

Responses in ExtJS
------------------

Bancha does not only need to take care of ExtJS requests and transform
them into CakePHP compatible requests it does also need to transform the
responses. ExtJS excepts the following key-value pairs in each response.

-  type - rpc
-  tid - The transaction id that has just been processed
-  action - The class/action that has just been processed
-  method - The method that has just been processed
-  result - The result of the method call

Request Handling in CakePHP
---------------------------

The CakePHP framework was designed to handle normal HTTP requests. Thus
there is exactly one dispatcher, which takes exactly one HTTP request.
Each request triggers one action on exactly one controller.

.. figure:: images/image00.png
   :align: center
   :alt: Figure 1

   Figure 1

Figure 1: Part of the CakePHP class structure, highlighting the dispatch
process.

Unfortunately request handling in CakePHP does not only include passing
parameters (GET and POST) to the *CakeRequest* object, but also includes
transforming and defining some special request objects. What follows is
an in-depth description of request handling in CakePHP.

CakePHP uses *CakeRequest* to handle and represent all kinds of HTTP
requests inside the framework. Therefore this class reads the values of
*:math:`_GET*, *`\ *POST\*, \*:math:`_COOKIE*, *`\ *SESSION* and
\*:math:`_SERVER*, which are automatically setted by PHP. Cake calls GET parameters params and POST parameters data and stores them in two different arrays _`\ params\_
and *$data*. Params can be accessed using the magic array methods (see
below), while data values can be accessed using the *data()* method of
*CakeRequest*.

GET parameters (params) in CakePHP:

.. raw:: html

   <pre>
   $request = new CakeRequest(); 
   $request['key'] = 'value'; 
   echo $request['key'];
   </pre>

POST parameters (data) in CakePHP:

.. raw:: html

   <pre>
   $request = new CakeRequest(); 
   $request->data('key', 'value'); 
   echo $request->data('key');
   </pre>

In addition the params array can contain values and arrays with a
special meaning. This is because of the *Convention over Configuration*
philosophy of CakePHP. For example the params *controller* and *action*
are used to define which method in which class is executed, that means
these params contain program logic for the given request. Another
special element of params is *pass*. Pass contains variables which
should be passed directly to the action method instead of being stored
in the request object. For example we could take a look at the default
method definition of the view action.

Parameters defined in the pass array are directly passed to the action
method.

.. raw:: html

   <pre>
   public function view($id = null) { 
       // Controller logic 
   }
   </pre>

Another special parameter Bancha needs to take care of is named.
Normally this array is generated by the CakePHP Router based on the
given URL. But because Bancha only used one URL and passes all other
parameters to the server in the POST request body, Bancha needs to
create the named parameter manually. While the named array has also
other purposes in a normal CakePHP application, it is used mainly to
define paging properties in the Bancha context.

In the next two sub chapters we want to further define how
controllers/actions and paging parameters are handled in CakePHP.

Controllers and actions in CakePHP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The name of a controller in CakePHP is the name of in corresponding
model in plural form, the class also is suffixed with the word
Controller. For example:

-  Model: Article
-  Controller name: Articles
-  Controller class name: ArticlesController

The Dispatcher class of CakePHP loads and creates the object
automatically based on the value with the key controller in the params
array.

The action is based on the value of *action* in the params array and
Cake automatically executes the method with this name from the
controller object.

When using CRUD, CakePHP uses the following names:

.. raw:: html

   <table>
    <tr><td> 

Create

.. raw:: html

   </td><td> 

add

.. raw:: html

   </td></tr>
    <tr><td> 

Retrieve

.. raw:: html

   </td><td> 

index/view

.. raw:: html

   </td></tr>
    <tr><td> 

Update

.. raw:: html

   </td><td> 

edit

.. raw:: html

   </td></tr>
    <tr><td> 

Delete

.. raw:: html

   </td><td> 

delete

.. raw:: html

   </td></tr>
   </table>

There are two methods to retrieve data. *index()* loads a list of
entities, while *view()* loads an entity with a specific ID. *view()*,
*edit()* and *delete()* require a pass parameter $id.

Paging in CakePHP
~~~~~~~~~~~~~~~~~

CakePHP automatically supports paging when passing the right parameters
to an action. Thus Bancha needs to define these paging parameters in
order to invoke the paging process in Cake. The paging parameters are
stored, as mentioned before, as an param named paging. We now describe
the paging parameters:

-  *page*: The current page, starts with 1. The default value is 1.
-  *limit*: Maximum number of entities per page, default for Bancha is
   500.
-  *order*: Associative array where the key is the name of a field of a
   model (for example *Article.title*) and the value is the direction
   (asc or desc).
-  *conditions*: Conditions that a row must fulfill in order to be
   included in the result set. See
   http://book.cakephp.org/view/1030/Complex-Find-Conditions for more
   information on find conditions in CakePHP.

16. *fields*: Array with column names (including the model name) that
    should be included in the result set.
17. *recursive*: If true related models will also be fetched from the
    database.

Bancha Architecture
-------------------

The problem we faced is that ExtJS occasionally sends multiple (logical)
requests inside a single HTTP request, but CakePHP is not able to
process multiple requests. Therefore we needed to somehow extend the
dispatching process to handle multiple requests.

In order to process batch requests we needed to replace the default
CakePHP Dispatcher with an implementation that understands batch
requests. We do this by adding a custom front controller, which uses
*BanchaDispatcher* instead of the default *Dispatcher* class. In
addition we also replace *CakeRequest* and *CakeResponse*.

The purpose of these classes is to give the developer an abstract view
of the HTTP request and response. Our *BanchaRequestCollection* class
contains a method getRequests() which parses the request and creates an
instance of *CakeRequest* for every logical request inside the batch
request. *BanchaResponseCollection* works the other way around. It takes
multiple *CakeResponse* objects and combines them into a single batch
response and sends them to the browser.

But there is another challenge with responses. Because it should be
possible to use Bancha without changing a lot of the programs logic in
the controller it is required to automatically transform the data
structure returned from the action into the format defined by Ext
Direct. This task is done by *BanchaResponseTransformer*. There is also
a class called *BanchaRequestTransformer* which does all the request
transformations.

The main challenge with the Bancha architecture was that it should work
without modifying the source code of CakePHP. An additional requirement
was that it should be possible to add Bancha to existing projects
without much effort. Therefore we designed the *BanchaDispatcher* as a
wrapper for the normal CakePHP dispatching process. Thus the Dispatcher
class of CakePHP is invoked with a *CakeRequest* object by
*BanchaDispatcher*. When the normal CakePHP dispatching process is
completed, *BanchaDispatcher* receives a *CakeResponse* object for every
dispatched request and packs them using *BanchaResponseCollection*. This
way we try to minimize incompatibility with future versions of CakePHP.

Figure 2 highlights that the Bancha architecture only wrappers the
normal CakePHP architecture and therefore offers a great amount of
compatibility with existing CakePHP applications.

.. figure:: images/image05.png
   :align: center
   :alt: Figure 2

   Figure 2

Figure 2: Part of the Bancha architecture showing the dispatching
process.

