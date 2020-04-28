---
layout: post
status: publish
published: true
title: Bypassing the Titanium Studio required authentication
author:
  display_name: kinabalu
  twitter: kinabalu
  login: kinabalu
  email: andrew@mysticcoders.com
  url: http://www.mysticcoders.com
author_login: kinabalu
author_email: andrew@mysticcoders.com
author_url: http://www.mysticcoders.com
wordpress_id: 1810
wordpress_url: http://www.mysticcoders.com/?p=1810
date: '2012-02-07 12:08:21 +0000'
date_gmt: '2012-02-07 19:08:21 +0000'
tags: []
comments: true
---
<img src="https://www.mysticcoders.com/wp-content/uploads/2012/02/Screen-Shot-2012-02-07-at-10.59.43-AM.png" border="0" />

A flight is sometimes the perfect time for uninterrupted work.  So as we've been building things with the mobile toolkit <a href="http://www.appcelerator.com" target="_blank">Appcelerator Titanium</a>, I ran it from my trusty key shortcut.  Above is the only thing I saw.  I even warn folks about this when giving talks about how to rapidly develop iOS applications with Titanium.

How do we bypass this?  <em>Please note, that doing this will disable debugging capability within the Titanium Studio, and possibly won't work past the latest versions</em>.

Follow the instructions for your platform to "<a href="https://wiki.appcelerator.org/display/tis/Modifying+Your+Configuration">Find the TitaniumStudio.ini file</a>"

<strong>Add the following at the end of the file</strong>

<pre>
-Dtitanium.bypassAuthentication=true
</pre>
That's it.  When you run it without an internet connection, development bliss.  Peace and quiet.

