How to Expose Controller Methods
================================

Expose the method
-----------------

Simply add following phpdoc comment to the methods you want to expose:

::

        /**
         * @banchaRemotable
         */

Define the result value
-----------------------

The result will be the return value of the controller method. See also
`supported controller
result-values <./Supported-Controller-Method-Results.html>`_. 

Example
-------

::

    <?php
    class UsersController extends AppController {

        /**
         * @banchaRemotable
         */
        public function getAnonymizedRecord($id = null) {

            // your business logic here
            $this->User->id = $id;
            if (!$this->User->exists()) {
                throw new NotFoundException(__('Invalid user'));
            }
            $user = $this->User->read(null, $id);
            $user['User']['firstname'] = '***';
            $user['User']['lastname'] = '***';

            // data for non-bacha requests
            $this->set('user', $user);

            // result for bancha requests
            return $user;
        }
    }

ExtJS usage
-----------

Bancha just creates Ext.Direct stubs which can be accessed with

Bancha.getStub(*[controller name in singular]*).*[controller method
name]*.(*param1*,*param2*,...*paramX*,callback);

From our example above:

::

    Bancha.getStub('User').getAnonymizedRecord(id,callback);

