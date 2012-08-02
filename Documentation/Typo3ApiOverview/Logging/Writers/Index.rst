.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../../Includes.txt

.. _Typo3ApiOverview-Logging-Writers:

Log Writers
===========

The purpose of a log writer is (usually) to save all log entries into a persistent storage, like a log file, a database table, or to a remote syslog server.

Different log writers offer possibilities to log into different targets. Custom log writers can extend the functionality shipped with TYPO3 core.


Built-in Log Writers
--------------------

DatabaseWriter
^^^^^^^^^^^^^^
logs into a database.

=========  ==========  ============================  =============================
Option     Status      Description                   Default
=========  ==========  ============================  =============================
logTable   optional    Database table                ``sys_log``
=========  ==========  ============================  =============================


FileWriter
^^^^^^^^^^

logs into a log file, one log entry per line. The file name of the log file can be configured using the :php:`logFile` directive in the :ref:`Typo3ApiOverview-Logging-Configuration-Writer`.
If the log file does not exist, it will be created (including parent directories, if needed). Please make sure that your web server has write-permissions to that path and it is below the root directory of your web site (defined by :php:`PATH_site`).

=========  ==========  ============================  =============================
Option     Status      Description                   Default
=========  ==========  ============================  =============================
logFile    optional    Path to log file              ``typo3temp/logs/typo3.log``
=========  ==========  ============================  =============================


.. _Typo3ApiOverview-Logging-Writers-PhpErrorLogWriter:

PhpErrorLogWriter
^^^^^^^^^^^^^^^^^

logs into the PHP error log.


SyslogWriter
^^^^^^^^^^^^
logs into the syslog (Unix only).

=========  ==========  ============================  =============================
Option     Status      Description                   Default
=========  ==========  ============================  =============================
facility   optional    Syslog facility to log into   ``USER``
=========  ==========  ============================  =============================


Custom Log Writers
------------------

Custom log writers can be added through extensions. Every log writer has to implement the interface :php:`t3lib_log_writer_Writer` and you have to adjust your :ref:`Configuration <Typo3ApiOverview-Logging-Configuration>` to make use of the new writer.

Please keep in mind that TYPO3 will silently continue operating, in case a log writer is throwing an exception while executing the :php:`writeLog()` method. Only in the case that all registered writers fail, the log entry plus additional information will be added to the configured fallback logger (which defaults to the :ref:`PhpErrorLog <Typo3ApiOverview-Logging-Writers-PhpErrorLogWriter>` writer).
