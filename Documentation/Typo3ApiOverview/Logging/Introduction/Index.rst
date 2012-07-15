

.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. ==================================================
.. DEFINE SOME TEXTROLES
.. --------------------------------------------------
.. role::   underline
.. role::   typoscript(code)
.. role::   ts(typoscript)
   :class:  typoscript
.. role::   php(code)

Introduction
^^^^^^^^^^^^

TYPO3 Logging consists of the following objectives:

* :ref:`Typo3ApiOverview-Logging-Logger` that receives the Log message and related details, like a severity
* :ref:`Typo3ApiOverview-Logging-Configuration`
* :ref:`Typo3ApiOverview-Logging-Processors` that process log messages and enrich it with additional information
* :ref:`Typo3ApiOverview-Logging-Writers` that write the log entries to different targets (like file, database, rsyslog server, etc.)


Quick Usage
-----------

Instantiate a logger for this class:

.. code-block:: php

   <?php
   /** @var t3lib_log_AbstractLogger */
   $logger = t3lib_log_LogManager::getLogger(__CLASS__);

Log a simple message:

.. code-block:: php

   <?php
   $logger->info('everything went fine, puhh');
   $logger->warning('A warning!');

Provide additional information with the log message:

.. code-block:: php

   <?php
   $logger->error(
     'This was not a good idea',
     array(
       'param1' => $param1,
       'param2' => $param2,
     )
   );

:php:`$logger->warning()` etc. are only shorthands - you can also call :php:`$logger->log()` directly and pass the severity level:

.. code-block:: php

   <?php
   $logger->log(t3lib_log_Level::WARNING, 'Another warning');
