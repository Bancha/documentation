Supported Validation Rules
==========================

Bancha currently understands following validation rules

General:

-  notempty

Strings:

-  minLength
-  maxLength
-  between
-  alphaNumeric
-  email
-  url

Numbers:

-  numeric with an additional config "precision". To allow two decimal
   places you would define: 'precision'=>2
-  range (please always define precision when using)

Files:

-  extension (the rule marks that this is an file field, property
   extension can be an array of allowed types)

