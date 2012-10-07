Introduction
============

This documents explains the design of Bancha and the underlying
technologies which are required to understand how Bancha works. The
Technical Documentation starts with some shorts chapters which explain
used technologies and principles used in the design and development of
Bancha. In the next section the architecture of Bancha is explained.
Dispatching in Bancha explains in great depths how the most complex part
of Bancha, the dispatching process works. The chapter Consistent Model
explains how Bancha ensures consistency in an asynchronous environment.
The last chapter explains the Frontend Architecture of Bancha.

Additional documentation can be found in the source code or at the
following resources:

-  `PHP API Reference <http://docs.banchaproject.org/php>`_
-  `JavaScript API Reference <http://docs.banchaproject.org/js/>`_

Definition of Technologies
--------------------------

This section shortly explains frameworks and technologies, which are
used by Bancha.

**CakePHP**

CakePHP is a highly popular server-side framework written in PHP.

**Ext Direct**

A specification on how to enable RMI from the Client in ExtJS.

Principles of the Bancha Architecture
-------------------------------------

This chapter highlights some of the main principles of the Bancha
architecture, which have effects on how the Bancha code is implemented.
### Do not overwrite CakePHP The source code of CakePHP should not be
overwritten. This is important, because it makes updating the CakePHP
code much easier. Also, if Bancha does not overwrite CakePHP every
CakePHP user can easily install Bancha, even for existing installations.
Therefore Bancha is designed as a CakePHP plug-in.
### Do not overwrite ExtJS The source code of ExtJS should not be
overwritten either. On the ExtJS part this is easier. We just create a
Bancha Singleton which then uses the existing classes and in some parts
extend them.

