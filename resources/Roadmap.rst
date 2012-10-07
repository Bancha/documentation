Roadmap
=======

List of Releases
>>>>>>>>>>>>>>>>

Beta2-Release
~~~~~~~~~~~~~

1. Fixed issues with data mapping
2. Provided saveFields method to all Bancha models
3. Fixed php tests and added new ones.
4. Fixed issues crud issues
5. Fixed issues with formHandler
6. Added basic support for dates
7. Fixed validation range rule (cake and ext have different
   understandings)
8. Updated samples

Beta3-Release
~~~~~~~~~~~~~

1. Mayor refactoring, and easier installation
2. Enabled Bancha to expose Controller methods
3. Updated and clearified documentation
4. Added a Setup-Check for easier error finding

Beta4-Release
~~~~~~~~~~~~~

1. Bug fixes

1.0 Release Candidate
~~~~~~~~~~~~~~~~~~~~~

1.  Tested in all mayor browsers
2.  Sencha Touch Support
3.  File Uploads
4.  Support to expose Controller Methods
5.  Created Setup-Check.html for easier Bancha setup
6.  Lots of additional tests and Bug fixes
7.  Added a Namespace for created models
8.  Added support for Ext Class Loader
9.  Enabled api caching
10. Cleaned up bootstraping

1.0 Stable Release (current download version)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Added Bug Fixes
2. Added Support for CakePHP 2.2
3. Added ValidationRules-Transformations
4. Extended setup check page
5. Partially added legacy support for IE6+
6. Added Bancha.getStub('Article') method

1.1 Release
~~~~~~~~~~~

1. Added Bug Fixes
2. Added support for PHP strict mode
3. Added support for virtual fields
4. Partially added Localization support
5. Added better debugging support
6. Added default error handling in debug mode
7. Added server-side error logging for production mode

Current github version
~~~~~~~~~~~~~~~~~~~~~~

-

1.2 Release
~~~~~~~~~~~

1. Full support for non ECMAScript 5 engines (IE6,7,8)
2. Cake Bake Console for Bancha models

1.3 Release
~~~~~~~~~~~

1. Full Localization support

1.4 Release
~~~~~~~~~~~

1. Enable sending of nested data in read responses, see
   `implicitIncludes <http://docs.sencha.com/ext-js/4-0/#!/api/Ext.data.reader.Reader-cfg-implicitIncludes>`_

1.5 Release
~~~~~~~~~~~

1. Recognize cake validation errors and push them to extjs (see
   BanchaBahviour::saveFields)
2. Support reads with filters
3. Extends date and datetime support (+debug messages on dev failures)
4. Force consistent requests (basic level)

1.6 Release
~~~~~~~~~~~

1. Fully support the consistent model.
2. Support admin routing (BanchaRemotbaleBehavior config
   'Routing.prefix', and dispatcher mapping)

later
~~~~~

1. Setup Hudson and Phing for automatic builds
2. Native support for Ext.data.TreeStore with server-side TreeBehavior

