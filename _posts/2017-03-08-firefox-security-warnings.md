---
title: Disable annoying security warnings in Firefox
location: Rastatt
---
In Firefox 52, Mozilla has added a "feature" that shows a warning on any login fields if the connection is not secured with HTTPS. You can disable this warning by changing a configuration value:

1. Go to `about:config`
2. Search for "secure"
3. Change the value of `security.insecure_field_warning.contextual.enabled` to **false** by double-clicking on it.

If you want to restore Autofill on HTTP sites, you will also need to change the value of `signon.autofillForms.http` to true in the same way.
