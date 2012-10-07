Advanced Configurations
=======================

Configure::write Configurations
-------------------------------

-  **Bancha.Api.stubsNamespace** (Default: 'Bancha.RemoteStubs')

   If you want to use exposed controller methods and don't like the
   *Bancha.RemoteStubs* namespace, you can change it here. Normally
   there is no reason to do this.

   ::

       Configure::write('Bancha.Api.stubsNamespace','MyRemoteStubsNamespace');

-  **'Bancha.Api.remoteApiNamespace** (Default: 'Bancha.REMOTE\_API')

   If you want to use define your Ext.Direct Remote API to a different
   place, change this property and also JavaScript property
   *Bancha.remoteApi*. There should be no reason to do this.

-  **Bancha.allowMultiRecordRequests** (Default: false)

   There is no known reason to every enable this, please think twice
   before enabling it since it maybe makes it very hard for you to find
   errors.

   If you want to send multiple multiple records from ExtJS to CakePHP
   in one action (this is not about request batching!), you have to
   enable this. Normally this is not needed and the according error is
   only triggered because the ExtJS store proxy is configured with
   *batchActions:true*. Please never batch records on the proxy level
   (Ext.Direct is batching them). To enable it add the following to your
   *app/Config/core.php*:

   ::

       Configure::write('Bancha.allowMultiRecordRequests',true)

BanchaRemotableBehavior Configurations
--------------------------------------

The BanchaRemotableBehavior provides following configuration settings,
used like this:

::

    $actsAs = array('Bancha.BanchaRemotable'=>array('useOnlyDefinedFields'=>true));

Options:
~~~~~~~~

-  **useOnlyDefinedFields (Default: true)**

   By default ExtJS only send changes to the server, therefore the
   CakePHP doesn't get the whole record and *Model::save* should only
   save changed fields. Note that this is forced even for "normal" cake
   requests. If true the model also saves and validates records with
   missing fields. If you set this to false please use
   *model->saveFields($data, $options)* for saving ExtJS data
   with partial model fields.

