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
