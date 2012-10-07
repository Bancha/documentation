Supported Controller Method Results
===================================

Here you find all supported ontroller-Method Result Types

TRUE / FALSE
------------

This indicates that the operation was or was not successful, on the
client side this will either trigger the failure or the success
listener.

Cake One Record
---------------

::

    array( 
        'User' => array( 
            'id' => 5, 
            'name' => 'Joe', 
        ), 
        'success' => true // optional, true is default 
    )

This returns one record, like it is used in the add or edit method. The
optional success property tells the client is the success or failure
trigger should be called.

You don't have to clean this array from possible associated data, Bancha
does this for you.

Cake Multi-Result
-----------------

::

    array( 
        '0' => array( 
            'User' => array( 
                'id' => 5, 
                'name' => 'Joe', 
            ), 
        ), 
        '2' => array( 
            'User' => array( 
                'id' => 6, 
                'name' => 'Roland', 
            ), 
        ), 
    )

This returns multiple records, like it is used in the index method.

You don't have to clean this array from possible associated data, Bancha
does this for you.

Paginated results
-----------------

This is a special structure, created by calling from inside the cake
controller method:

::

    return array_merge($this->request['paging']['_MODELNAME_'],array('records'=>$users)); 

We need this structure to map cakes paging properties to extjs.

ExtJS formated Data
-------------------

If the data is not in any of the above, it will just be passed through.
So it is also possible to pass something like this:

::

    array( 
        'success'=>true, 
        'data' => array( 
            'whatever' => true 
        ) 
    ) 

That's it, enjoy.

