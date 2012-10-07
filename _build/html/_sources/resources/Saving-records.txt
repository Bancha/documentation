Saving Records
==============

CakePHP's model save record expects all fields to be defined. But to
save bandwidth and make the usage more flexible (you don't have to share
all data with the frontend) Bancha uses Sencha Touch/ExtJS's partial
saving (`Writer
writeAllFields:false <http://docs.sencha.com/ext-js/4-0/#!/api/Ext.data.writer.Writer-cfg-writeAllFields>`_).

CakePHP would now go ahead and set all missing fields to null. That's
why the BanchaBehavior adds new methods to every model:

saveFields
~~~~~~~~~~

Use this method instead of Model::save, it creates a new record if no id
is provided and updates if an id is provided. This internally sets cakes
model whitelist property to only save the provided fields before before
saving. You can also use this methods for normal CakePHP requests and it
will behave like a normal save operation.

getLastSaveResult
~~~~~~~~~~~~~~~~~

This will return the result of the last model save in the correct Bancha
result format. So a common last line in your controller method could be
*return $this->User->getLastSaveResult();*

saveFieldsAndReturn
~~~~~~~~~~~~~~~~~~~

This is a convenience method, combining the above two. Bancha expects
every controller method to return a result. So if you last actions is to
save a record and then return the result (like in a typical create or
update request) you can use this method.

