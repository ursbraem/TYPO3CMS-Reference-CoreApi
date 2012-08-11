.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../../Includes.txt

.. _Typo3ApiOverview-Logging-Writers:

Log Writers
===========

The purpose of a log writer is (usually) to save all log records into a persistent storage, like a log file, a database table, or to a remote syslog server.

Different log writers offer possibilities to log into different targets. Custom log writers can extend the functionality shipped with TYPO3 core.


Built-in Log Writers
--------------------

This section describes the log writers shipped with the TYPO3 core. Some writers have options to allow customization of the particular writer. See the :ref:`Configuration <Typo3ApiOverview-Logging-Configuration-Writer>` section, how to use these options.

DatabaseWriter
^^^^^^^^^^^^^^
The database writer logs into a database table. This table has to reside in the database used by TYPO3 and is *not* automatically created.

=========  ==========  ============================  =============================
Option     Mandatory   Description                   Default
=========  ==========  ============================  =============================
logTable   no          Database table                ``sys_log``
=========  ==========  ============================  =============================


FileWriter
^^^^^^^^^^

The file writer logs into a log file, one log record per line.
If the log file does not exist, it will be created (including parent directories, if needed). Please make sure that your web server has write-permissions to that path and it is below the root directory of your web site (defined by :php:`PATH_site`).
If :php:`$TYPO3_CONF_VARS['SYS']['generateApacheHtaccess']` is set, an .htaccess file is added to the directoy. It protects your log files from being accessed from the web.

=========  ==========  ============================  =============================
Option     Mandatory   Description                   Default
=========  ==========  ============================  =============================
logFile    no          Path to log file              ``typo3temp/logs/typo3.log``
=========  ==========  ============================  =============================


.. _Typo3ApiOverview-Logging-Writers-PhpErrorLogWriter:

PhpErrorLogWriter
^^^^^^^^^^^^^^^^^

logs into the PHP error log.


SyslogWriter
^^^^^^^^^^^^
logs into the syslog (Unix only).


=========  ==========  ============================  =============================
Option     Mandatory   Description                   Default
=========  ==========  ============================  =============================
facility   no          Syslog Facility_              ``USER``
                       to log into.
=========  ==========  ============================  =============================

.. _Facility: http://en.wikipedia.org/wiki/Syslog#Facility_Levels

Custom Log Writers
------------------

Custom log writers can be added through extensions. Every log writer has to implement the interface :php:`t3lib_log_writer_Writer`.
It is suggested to extend the abstract class :php:`t3lib_log_writer_Abstract` which allows you use configuration options by adding the corresponding properties and setter methods.


Please keep in mind that TYPO3 will silently continue operating, in case a log writer is throwing an exception while executing the :php:`writeLog()` method. Only in the case that all registered writers fail, the log entry plus additional information will be added to the configured fallback logger (which defaults to the :ref:`PhpErrorLog <Typo3ApiOverview-Logging-Writers-PhpErrorLogWriter>` writer).
