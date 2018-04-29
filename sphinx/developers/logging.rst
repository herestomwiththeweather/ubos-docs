Logging
=======

Things may go wrong. It's computers, after all. If a log exists, developers and/or
users have a chance of figuring out what happened, and fix whatever the problem was.
Without such logs, life can be very frustrating. So, let's log!

Analyzing what happened on a user's device is easier if all code on that device
uses single logging facility. If a problem occurs due to an unexpected interaction
between two different components, for example, a single log has all events in
chronological sequence and debugging is much easier.

UBOS uses the logging facilities provided by ``systemd``. All packages,
:term:`Apps <App>` and :term:`Accessories <Accessory>` are encouraged to use them.

You can find a few excellent introductions to logging via systemd on-line, e.g.

* `Using the Journal <http://0pointer.de/blog/projects/journalctl.html>`_ and
* `Logging to the Journal <http://0pointer.de/blog/projects/journal-submit.html>`_

For general information about systemd, see the
`Systemd home page <http://freedesktop.org/wiki/Software/systemd/>`_.

When logging, developers are encouraged to provide as much contextual information as
needed to understanding the context of the message, in particular the :term:`AppConfigId`
of the installation that the log message refers to. The syslog identifier should be
set to the name of the reporting package.
