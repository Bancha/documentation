FAQ
===

I have an existing CakePHP application, can I use Bancha here?
--------------------------------------------------------------

Yes. Bancha will perfectly integrate into your existing project, install
the Bancha Cake-Plugin an get started working on your new ExtJS- or
Sencha Touch Application.

All your original CakePHP pages will work like the are used to.

How can I use associated data?
------------------------------

We are currently supporting associated data in the model (see
`Associations in
ExtJS <http://www.sencha.com/blog/ext-js-4-anatomy-of-a-model>`_), so
all in Cake defined associations are mapped to ExtJS model.

We are not yet supporting associated data in our scaffolding (this seems
to be quite tricky, since ExtJs doesn't provide a elegant solution for
this). You can manually load the store and overwrite the column config
with a custom renderer, like:

::

    // create & load the store (article belongsTo author)
    var authorStore = config.store.author();
    authorStore.load({
        callback: function() {
            // store is laoded, so now build the grid using Bancha scaffolding
            Ext.create('Ext.grid.Panel' {
                ...
                scaffold: {
                    ...
                    afterBuild: function(config) {
                        // overwrite id column type for the name
                        config.columns[1].xtype = 'gridcolumn';

                        // add a renderer
                        config.columns[1].renderer = function(a,b,c) {
                            return store.getAt(store.findExact('id',c.data.author_id)).data.name;
                        };
                    } //eo afterBuild
                } /eo scaffold
            }; //eo panel
    }}); //eo load

Why do you have your own dispatcher?
------------------------------------

The decision for a separate dispatcher wasn't an easy one, but it was
the only way we could support the full Ext.Direct spec. Ext.Direct
allows us to pack multiple requests into one http request, like
[request1, request3, ..]. Cakes dispatcher is a single dispatcher and
can't handle multiple requests since the design expects HTML outputs. So
to accomplish our goal of minimizing network latency we had to wrap
cakes single dispatcher with an Bancha dispatcher which handles multiple
requests and current routing between ExtJS naming and Cakes naming.

A simple example: A app shows a user login, as soon as the user is
logged in 9 different stores/individual records get loaded. Using the
standard cakephp dispatcher the first 4 requests would be send by the
browser (browsers don't support sending of unlimited requests at the
same time), handled and after those the next 4.. everyone with his own
latency, etc… With Banchas Multi-Dispatcher this stays one http request
serving the data in a wrapped way.

The icing on the cake is here that we can keep both worlds separate and
naming always stays by convention. Also some other features, like remote
sorting elegantly integrates into Cake.

More?
-----

**If you have any questions or tips what we should explain better in the
documentation, just write us: mail@rolandschuetz.at**

**Please report bugs to `github
issues <https://github.com/Bancha/Bancha/issues>`_**
