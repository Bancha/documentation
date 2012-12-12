Controller Method Return Values
===============================

Bancha expects each remotable method to return the result for the remotable request. It supports two options of return values:  

If the result is an array with a success property, it will be traited as a final response and will not be transformed in any way.

Otherwise it will be transformed, depending on the input. You can find more information below.


Final ExtJS/Sencha Touch response
---------------------------------
In this case you must provide an array with a success property, and Bancha will deliver the data as given. 

A possible controller return value for this would be:

::

    array( 
        'success' => true // this is the only necessary property
        'data' => array( 'whatever' => true ), 
        'message' => 'This is a message for the client.'
    )


In all other cases Bancha will transform the data.

Boolean Value
------------

This indicates to Bancha that the operation was or was not successful, 
on the client side this will either trigger the failure or the success
listener.

Bancha will transform this to the ExtJS/Sencha Touch response:

::

    array( 
        'success' => (boolean) $result
    )


Cake One Record
---------------

If you return a cake single record Bancha will transform it into the 
ExtJS/Sencha Touch structure. This is like it is used in the add or 
edit method. 

A possible input looks like this:

::

    array( 
        'User' => array( 
            'id' => 5, 
            'name' => 'Joe', 
        ),
    )

You don't have to clean this array from possible associated data, Bancha
does this for you.

Cake Multi-Result
--------------

If you return a list of cake records Bancha will transform it into the 
ExtJS/Sencha Touch structure. 

A possible input looks like this:

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

You don't have to clean this array from possible associated data, Bancha
does this for you.

Paginated results
-----------------

This is a special structure, created by calling from inside the cake
controller method:

::

    return array_merge($this->request['paging']['_MODELNAME_'],array('records'=>$queried_models)); 

We need this structure to map cakes paging properties to ExtJS/Sencha Touch. Bancha will 
scaffold any CRUD index method like above to return a list of records to 
ExtJS/Sencha Touch stores.

Arbitrary Data
------------

All results which do not match any of the above will be handled as arbitrary data and 
transformed to:

::

    array( 
        'success'=>true, 
        'data' => $result
    ) 



That's it, enjoy.

