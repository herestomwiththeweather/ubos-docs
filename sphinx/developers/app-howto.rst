How to make an app available on UBOS
====================================

If you have an app that you'd like to make available to UBOS users, it is recommended
you do this:

#. Say "Hi" in the `forum </community/>`_. We don't bite, and might even be
   helpful :-)

#. Set up a :doc:`development machine <setting-up-development-machine>`.

#. Package your app using ``makepkg``, with a ``PKGBUILD`` file and a
   :doc:`ubos-manifest`. You can find examples in :doc:`toyapps`, and documentation
   in other sections of this site.

#. Test that your app plays nicely on UBOS with :doc:`webapptest <app-test>`.

#. Augment the list of UBOS build files `here <https://github.com/uboslinux/ubos-buildconfig/tree/master/hl/us>`_,
   and file a pull request, so your app gets built and tested by the official UBOS build.

Done!
