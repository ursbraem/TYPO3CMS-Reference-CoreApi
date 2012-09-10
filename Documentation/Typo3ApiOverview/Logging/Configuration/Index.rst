.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../../Includes.txt


.. _Typo3ApiOverview-Logging-Configuration:

Configuration of the Logging system
===================================

Instantiation of Loggers is configuration-free, as the LogManager automatically applies its configuration.

The Logger configuration is read from :php:`$TYPO3_CONF_VARS['LOG']`, which contains an array reflecting the namespace
and class hierarchy of your TYPO3 project.

Example:

To apply a configuration for e.g. all Loggers within the :php:`\TYPO3\CMS\Core\Cache` namespace, the configuration is
read from :php:`$TYPO3_CONF_VARS['LOG']['TYPO3']['CMS']['Core']['Cache']`. So every logger requested for classes like
:php:`\TYPO3\CMS\Core\Cache\CacheFactory`, :php:`\TYPO3\CMS\Core\Cache\Backend\NullBackend`, etc.
will get this configuration applied. The same holds for the old pseudo-namespaces with underscore separator which
are still common in extensions.

Configuring Logging for extensions works the same: To treat all extensions differently than the core classes, configuration is
searched for in :php:`$TYPO3_CONF_VARS['LOG']['tx']` (as extension class names start with :php:`tx` or :php:`Tx` and all
class names are converted to lowercase).

.. _Typo3ApiOverview-Logging-Configuration-Writer:

Writer configuration
--------------------

The Log Writer configuration is read from the subkey :php:`writerConfiguration` of the configuration array:

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

To apply a special configuration for classes of the *log_example* extension :php:`tx_logexample`, use the following configuration:

.. code-block:: php

   <?php
   $TYPO3_CONF_VARS['LOG']['tx']['logexample']['writerConfiguration'] = array(
       // configuration for DEBUG severity, including all
       // levels with higher severity (INFO, WARNING, ERROR, ...)
     t3lib_log_Level::DEBUG => array(
         // add a FileWriter
       't3lib_log_writer_File' => array(
           // configuration for the writer
         'logFile' => 'typo3temp/logs/myextension-debug.log'
       )
     ),
       // configuration for WARNING severity, including all
       // levels with higher severity (ERROR, CRITICAL, EMERGENCY)
     t3lib_log_Level::WARNING => array(
         // add a SyslogWriter
       't3lib_log_writer_Syslog' => array(),
     ),
   );

An arbitrary number of writers can be added for every severity level (INFO, WARNING, ERROR, ...). The configuration based on
severity levels is applied to log entries of the particular severity level plus all levels with a higher severity. Thus, a
log messages created with :php:`$logger->warning()` will be affected by a :php:`writerConfiguration` for
:php:`t3lib_log_Level::DEBUG, t3lib_log_Level::INFO, t3lib_log_Level::NOTICE and t3lib_log_Level::WARNING`.
For the above example code that means:
* Calling :php:`$logger->warning($msg);` will result in $msg being written to a file and to syslog.
* Calling :php:`$logger->debug($msg);` will result in $msg being written to a file.

For a list of writers shipped with the TYPO3 Core see the configuration about :ref:`Log Writers <Typo3ApiOverview-Logging-Writers>`.


Processor configuration
-----------------------

According to the writer configuration, log record record processors can be configured on a per-class and per-namespace
basis from the subkey :php:`proessorConfiguration`

.. code-block:: php

   <?php
   $TYPO3_CONF_VARS['LOG']['tx']['logexample']['processorConfiguration'] = array(
       // configuration for ERROR level log entries
     t3lib_log_Level::ERROR => array(
         // add a MemoyUsageProcessor
       't3lib_log_processor_MemoryUsage' => array(
         'formatSize' => FALSE
       )
     )
   );

For a list of processors shipped with the TYPO3 Core see the configuration about :ref:`Log Processors <Typo3ApiOverview-Logging-Processors>`.
