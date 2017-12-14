# Python Logging

## [Basic Logging Tutorial](https://docs.python.org/3/howto/logging.html)

* We log contextual data and assign it a level
* This helps us debug the program if certain events occur

|Level|When it's used|
|---|---|
|`DEBUG`|Detailed information, typically of interest only when diagnosing problems.|
|`INFO`|Confirmation that things are working as expected.|
|`WARNING`|An indication that something unexpected happened, or indicative of some problem in the near future (e.g. ‘disk space low’). The software is still working as expected.|
|`ERROR`|Due to a more serious problem, the software has not been able to perform some function.|
|`CRITICAL`|A serious error, indicating that the program itself may be unable to continue running.|

Default level is `WARNING` so all errors this level and above will be tracked

### Basic Config

```python
import logging

logging.basicConfig(
    filename='my_log.log',
    level=logging.DEBUG,
    format='',
    datefmt='',
)
```

* Can also set logging level via a command-line option `--log=LEVEL`
    * Access via `getattr(logging, loglevel.upper())`

---

## [Advanced Logging Tutorial](https://docs.python.org/3/howto/logging.html#logging-advanced-tutorial)

### Logging from Mutiple Modules

> * __Loggers__ expose the interface that application code directly uses.
> * __Handlers__ send the log records (created by loggers) to the appropriate destination.
> * __Filters__ provide a finer grained facility for determining which log records to output.
> * __Formatters__ specify the layout of log records in the final output.

We pass information between loggers, handlers, and formatters in a `LogRecord` instance, which is created automatically by the `Logger` each time something is logged

* Loggers perform the actual logging
    * module-level logger: `logger = logging.getLogger(__name__)`
* Can save logs to different destinations using the handler
    * default is none
    * if no location is set, will go to `stderr`

#### Logging Process Flow

<img src="https://docs.python.org/3/_images/logging_flow.png" />

#### [Loggers](https://docs.python.org/3/howto/logging.html#loggers)

* expose methods that application can use to log
* determine which log messages to act on based upon severity
    * [logging level](https://docs.python.org/3/howto/logging.html#logging-levels)
    * default is `WARNING`
* pass relevant log messages to log
* configure handlers for top level logger and create child loggers as needed

* `.setLevel()` specifies the lowest severity message logger will handle
* `.addHandler()` / `.removeHandler()` to add/remove handler objects
* `.addFilter()` / `.removeFilter()` to add/remove filter objects

#### [Handlers](https://docs.python.org/3/howto/logging.html#handlers)

* dispatch log message to handler's specified destination
* can have multiple handlers
* [useful handlers](https://docs.python.org/3/howto/logging.html#useful-handlers)

#### [Formatters](https://docs.python.org/3/howto/logging.html#formatters)

* configure how the log message looks

`logging.Formatter.__init__(fmt=None, datefmt=None, style='%')`

#### [Configuring Logging](https://docs.python.org/3/howto/logging.html#configuring-logging)

> 1. Creating loggers, handlers, and formatters explicitly using Python code that calls the configuration methods listed above.
> 1. Creating a logging config file and reading it using the `fileConfig()` function.
> 1. Creating a dictionary of configuration information and passing it to the `dictConfig()` function.

#### [Optimization](https://docs.python.org/3/howto/logging.html#optimization)

* `logger.isEnabledFor(logging.LEVEL)` is a useful check to make sure we don't have to do expensive strip interpolations

---

## [Logging Cookbook](https://docs.python.org/3/howto/logging-cookbook.html)

* Lots of great recipes
* [Adding Contextual Information to Logging Output](https://docs.python.org/3/howto/logging-cookbook.html#adding-contextual-information-to-your-logging-output)

---

## [`logging`](https://docs.python.org/3/library/logging.html#module-logging)

* [`LogRecord`](https://docs.python.org/3/library/logging.html#logrecord-objects)
* [Configuration dictionary schema](https://docs.python.org/3/library/logging.config.html#configuration-dictionary-schema)
* [Handlers](https://docs.python.org/3/library/logging.handlers.html)

---

## [PEP 282 -- A Logging System](https://www.python.org/dev/peps/pep-0282/)

* A registery of [singleton](https://en.wikipedia.org/wiki/Singleton_pattern) logger object holds references to all initialized loggers

### Control Flow

> Applications make logging calls on Logger objects. Loggers are organized in a hierarchical namespace and child Loggers inherit some logging properties from their parents in the namespace.
>
> These Logger objects create __LogRecord__ objects which are passed to __Handler__ objects for output. Both Loggers and Handlers may use logging __levels__ and (optionally) __Filters__ to decide if they are interested in a particular LogRecord. When it is necessary to output a LogRecord externally, a Handler can (optionally) use a __Formatter__ to localize and format the message before sending it to an I/O stream.
>
> The main benefit of a logging system like this is that one can control how much and what logging output one gets from an application without changing that application's source code. Therefore, although configuration can be performed through the logging API, it must also be possible to change the logging configuration without changing an application at all. For long-running programs like Zope, it should be possible to change the logging configuration while the program is running.

Does this go against the principles of 12 factor?

> The most simple configuration is that of a single handler, writing to stderr, attached to the root logger. This configuration is set up by calling the basicConfig() function once the logging module has been imported.
>
> To support use of the logging mechanism in short scripts and small applications, module-level functions debug(), info(), warn(), error(), critical() and exception() are provided. These work in the same way as the correspondingly named methods of Logger - in fact they delegate to the corresponding methods on the root logger. A further convenience provided by these functions is that if no configuration has been done, basicConfig() is automatically called.

* At application exit we can flush all hanlers using `shutdown()`
