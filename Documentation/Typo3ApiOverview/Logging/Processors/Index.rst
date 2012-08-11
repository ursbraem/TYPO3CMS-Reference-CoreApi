.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../../Includes.txt

.. _Typo3ApiOverview-Logging-Processors:

Log Record Processors
=====================

The purpose of a log processor is (usually) to modify a log record or add more detailed information to it.

Log processors allow to manipulate log records without changing the code where the log method actually is called (inversion of control). This enables you to add any information from outside the scope of the actual calling function, for example webserver environment variables. The TYPO3 core ships some basic log processors, but more can be added with extensions.


Built-in Log Processors
--------------------

This section describes the log processors shipped with the TYPO3 core. Some processors have options to allow customization of the particular processor. See the :ref:`Configuration <Typo3ApiOverview-Logging-Configuration-Processor>` section, how to use these options.

IntrospectionProcessor
^^^^^^^^^^^^^^^^^^^^^^

The introspection processor adds backtrace data about where the log event was triggered:

file
  absolute path to the file.
line
  line number.
class
  class name.
function
  function name.


MemoryUsageProcessor
^^^^^^^^^^^^^^^^^^^^

The memory usage processor adds the amount of used memory to the log record:

memoryUsage
  Result from memory_get_usage().

================  ==========  ===========================================================================  =============================
Option            Mandatory   Description                                                                  Default
================  ==========  ===========================================================================  =============================
realMemoryUsage   no          Use real_ size of memory allocated from system instead of emalloc() value.   ``TRUE``
================  ==========  ===========================================================================  =============================

.._real: http://php.net/manual/de/function.memory-get-usage.php


MemoryPeakUsageProcessor
^^^^^^^^^^^^^^^^^^^^^^^^

The memory peak usage processor adds the peak amount of used memory to the log record:

memoryPeakUsage
  Result from memory_get_peak_usage().

================  ==========  ===========================================================================  =============================
Option            Mandatory   Description                                                                  Default
================  ==========  ===========================================================================  =============================
realMemoryUsage   no          Use real_ size of memory allocated from system instead of emalloc() value.   ``TRUE``
================  ==========  ===========================================================================  =============================

.._real: http://php.net/manual/de/function.memory-get-peak-usage.php


WebProcessor
^^^^^^^^^^^^

The web processor adds selected webserver environment variables to the log record:

_ARRAY
  All possible values from :php:`t3lib_div::getIndpEnv('_ARRAY')`:
  HTTP_HOST, TYPO3_HOST_ONLY, TYPO3_PORT, PATH_INFO, QUERY_STRING, REQUEST_URI, HTTP_REFERER, TYPO3_REQUEST_HOST, TYPO3_REQUEST_URL, TYPO3_REQUEST_SCRIPT, TYPO3_REQUEST_DIR, TYPO3_SITE_URL, TYPO3_SITE_SCRIPT, TYPO3_SSL, TYPO3_REV_PROXY, SCRIPT_NAME, TYPO3_DOCUMENT_ROOT, SCRIPT_FILENAME, REMOTE_ADDR, REMOTE_HOST, HTTP_USER_AGENT, HTTP_ACCEPT_LANGUAGE.

Custom Log Processors
---------------------

Custom log processors can be added through extensions. Every log processor has to implement the interface :php:`t3lib_log_processor_Processor`.
It is suggested to extend the abstract class :php:`t3lib_log_processor_Abstract` which allows you use configuration options by adding the corresponding properties and setter methods.

Please keep in mind that TYPO3 will silently continue operating, in case a log processor is throwing an exception while executing the :php:`processLogRecord()` method.