References:

1. https://ali-akhtar.medium.com/mastering-in-core-data-part-0-5a529c6c5a93
2. https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/Performance.html
3. https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Predicates/AdditionalChapters/Introduction.html#//apple_ref/doc/uid/TP40001789

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
>
> **Multiple Context:**
>
> **Importing Data:** If you're importing data from a file or a network request, you can create a separate managed object context to perform this task on a background queue. This allows the main thread to remain responsive and the UI to update smoothly.
> 
> **Batch Operations:** If you're performing batch operations on a large number of objects, you can use a separate managed object context to perform these operations on a background queue. This prevents the main thread from being blocked and the UI from stuttering.
> 
> **Undoable Operations:** If you're performing operations that the user might want to undo, you can use a child managed object context. The child context can discard its changes if the user decides to undo the operation, without affecting the main context.
> 
> **Concurrency:** If you're working with multiple threads, you should create a separate managed object context for each thread. This ensures that each thread has its own managed object context and prevents conflicts between threads.

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

Save

> Save method moved all data from NSManagedObjectContext(memory) to persistent store (disk).
> 
> Now NSManagedObjectContext cached this data. Next time if you want to fetch this data it first check from cache (NSManagedObjectContext) if it found, it will return from cache else it will go to the persistent store which would take more time than fetching from cache.

Perform and Perform and Wait:  [Ref](https://cocoacasts.com/more-core-data-and-concurrency)

> A managed object context should only be accessed from the thread it's associated with.
> 
> If you invoke init(concurrencyType:) and pass in privateQueueConcurrencyType as the concurrency type, Core Data instantiates the managed object context with a private queue. In other words, the managed object context is created on a background thread even though you may have instantiated it on the main thread.
> 
> **perform(_:)**
> **performAndWait(_:)**
> 
> 1. These methods guarantee that the closure you hand it is executed on the queue the managed object context is associated with.
>
> 2. The perform(_:) method schedules the closure and immediately returns control. It doesn't wait for the closure to finish execution.
>
> 3. performAndWait(_:) schedules the closures and returns control when the closure has finished executing.
>
> 4. In other words, performAndWait(_:) executes the closure synchronously whereas perform(_:) executes the closure asynchronously.

  

