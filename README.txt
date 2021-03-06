Plasti-Auth, Version 0.3
===================================================
This is an authentication plugin for Oracle Application Express 4.1. It's still under development and wont be ready for use in production until version 1.0. Feel free to contribute fixes and improvements.

Please read ADDITIONAL DETAILS and understand the risks before you enable session recycling.

Existing Features

* Requires a specified custom authentication function
* Recycle user session from browser cookie based on a page id whitelist
* Clear REQUEST variable when recycling to prevent default DML processes being invoked
* Optional confirmation page displayed before recycling takes place
* Error page displayed for incoming links to pages that are not in the whitelist
* Use the template of a given page when rendering confirm and error pages

Proposed Features

* Referer whitelist/regex and corresponding error page
* Configurable session recycling behaviour when landing on a public page with no session


TABLE OF CONTENTS
=================

* Installation and Update
* How to use
* Additional Details
* FAQ
* Uninstall
* Credits, License and Terms of Use
* Contact and Support
* Change Log


INSTALLATION AND UPDATE
=======================
1. Ensure you are running Oracle APEX version 4.0 or higher
2. Unzip and extract all files
2. Access your target Workspace
3. Select the Application Builder
4. Select the Application where you wish to import the plug-in 
   (plug-ins belong to an application, not a workspace)
5. Access Shared Components > Plug-Ins
6. Click [Import >]
7. Browse and locate the installer file, plasti-auth.sql
8. Complete the wizard

If the plug-in already exists in that application, you will need to confirm that you 
want to replace it.  Also, once imported, this plug-in can be installed into additional
applications within the same workspace.  Simply navigate to the Export Repository 
(Export > Export Repository), click Install, and then select the target application.
Once the install file is no longer needed, it can be removed from the Export Repository.


HOW TO USE
==========
1. Install the plug-in (see INSTALLATION AND UPDATE)
2. Navigate to Shared Components | Authentication Schemes | Create
3. Select "Based on a pre-configured scheme from the gallery" | Next
4. Name your scheme, select the installed plugin as the type ("Plasti-Auth [Plug-in]" by default)
5. Follow the in line help links to configure the rest of the plugin.

You must explicitly enable Session Recycling to actually get this plugin to do anything useful. This is by design to ensure people think for a minute before blindly switching it on.


ADDITIONAL DETAILS
==================
It is not currently recommended to use this plugin in it's present state in any production environment where security is a concern.

External links into the system may be generated by an email notification or shared amongst colleagues. By default, such external links will trigger a new session to be created. The old session cookie is lost as soon as the login screen is displayed leaving the user with no way to recover their existing session.

Enabling session recycling avoids the need for the user to log in when clicking such links, but can also expose Cross-site Request Forgery vulnerabilities (CSRF). For this reason a whitelist of pages permitted for external linking must be filled in and any REQUEST variable passed in such links will be ignored to further reduce the risk of inadvertent DML from clicking a link.

IMPORTANT: To guard against CSRF, do not place DML page processes on whitelisted pages unless they are conditional on the REQUEST variable either explicitly or due to button submit conditions.

Even with these measures, JavaScript injected into any page in any application on the same domain could access the confirmation page in the background without the users knowledge and then dig out the session id without any user interaction. In future, this plugin will have configurable referrer checks to further mitigate such risks. Until this is implemented, do not enable session recycling on a domain that shares multiple applications.


FAQ
===
* Q: How can this improve my application?
  A: Allows users to painlessly follow links sent via notification emails or instant messaging amongst collaborative teams

* Q: Will this make my application less secure?
  A: Almost certainly yes, see ADDITIONAL DETAILS and then weigh up the risks

* Q: So is this essentially a trade off of security for usability?
  A: Pretty much, with the goal of making the security trade off as minimal as possible. There are some significant usability issues in APEX 4.1 that may affect some kinds of users more than others. See this wiki page for a full discussion and other possible solutions... https://github.com/CaptEgg/Plasti-Auth/wiki/Plasti-Auth-APEX-Authentication-Plugin


UNINSTALL
=========
To completely remove the plug-in:

1. Access the plug-in under Shared Components > Plug-Ins
2. Click [Delete]
   Note: If the [Delete] button doesn't show that indicates the plug-in is in use within the
         application.  Click on 'Utilization' under 'Tasks' to see where it is used.


CREDITS, LICENSE AND TERMS OF USE
=================================
This plugin is distributed as open-source under the terms of the MIT Licence

Copyright (c) 2012 Sam Hall, Charles Darwin University. All rights reserved.

Permission is hereby granted, free of charge, to any person obtaining a copy 
of this software and associated documentation files (the "Software"), to deal 
in the Software without restriction, including without limitation the rights 
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell 
copies of the Software, and to permit persons to whom the Software is 
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all 
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR 
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, 
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE 
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER 
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, 
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE 
SOFTWARE.


CONTACT AND SUPPORT
===================
Sam Hall
https://github.com/CaptEgg/Plasti-Auth

Please use above GitHub repository issues register to report any bugs or issues with this plugin.


CHANGE LOG
==========
v0.1 (08-Mar-2012)
-) Buggy proof of concept

v0.2 (12-Mar-2012)
-) Reliably recycle sessions with a confirmation page

v0.3 (08-May-2012)
-) ODTUG competition release
-) Template based on existing page
-) Error page added
-) Restructured the GitHub project
