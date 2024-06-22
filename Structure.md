# Structure
These rules describe how code should be created to support extendability, testing, and easier debugging.

<span style="color:yellow">NOTE:</span> **Pushing** refers to changing the state, or triggering a state change of an object from outside that object. **Pulling** refers to requesting the state of an object from outside that object.

## Composition Over Inheritance

Inheritance should only be used for creating sub-class sandboxes, and common interfaces for dependency injection. If the super-class is only providing methods, then an interface should be used instead. Any inheritance tree must be limited to a depth of 2, but preferably will stay at a depth of 1. Interfaces do not count towards this tree depth.

## Dependency Hierarchy

Circular dependencies should be avoided. All classes should be sorted into namespaces with matching assembly definitions to enforce this. Any class that is not sorted into a namespace, and is in the global assembly, should be considered for refactoring.

To communicate between classes, the following approaches will be used:
- Classes that represent individual objects should not directly reference their managers. Individual objects may only invoke events, which their manager may subscribe to. 
- Classes can reference managers for other classes, but this must be a non-cyclical dependency (Class A cannot reference Manager B, if Class B references Manager A).
- Classes cannot directly reference any class that depends on it. This should be enforced by assembly definitions. Dependencies can only push to their dependents with events, and callbacks based on a common interface.
- Classes can push and pull on their dependencies freely.


Classes may be allowed to have circular dependencies if they can be described as a "simple-closed system". A simple-closed system must have:
- No more than 3 classes.
- Exist in the same assembly and namespace.
- All classes must share a common manager, or not have a manager at all.
- Cannot have any dependencies beyond Unity, 3rd party libraries, and static utilities.

## Callstack Length

To reduce logic complexity, it's encouraged to keep callstacks small. Callstacks should avoid going more than 10 methods deep from any simulation loop callbacks. Methods that are added to the callstack by frameworks or other 3rd party libraries do not count towards this limit.

Event listeners should not be allowed to invoke other events to prevent events triggering events triggering events.

## Memory Management & Performance

Side effects should be avoided. If a function is producing collections, then a non-alloc version should also exist, which allows for existing collections to be passed in as arguments. 

Reducing the memory footprint of collections should not be a priority when first creating new code. However, making collections easy to swap between should be. This should be done with wrapper methods around the collection interface.

For example:
```
private Dictionary<PlayerId, Player> players;
public Player GetPlayer(PlayerId id)
{
    if (!players.ContainsKey(id)) return null;
    return players[id];
}

vs.

private List<Player> players;
public Player GetPlayer(PlayerId id)
{
    foreach (Player player in players)
    {
        if (player.id == id) return player;
    }
}

vs.

private Player[] players;
public Player GetPlayer(PlayerId id)
{
    int index = id.ToInt();
    if (index < 0 || players.Length <= index) 
        return null;
    return players[index];
}
```

Any class that is planning on exposing a collection should have these wrapper methods so that changing the collection type during refactoring is easier.

Beyond collections, memory usage and performance optimizations in general should be saved for refactors. On first writes, developers should be focused on creating readable and extendable code.

## Network Code

Network code must be held in a separate class from the simulation logic, which the simulation logic will depend on. The goal is to make all simulation code runnable without needing any multiplayer setup.