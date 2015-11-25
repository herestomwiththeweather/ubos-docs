User FAQ
========

(See also :doc:`troubleshooting` and the :doc:`developer FAQ <../developers/faq>`.)

Why is it called UBOS?
----------------------

Why not? :-)

Also, you can pronounce it in a way that makes a lot of sense for the kinds of things
UBOS was created to do.

Why does UBOS ask for a domain name when installing a new site?
---------------------------------------------------------------

UBOS mostly cares about applications that are accessible over the web. To access an
application over the web, your browser needs to know which server to talk to (the web is
a big place!) and the common way of doing that is to use domain (DNS) names. This is
why ``ubos-admin createsite`` asks for a domain name.

I own a domain name, and I'd like to use it for my UBOS device. How do I do that?
---------------------------------------------------------------------------------

Generally, you do two things:

* When you set up your site with ``ubos-admin createsite``, you specify that domain name.
  (You could also say ``*``, but if you specify the domain name you can later add another
  site with a different domain name that runs on the same UBOS device.)
* You instruct your domain name registrar or name server to resolve your domain to your
  UBOS device. The exact details of this process depend on your registrar or name server,
  so we cannot describe this here. But in general, you set up an "A" record that points
  to the IP address of your UBOS device.

Can I use UBOS without purchasing a domain name?
------------------------------------------------

In two ways:

* You can use the IP address of your UBOS device instead of a domain name, if you
  have specified ``*`` as the domain name when executing ``ubos-admin createsite``.
* If you are satisfied with accessing your UBOS device only on your local network,
  the UBOS device advertises itself via mDNS. See :doc:`networking`.

Is it safe to have my site accessible from the public web?
----------------------------------------------------------

This is one of these unanswerable questions. Like: is it safe to go on a two-week vacation
or will my house be broken into while I'm gone? We can't quite answer that question.

But here are some thoughts:

* As long as UBOS is in beta, assume there are bugs.
* Once UBOS is out of beta, still assume that there are bugs.
* To see which bugs have been filed, go to `Github <https://github.com/uboslinux/>`_.
* As an open-source project, UBOS is "as-is" and we make no warranties of any kind.
* So far, we are not aware of any break-in or compromise of any UBOS system by
  anybody.
* We run UBOS ourselves, and we definitely don't want to be compromised.

I found a bug.
--------------

Please tell us about it by filing it
`on Github <https://github.com/uboslinux/ubos-admin/issues/new>`_

Help! I have trouble!
---------------------

What about visiting our :doc:`troubleshooting` section?

What should I do if I get an error, and I don't know how to solve it myself?
----------------------------------------------------------------------------

Here are some things you can do:

* Consult the `UBOS user documentation <http://ubos.net/docs/user/>`_, in particular
  the section about :doc:`troubleshooting`.
* Ask a friendly Linux geek you might know.
* Come find us `on IRC <http://ubos.net/community/>`_ and ask there.