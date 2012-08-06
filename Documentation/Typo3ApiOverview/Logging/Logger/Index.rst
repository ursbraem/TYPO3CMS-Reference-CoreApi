.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../../Includes.txt


.. _Typo3ApiOverview-Logging-Logger:

Logger
======

Instantiation
-------------

The :php:`LogManager` enables an auto-configured usage of :php:`Loggers` in your PHP code by reading the logging configuration and setting the minimum severity level of the Logger accordingly.

.. code-block:: php

   <?php
   /** @var t3lib_log_AbstractLogger */
   $logger = t3lib_log_LogManager::getLogger(__CLASS__);

Using :php:`__CLASS__` as name for the logger is strictly recommended to enable logging configuration based on the class hierarchy.


Log() method
------------

:php:`t3lib_log_Logger` provides a central point for submitting log messages the :php:`log()` method:

.. code-block:: php

   <?php
   class t3lib_log_Logger {
     function $logger->log($level, $message, array $data = array());
   }

:php:`$level` is one of
  - :php:`t3lib_log_Level::EMERGENCY`
  - :php:`t3lib_log_Level::ALERT`
  - :php:`t3lib_log_Level::CRITICAL`
  - :php:`t3lib_log_Level::ERROR`
  - :php:`t3lib_log_Level::WARNING`
  - :php:`t3lib_log_Level::NOTICE`
  - :php:`t3lib_log_Level::INFO`
  - :php:`t3lib_log_Level::DEBUG`

:php:`$message`
   Contains the log message as string.

:php:`$data`
   Optional parameter, can contain additional data, which is added to the log record in form of an array.

An early return in the :php:`log` method prevents unneeded computation work to be done. So you are safe to call :php:`$logger->debug()` frequently without slowing down your code a lot. The Logger will know by its configuration, what it most explicit severity level is.

As next step, all registered :ref:`Processors <Typo3ApiOverview-Logging-Processors>` will be notified, which can modify the log entries or add additional information.

The Logger then forwards the log entry to all of its configured :ref:`Writers <Typo3ApiOverview-Logging-Writers>`, which will then persist the log entry.

Shorthand methods
-----------------

For each of the :php:`$level` choices mentioned above, a shorthand method exists for :php:`t3lib_log_Logger`, like

* :php:`t3lib_log_Logger->debug($message, array $data = array());`
* :php:`t3lib_log_Logger->info($message, array $data = array());`
* :php:`t3lib_log_Logger->notice($message, array $data = array());`
* etc.