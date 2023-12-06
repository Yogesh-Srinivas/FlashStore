References:

1. https://ali-akhtar.medium.com/mastering-in-core-data-part-0-5a529c6c5a93

---
> Note:
> Misconception:
> 
> Core Data is not an Object Relational Mapper (It’s much more than that)
> Core Data is not an SQL wrapper
> 
> Truth:
> 
> Core Data is a model layer objects framework
> Core Data is also a persistent framework

![CoreData Stack](https://docs-assets.developer.apple.com/published/8fc7c1ecbc/35317515-fd0c-418f-862d-d81efd29ed29.png)

Managed Object Context

> Its primary responsibility is to manage a collection of Managed Objects (Entities). A Managed Object Context maintains the state of entities, typically in memory.
> 
>  It provides Caching, Change tracking, Lazy loading, Redo, Undo and validation features

![Bisection of Stack](https://miro.medium.com/v2/resize:fit:640/format:webp/1*Ufa0bJXh2a6CfBZk-AeBCQ.png)

Persistent Store Coordinator (PSC)

> The persistent store coordinator role is to manage multiple/single stores and present to its managed object contexts the facade of a single unified store as shown below in the figure coordinator communicating to multiple stores.

Persistent store

> You can think of a persistent store as a database file (Sqlite) where individual records each hold the last-saved values of a managed object (Entity).
>
> Core Data also provides an in-memory store that lasts no longer than the lifetime of a process
>
> We can make multiple persistent stores per stack
>
> Core Data offers three native file types for a persistent store: binary, XML, and SQLite. You can implement your own store type if you want Core Data to interoperate with a custom file format or server
