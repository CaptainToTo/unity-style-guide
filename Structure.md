# Structure
These rules describe how code should be created to support extendability, testing, and easier debugging.

<span style="color:yellow">NOTE:</span> **Pushing** refers to changing the state, or triggering a state change of an object from outside that object. **Pulling** refers to requesting the state of an object from outside that object.

## Composition Over Inheritance

Inheritance should only be used for creating sub-class sandboxes, and common interfaces for dependency injection. If the super-class is only providing methods, then an interface should be used instead. Any inheritance tree must be limited to a depth of 2, but preferably will stay at a depth of 1. Interfaces do not count towards this tree depth.

## Dependency Hierarchy

Circular dependencies should be avoided. All classes should be sorted into namespaces with matching assembly definitions to enforce this. Any class that is not sorted into a namespace, and is in the global assembly, should be considered for refactoring.

**To communicate between classes, the following approaches will be used:**
- Classes that represent individual objects should not directly reference their managers. Individual objects may only invoke events, which their manager may subscribe to. Classes can reference managers for other classes, but this must be a non-cyclical dependency (Class A cannot reference Manager B, if Class B references Manager A).
- Classes cannot directly reference any class that depends on it. This should be enforced by assembly definitions. Dependencies can only push to their dependents with events, and callbacks based on a common interface.