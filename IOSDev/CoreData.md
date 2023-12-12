References:

1. https://ali-akhtar.medium.com/mastering-in-core-data-part-0-5a529c6c5a93
2. https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/Performance.html
3. https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Predicates/AdditionalChapters/Introduction.html#//apple_ref/doc/uid/TP40001789

---
> Note:
> Misconception:
> 
> - Core Data is not an Object Relational Mapper (It’s much more than that)
> - Core Data is not an SQL wrapper
> 
> Truth:
> 
> - Core Data is a model layer objects framework
> - Core Data is also a persistent framework

![CoreData Stack](https://docs-assets.developer.apple.com/published/8fc7c1ecbc/35317515-fd0c-418f-862d-d81efd29ed29.png)

Managed Object Context

>  - Its primary responsibility is to manage a collection of Managed Objects (Entities). A Managed Object Context maintains the state of entities, typically in memory.
> 
>  - It provides Caching, Change tracking, Lazy loading, Redo, Undo and validation features
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

> - The persistent store coordinator role is to manage multiple/single stores and present to its managed object contexts the facade of a single unified store as shown below in the figure coordinator communicating to multiple stores.

Persistent store

> - You can think of a persistent store as a database file (Sqlite) where individual records each hold the last-saved values of a managed object (Entity).
>
> - Core Data also provides an in-memory store that lasts no longer than the lifetime of a process
>
> - We can make multiple persistent stores per stack
>
> - Core Data offers three native file types for a persistent store: binary, XML, and SQLite. You can implement your own store type if you want Core Data to interoperate with a custom file format or server

Save

> - Save method moved all data from NSManagedObjectContext(memory) to persistent store (disk).
> 
> - Now NSManagedObjectContext cached this data. Next time if you want to fetch this data it first check from cache (NSManagedObjectContext) if it found, it will return from cache else it will go to the persistent store which would take more time than fetching from cache.

Perform and Perform and Wait:  [Ref](https://cocoacasts.com/more-core-data-and-concurrency)

> - A managed object context should only be accessed from the thread it's associated with.
> 
> - If you invoke init(concurrencyType:) and pass in privateQueueConcurrencyType as the concurrency type, Core Data instantiates the managed object context with a private queue. In other words, the managed object context is created on a background thread even though you may have instantiated it on the main thread.
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

Relationship Between Entities in Core Data:  [Ref](https://ali-akhtar.medium.com/mastering-in-coredata-part-5-relationship-between-entities-in-core-data-b8fea1b50efb)

> **Destination entity** → Destination Entity name you want to create relationship with
>
> **Cardinality** → whether a relationship is One-To-One, Many-To-One
> 
> **Optional** → relationship can be NULL / NOT NULL
> 
> **Transient** → In memory usage
> 
> **Limit** → If it’s a to-many, are there maximum or minimum numbers of objects that can be in the relationship? (The lower limit does not have to be zero)
> 
> **Direction** → Most object relationships are inherently bidirectional. If a department has a To — Many relationships to the Employees who work in a Department, there is an inverse relationship from an Employee to the Department that is To-One . **The recommended approach is to model relationships in both directions and specify the inverse relationship appropriately.** Core Data uses this information to ensure the consistency of the object graph if a change is made
> 
> **Delete Rule** → A relationship’s delete rule specifies what should happen on the destination Entity if an attempt is made to delete the source object.
> 1. **Deny** → If there is at least one object at the relationship destination (employees), do not delete the source object (department).
>
> 2. **Nullify** → Remove the relationship between the objects, but do not delete either object.
>
> 3. **Cascade** → Delete the objects at the destination of the relationship when you delete the source.
>
> 4. **No Action** → Do nothing to the object at the destination of the relationship.

CoreData Validations

> - The validation constraints are applied by Core Data **only during a save operation or upon request.** (you can invoke the validation methods directly at any time it makes sense for your application flow)
>
> - You can request validation before calling save method throw validateForInsert(), validateForUpdate() instance method on Entity itself and you can override this method to apply custom validation with your code.
>

Fetch Request

![Fetch Request](https://miro.medium.com/v2/resize:fit:720/format:webp/1*V5MJAfRJtODRyu52-RK-Rg.png)

> A fetch request is an instance of NSFetchRequest. The entity it specifies is represented by an instance of NSEntityDescription; any constraints are represented by an NSPredicate object, and the ordering by an array of one or more instances of NSSortDescriptor. These are akin to the table name, WHERE clause, and ORDER BY clauses of a database SELECT statement respectively.
> 
> **fetch(_:)** returns an array of managed objects meeting the criteria specified by the fetch request. Method has two possible results. It either returns an NSArray object of type NSManagedObject with zero or more objects, or it throw an error , you have received an error from Core Data and need to respond to it.
> 
> **NSPredicate** object is used to fetch request to narrow/filtered the number of objects being returned. For example, if you only want User objects that have a firstName = ali, you add the predicate directly to NSFetchRequest.
>
> **NSSortDescriptor** object is used to order/sort a collection of objects fetched by the NSFetchRequest based on a property common to all the objects. For example, if you want all User objects sorted by firstName in an ascending order , you add the NSSortDescriptor instance directly to NSFetchRequest
>
> **Optimizations**
>  - As we know, We can make multiple persistent stores per stack. The fetch request can be modified to search particular stores using the setAffectedStores: method on an NSFetchRequest.
>  - When you’re creating an object, you can assign the entity to a particular store using the assignObject:toPersisentStore: method on NSManagedObjectContext.
>    
> **Result type of the Fetch Request** [Ref](https://ali-akhtar.medium.com/mastering-in-coredata-part-9-nsfetchrequest-d9ad991355d9)
> 
> NSFetchRequest always return Array but the type of array is decided by the result type. There are currently four result type supported by NSFetchrequest which are listed below
> - managedObjectResultType (Default )
> - dictionaryResultType. (Return array of dictionary)
> - countResultType. (Return only count )
> - managedObjectIDResultType (Return only objectIds )
>   **Note:**
>
>   The question that might be arised in your mind what is NSManagedObjectID we will look into this when we will be doing multiple managedObjectContext or in threading part as well . It’s not very helpful when we have only one NSManagedObjectContext. At this moment if you don’t understand anything about it it’s OK but one think you should take is that we can return NSMangedObjectId in fetch request as well by changing it’s resultType to NSManagedObjectID. It will surely optimize the performance when we need to transfer data between two contexts.

  

