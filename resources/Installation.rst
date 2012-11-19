Installation
============

Prerequisites
-------------

1. You have downloaded and installed the lasted release of `CakePHP
   2 <http://cakephp.org/>`_.
2. You have downloaded the lastest release of `ExtJS
   4 <http://www.sencha.com/products/extjs/download/>`_ and/or `Sencha
   Touch 2 <http://www.sencha.com/products/touch/download/>`_

   ::

       Note that you might have to buy a commercial ExtJS license, 
       see http://www.sencha.com/products/extjs/license/

Installation
------------

1. Download the `Bancha source <banchaproject.org/download.html>`_ and 
   place it into the *plugins* folder of your existing CakePHP installation. 
   (You should then either have a *plugins/Bancha* or *app/Plugin/Bancha* folder).

2. Write at the end of your bootstrap (*app/Config/bootstrap.php*):

   .. code-block:: php

       CakePlugin::load(array('Bancha' => array('routes' => true, 'bootstrap' => true)));  

3. Copy the file *app/Plugin/Bancha/_app/webroot/bancha-dispatcher.php*
   to *app/webroot/bancha-dispatcher.php* (Advanced users may want to
   symlink)
4. Add the BanchaPaginatorComponent to your AppController:

   .. code-block:: php

      /**
       * Use the BanchaPaginatorComponent to also support pagination
       * and remote searching for Sencha Touch and ExtJS stores
       */
      public $components = array('Session', 'Paginator' => array('className' => 'Bancha.BanchaPaginator'));  

5. For production you may want to add a caching provider, since Bancha
   uses reflection. For default configuration add this at the end of
   *app/Config/core.php* (please make sure you are using the CakePHP
   2.1.0+ core.php, otherwise $prefix is not defined):

   .. code-block:: php

       /**
        * Configure the cache for Banchas Remote API.
        */
       Cache::config('_bancha_api_', array(
           'engine' => $engine,
           'prefix' => $prefix . 'bancha_api_',
           'path' => CACHE . 'models' . DS,
           'serialize' => ($engine === 'File'),
           'duration' => $duration
       ));

Setting up ExtJS
----------------

1. Place *ext-all.js* and *ext-all-dev.js* into *app/webroot/js/* and
   the ExtJS resources folder into *app/webroot/css/*
2. To use Bancha on a site, include ExtJS and the files */bancha-api.js*
   and */bancha/js/Bancha.js*. For production usage you may want to add
   a parameter to already load all metadata on startup:
   */bancha-api/models/all.js* or
   */bancha-api/models/[Article,User].js*. 

   Code for a plain html file:

   .. code-block:: html

       <!-- include ExtJS css -->
       <link rel="stylesheet" href="/css/resources/css/ext-all.css" type="text/css" />

       <!-- include ExtJS -->
       <script type="text/javascript" src="/js/ext-all-dev.js"></script>

       <!-- include Bancha and the remote API -->
       <script type="text/javascript" src="/Bancha/js/Bancha-dev.js"></script> <!-- for the github version use /Bancha/js/Bancha.js -->
       <script type="text/javascript" src="/bancha-api/models/all.js"></script>



   Code for cake layouts:

   .. code-block:: php

       <?php
           echo $this->Html->css('resources/css/ext-all');
           echo $this->Html->script('ext-all-dev');
           echo $this->Html->script('/Bancha/js/Bancha-dev'); <!-- for the github version use /Bancha/js/Bancha -->
           echo $this->Html->script('/bancha-api/models/all');
       ?>

Setting up Sencha Touch
-----------------------

See also `Bancha for Sencha Touch 2
Screencast <http://vimeo.com/bancha/bancha-for-sencha-touch-2>`_

1. Place *sencha-touch-all.js* and *sencha-touch-debug.js* into
   *app/webroot/js/* and the Sencha Touch resources folder into
   *app/webroot/css/*
2. To use Bancha in a layout include Sencha Touch, and the files
   */bancha-api.js* and */bancha/js/Bancha.js* into your layout. For
   production usage you may want to add a parameter to already load all
   metadata on startup: */bancha-api/models/all.js* or
   */bancha-api/models/[Article,User].js*. Example:

   .. code-block:: html

       <!-- include Sencha Touch css -->
       <link rel="stylesheet" href="/css/resources/css/sencha-touch.css" />

       <!-- include Sencha Touch -->
       <script type="text/javascript" src="/js/sencha-touch-all-debug.js"></script>

       <!-- include Bancha and the remote API -->
       <script type="text/javascript" src="/Bancha/js/Bancha-dev.js"></script> <!-- for the github version use /Bancha/js/Bancha.js -->
       <script type="text/javascript" src="/bancha-api/models/all.js"></script>

Trouble shooting
----------------

After you have successfully finished installation you can open the page
*/Bancha/setup-check.html* to find any installation problem.

For questions write us a mail: support@banchaproject.org

