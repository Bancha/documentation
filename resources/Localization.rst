This is still a working draft, will be ready in a few days
==========================================================

Trouble Shooting
~~~~~~~~~~~~~~~~

**Can't load translations**

If your browser can't load the language file (e.g.
/bancha/bancha/translations/eng.js) your Apache is probably configured
with MultiViews. To fix this add following to your
app/webroot/.htaccess:

::

    Options -Multiviews

Background: When multiViews are on, apache "transforms" the above url to
bancha.php, and cake can't call the correct controller method. See also
http://httpd.apache.org/docs/current/content-negotiation.html#multiviews

Translate strings in JavaScript
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Set the current Language in Bancha.Localizer.currentLang. Then you can
use Bancha.t('some string') and you will get the translated string as
result. Bancha also supports %s to replace strings, e.g.

::

    {
        xtype: 'button',
        text: Bancha.t('Save to folder %s', folderName)
    }

Collect translations
~~~~~~~~~~~~~~~~~~~~

You can collect all translations (similar to **cake i18n extract**) by
typing in you shell from inside the app folder **Console/cake
bancha.jsi18n**
