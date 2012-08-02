.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../../Includes.txt


.. _Typo3ApiOverview-Logging-Configuration:

Configuration of the Logging system
===================================

Instantiation of Loggers is configuration-free, as the LogManager automatically applies its configuration.

The Logger configuration is read from :php:`$TYPO3_CONF_VARS['LOG']`, which contains an array reflecting the class
hierarchy of your TYPO3 project.

To apply a configuration for e.g. all Loggers with the :php:`t3lib_cache_` prefix, the configuration is read from
:php:`$TYPO3_CONF_VARS['LOG']['t3lib']['cache']`. So every logger requested for classes like :php:`t3lib_cache_Manager`,
:php:`t3lib_cache_backend_Redis`, etc. will get this configuration applied.

The same holds, of course, for extensions: To treat all extensions differently than the core classes, configuration is
searched for in :php:`$TYPO3_CONF_VARS['LOG']['tx']` (as extension class names start with :php:`tx` or :php:`Tx` and all
class names are converted to lowercase).

.. _Typo3ApiOverview-Logging-Configuration-Writer:

Writer configuration
--------------------

The Log Writer configuration is read from the subkey :php:`writerConfiguuration` of the configuration array:

.. code-block:: php

   <?php
   $TYPO3_CONF_VARS['LOG']['writerConfiguration'] = array(
       // configuration for ERROR level log entries
     t3lib_log_Level::ERROR => array(
         // add a FileWriter
       't3lib_log_writer_File' => array(
           // configuration for the writer
         'logFile' => 'typo3temp/logs/error.log'
       )
     )
   );

To apply a special configuration for classes within the "namespace" :php:`tx_myext`, use the following configuration:

.. code-block:: php

   <?php
   $TYPO3_CONF_VARS['LOG']['tx']['myext']['writerConfiguration'] = array(
       // configuration for DEBUG severity, including all
       // less-critical levels (INFO, WARNING, ERROR, ...)
     t3lib_log_Level::DEBUG => array(
         // add a FileWriter
       't3lib_log_writer_File' => array(
           // configuration for the writer
         'logFile' => 'typo3temp/logs/myextension-debug.log'
       )
     ),
       // configuration for WARNING severity, including all
       // less-critical levels (ERROR, CRITICAL, EMERGENCY)
     t3lib_log_Level::WARNING => array(
         // add a SyslogWriter
       't3lib_log_writer_Syslog' => array(),
         // add an additional FileWriter
       't3lib_log_writer_File' => array(
         'logFile' => 'typo3temp/logs/tx_myext-warning.log'
       )
     ),
   );

An arbitrary number of writers can be added for every severity level. The configuration based on severity levels (INFO,
WARNING, ERROR, ..) is applied to log entries of the particular severity level plus all less-critical levels. Thus, a
log messages created with :php:`$logger->warning()` will be affected by a :php:`writerConfiguration` for
:php:`t3lib_log_Level::DEBUG`.

For a list of writers shipped with the TYPO3 Core see the configuration about :ref:`Log Writers <Typo3ApiOverview-Logging-Writers>`.


Processor configuration
-----------------------

According to the writer configuration, log record record processors can be configured on a per-class and per-namespace
basis:

.. code-block:: php

   <?php
   $TYPO3_CONF_VARS['LOG']['processorConfiguration'] = array(
       // configuration for ERROR level log entries
     t3lib_log_Level::ERROR => array(
         // add a WebProcessor
       't3lib_log_processor_Web' => array()
     )
   );