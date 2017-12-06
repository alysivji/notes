# [The Twelve-Factor App](https://12factor.net/)

## I. [Codebase](https://12factor.net/codebase)

> One codebase tracked in revision control, many deploys

There is a one-to-one correlation between the codebase and the app.

## II. [Dependencies](https://12factor.net/dependencies)

> Explicitly declare and isolate dependencies

* Explicitly declare dependencies, don't rely on the implicit existence of site-packages; `pip`
* Isolate dependencies from the rest of the system; `virtualenv`
* Do not rely on the implicit existence of any system tools; might not exist on _all_ systems

## III. [Config](https://12factor.net/config)

> Store config in the environment

* __config__ is everything that can vary between different environments
    * Resource handles to the database
    * Credentials to external services
    * Hostname for deploy
* strict separation of config from code
* can we open source the code and not compromise credentials?
* use config files that are not checked into version control
* try to store config in environment variables

## IV. [Backing Services](https://12factor.net/backing-services)

> Treat backing services as attached resources

* __backing service__ is any service the app consumers over the networks
    * datastores
    * messaging / queueing systems
    * SMTP services for outbound email
    * caching systems
* should not make any distinction between local and third party services
* should be able to swap out local MySQL database with one production version without changing any code
    * should only have to change the config
* resources can be attached to and detached from deploys at will; code / infrastructure should be able to handle

## V. [Build, release, run](https://12factor.net/build-release-run)

> Strictly separate build and run stages

* Three stages of deployment:
    * _build stage_ converts code repo into executable bundle (aka build)
    * _release stage_ takes build and combines with correct config; package is called release
    * _run stage_ run release in the execution environment by launching process
* strict sepration between build, release, and run stages
* every release should have a unique release ID so we can rollback if needed
* build should be initiated everytime new code is deployed

## VI. [Processes](https://12factor.net/processes)

> Execute the app as one or more stateless processes

* App will be executed in the execution environment as one or more processes
* Processes should be stateless and share nothing between instaces unless it uses a stateful backing service

## VII. [Port Binding](https://12factor.net/port-binding)

> Export services via port binding

* App is self-contained and does not require the injection of a webserver in order to create a service
* Web app exports HTTP as a service by binding it to a port and listening to requests coming in on that port

## VIII. [Concurrency](https://12factor.net/concurrency)

> Scale out via the process model

* web apps usually have more than one process; server and worker (queue)
* processes should be first class citizens
* app must be able to span multiple processes running on multiple physical machines
    * makes it easier to scale out (aka scale horizontally)
* need to reply on OS process manager to manage output streams and handling sysadmin work (restarts, shutdowns)

### Aside: [Unix Process Model](https://adam.herokuapp.com/past/2011/5/9/applying_the_unix_process_model_to_web_apps/)

* Running program is a __process__
    * can be started manually
    * run automatically; _managed process_
* __Process manager__ can put processes under management

## IX. [Disposability](https://12factor.net/disposability)

> Maximize robustness with fast startup and graceful shutdown

* Processes are _disposable_ and they can be started or stopped anytime
* Should try to minimize startup time
* Terminate gracefully when receiving a __`SIGTERM`__ from the process manage
* Web apps should:
    * stop listening on service port
    * allow current requests to finish
    * exit
* Worker processes should:
    * return current job to the work queue
    * jobs should be [reentrant](https://en.wikipedia.org/wiki/Reentrancy_(computing))
        * function can stop during execution and restart without affect results
* processes should be robust against suddent death

## X. [Development / Production Parity](https://12factor.net/dev-prod-parity)

> Keep development, staging, and production as similar as possible

* Gaps between development and production can be seen in three areas:
    1. Time Gap -- a developer may work on code that takes days or longer to go into production
    1. Personnel Gap - devs write code, ops deploy
    1. Tools Gap - differences in development and production stacks
* Continuous deployment requires a small gap between development and production enviornments
* Try to use the same backing service across environments

## XI. [Logs](https://12factor.net/logs)

> Treat logs as event streams

* Logs provide visibility into the behavior of a running application; written to a file or streamed somewhere
* App should not concern itself with routing or storage of output stream
    * Should event stream to `stdout` where it should be captured by the execution environment

## XII. [Admin Process](https://12factor.net/admin-processes)

> Run admin/management tasks as one-off processes

* Sometimes we need to do one-off administration or maintenance tasks
    * Ensure these processes are run in an identical environment as the app
* Admin code must ship with application code to avoid sychronization issues
