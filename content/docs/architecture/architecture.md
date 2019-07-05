# Architecture
Totoval has two separated parts, **Totoval** repo and **Totoval Framework** repo.

## Totoval
Totoval repo is the main repo which defines the directory mapping of Totoval. It has **7** parts currently.

* app  
    The **app** is a directory which contains all the component that your project will use.
    * http  
        * [controllers]({{< relref "/docs/basics/controllers.md" >}})
        * [middleware]({{< relref "/docs/basics/middleware.md" >}})
        * [requests]({{< relref "/docs/basics/requests.md" >}})
    * [models]({{< relref "/docs/orm/models.md" >}})
    * [cache]({{< relref "/docs/deeper/cache.md" >}})
    * [jobs]({{< relref "/docs/deeper/jobs.md" >}})
    * [events]({{< relref "/docs/deeper/events.md" >}})
    * [listeners]({{< relref "/docs/deeper/events.md" >}})
    * [policies]({{< relref "/docs/security/authorization.md" >}})
    * console
        * [commands]({{< relref "/docs/deeper/artisan_console.md" >}})  
        * [schedule]({{< relref "/docs/deeper/scheduler.md" >}})
* bootstrap  
    The **bootstrap** is a directory contains the packages that should be initialized at Totoval app startup.
* config  
    The **config** is a directory for placing configurations in which Totoval app will use.
* database  
    The **database** is a directory for the database interacting things.
    * [migrations]({{< relref "/docs/database/migrations.md" >}}) 
    * seeds //TODO
* resources  
    The **resources** is a directory contains Totoval's language packages and views, etc.
    * [lang]({{< relref "/docs/frontend/localization.md" >}})
    * [views]({{< relref "/docs/frontend/views.md" >}})
* routes  
    The **routes** is a directory which defines Totoval's request routing.
    * [versions]({{< relref "/docs/basics/routing.md" >}})
    * [groups]({{< relref "/docs/basics/routing.md" >}})
* environment file  
    The **environment file** is typically refers to a file named `.env.json`, which you could defined a set of values in **json** format and it will rewrite the values set at [config]({{< relref "/docs/getting_started/configuration.md" >}}) files
* entrypoint  
    * main.go
        **main.go** is the entrypoint of starting the web application, and it will serve the http server binding the port you've configured.
    * artisan.go
        **artisan.go** is the entrypoint of Totoval's command line, you could use this entrypoint for all the command line interactions defined in Totoval.

## Framework
Totoval Framework repo is a set of utils that Totoval would use, you only need to import it at **go.mod** file in Totoval main repo