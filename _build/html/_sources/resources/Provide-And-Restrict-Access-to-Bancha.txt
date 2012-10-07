Provide & Restrict Access to Bancha
===================================

Access to controller methods
----------------------------

There is nothing bancha-specific to do for you here. Bancha uses the
normal *AuthComponent* Controller configurations.

See the `CakePHP
Book <http://book.cakephp.org/2.0/en/core-libraries/components/authentication.html>`_.

Access to Bancha metadata
-------------------------

Bancha by default provides all kind of information about your models to
the user without access restriction. Open */bancha-api/models/all.js* in
your browser to see what Bancha shares.

But if you are using *AuthComponent* by default ONLY authentificated
users can access Bancha, therefore no anonym user can user your Bancha
Application. This can be usefull if you are e.g. using Bancha in an
administration backend.

If you want to still provide access for all users adopt your
AppController like this:

::

        class AppController extends Controller {
            /*
             * Add authentification for your application
             */
            public $components = array(
                'Auth' => array(
                    ...
                )
            );

            public function beforeFilter() {
                // allow the Bancha API and meta data do be loaded from anyone
                if(strtolower($this->params['plugin']) == 'bancha' && strtolower($this->name) == 'bancha') {
                    $this->Auth->allow('index','loadMetaData','translations','logError');
                }
            }
        }

For advanced configurations just ask us: support@banchaproject.com
------------------------------------------------------------------

