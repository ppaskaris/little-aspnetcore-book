## Create models
There are two separate model classes that need to be created: a model that represents a to-do item stored in the database (sometimes called an **entity**), and the model that will be combined with a view (the **MV** in MVC) and sent back to the user's browser. Because both of them can be referred to as "models", I'll refer to the latter as a *view model*.

First, create a class called `TodoItem` in the Models directory:

**`Models/TodoItem.cs`**

```csharp
using System;

namespace AspNetCoreTodo.Models
{
    public class TodoItem
    {
        public Guid Id { get; set; }
        
        public bool IsDone { get; set; }

        public string Title { get; set; }

        public DateTimeOffset? DueAt { get; set; }
    }
}
```

This class defines what the database will need to store for each to-do item: an ID, a title or name, whether the item is complete, and what the due date is. Each line defines a property of the class: its name, its type (boolean, string, guid), and getter/setter methods (which makes the property read/write).

> A **g**lobally **u**nique **id**entifier (guid or GUID) is a long string of letters and numbers, like `43ec09f2-7f70-4f4b-9559-65011d5781bb`. Because they are random and are extremely unlikely to ever be accidentally duplicated, they are commonly used for things like IDs.

> You can also use a number as a database entity ID, but you'd need to configure your database to always increment the number when new rows are added to the database.

At this point, it doesn't matter what the underlying database technology is. It could be SQL Server, MySQL, MongoDB, Redis, or something more exotic. This model defines what the database row or entry will look like in C# code so you don't have to worry about the low-level database stuff. It's sometimes called a "plain ol' C# object" or POCO.

Often, the model (entity) you store in the database is similar but not *exactly* the same as the model you want to use in MVC (the view model). In this case, the `TodoItem` model represents a single item in the database, but the view might need to display two, ten, or a hundred to-do items (depending on how badly the user is procrastinating).

Because of this, the view model should be a class that holds an array of `TodoItem`s:

**`Models/TodoViewModel.cs`**

```csharp
using System.Collections.Generic;

namespace AspNetCoreTodo.Models
{
    public class TodoViewModel
    {
        public IEnumerable<TodoItem> Items { get; set; }
    }
}
```

`IEnumerable<>` is a fancy C# way of saying that the `Items` property contains zero, one, or many `TodoItem`s. (In technical terms, it's not quite an array, but rather an array-like interface for any sequence that can be enumerated or iterated over.)

> The `IEnumerable<>` interface exists in the `System.Collections.Generic` namespace, so you need a `using` statement at the top of the file.

Now that you have some models, it's time to create a view that will take a `TodoViewModel` and render the right HTML to show the user their to-do list.
