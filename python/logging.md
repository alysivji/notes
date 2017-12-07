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


