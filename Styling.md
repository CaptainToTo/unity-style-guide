# Styling
These rules do not affect the control flow, or functionality of code. These only serve to make code more consistent and readable.

<span style="color:yellow">NOTE:</span> The fundamental logic control structure in C# is the **method**. Methods that don't have a return value (void) are **procedures**. Methods that do have a return value are **functions**.

## Whitespace

Tabs are equivalent to 4 spaces. All indents should be done with space characters, and not tab characters. 

Placing whitespace between sections of code is recommended for better readability.

## Brackets

Brackets start on the next line. If-else statements, loops, getters, setters, and methods may be written in 1 line, or without brackets if the line of code is sufficiently trivial.

<span style="color:green">OK:</span>
```
if (this == that) return true;

if (this == that)
{
    return true;
}

if (this == that)
    return true;

public bool IsHoldingBall { get { return heldBall != null; } }
```

<span style="color:red">Bad:</span>
```
if (this == that) Physics.Raycast(start, direction, distance) ? return distance : return 0;

if (this == that)
    Physics.Raycast(start, direction, distance) ? return distance : return 0;
```

## Locality of Logic

To make traversing code easier, methods and properties that are related to one another should be placed nearby or next to each other within the file.

It is encouraged to visually group related methods and properties together using "section comments". Groups should be ordered as:

1. public properties
2. private properties
3. public getters & setters
3. public procedures
4. public functions
5. private procedures
6. private functions

Methods should be sorted by call order, from top to bottom. For example, if method A calls method B, then method A should appear before method B in the file.

If a file isn't "owned" by one developer, then each section should have a designated owner. More about ownership in the [process](Process.md) section.

For example:
```
// # Held Ball ================================
// * atumemot

private Ball heldBall = null;

/// <summary>
/// Returns true if the player is holding a ball.
/// </summary>
public bool IsHoldingBall { get { return heldBall != null; } }

/// <summary>
/// Function signature for any event listener subscribed to player ball interactions.
/// </summary>
public delegate void BallEvent(Ball ball);

// # ==========================================

// # Ball Pickup ==============================
// * tomemuta

/// <summary>
/// Invoked when this player picks up a ball.
/// Provides the ball that was picked.
/// </summary>
public event BallEvent OnBallPickup;

/// <summary>
/// Sets this player's held ball.
/// </summary>
public void PickupBall(Ball ball)
{
    ApplyBallPickup(ball);
    OnBallPickup?.Invoke(heldBall);
}

// handle state changes
private void ApplyBallPickup(Ball ball)
{
    heldBall = ball;
    heldBall.transform.SetParent(transform);
    heldBall.transform.localPosition = Vector3.zero;
}

// # ==========================================
```

## Naming Conventions

All names will be in either <span style="color:green">camelCase</span> or <span style="color:yellow">PascalCase</span>. Some names will also require a <span style="color:red">Prefix</span>.

- Local variables : <span style="color:green">camelCase</span>
- Properties : <span style="color:green">camelCase</span>
- Properties with implemented get/set : <span style="color:yellow">PascalCase</span>
- Methods : <span style="color:yellow">PascalCase</span>
- Remote Procedure Calls : <span style="color:red">Prefix: "RPC_"</span> , <span style="color:yellow">PascalCase</span>
- Enums and enum values : <span style="color:yellow">PascalCase</span>
- Classes : <span style="color:yellow">PascalCase</span>
- Structs : <span style="color:yellow">PascalCase</span>
- Interfaces : <span style="color:yellow">PascalCase</span>
- Generics : <span style="color:yellow">PascalCase</span>
- Delegates : <span style="color:yellow">PascalCase</span>
- Events (both Unity & C#) : <span style="color:red">Prefix: "On"</span> , <span style="color:yellow">PascalCase</span>
- Namespaces : <span style="color:yellow">PascalCase</span>
- Assemblies : <span style="color:yellow">PascalCase</span>
- Files & Folders : <span style="color:yellow">PascalCase</span>

Variable names should NOT be single characters unless it is obvious what their purpose is, such 
as using `i` for the iterator in a for-loop.

Assembly definitions should ALWAYS have the same name as the namespace it's encompassing.

## Code Comments

All **public**, **protected**, and **static** methods and properties MUST have a `<summary>` comment explaining its purpose. This comment should explain how arguments are used, and what the return value represents if applicable.

**Private** methods and properties should have a code comment explaining its purpose, but this doesn't need to be a `<summary>` comment.

Code should be commented in a such a way that you could rewrite the code using only the comments as notes. This generally means that each "step" should be given a comment that says what it does. A lack of comments is equally discouraged as over-commenting.

<span style="color:green">Adequate commenting:</span>
```
/// <summary>
/// Returns true if the given BST contains num as one of its nodes.
/// </summary>
public bool ContainsNum(BST tree, int num)
{
    // null guard
    if (BST == null) return false;

    // binary search for num
    Node cur = tree.root;
    while (cur != null)
    {
        if (cur.value == num)
        {
            return true;
        }
        else if (cur.value < num)
        {
            cur = cur.right;
        }
        else
        {
            cur = cur.left;
        }
    }

    // num not found
    return false;
}
```

<span style="color:red">Excessive commenting:</span>
```
/// <summary>
/// Returns true if the given BST contains num as one of its nodes.
/// </summary>
public bool ContainsNum(BST tree, int num)
{
    // null guard, return false if tree doesn't exist
    if (BST == null) return false;

    // binary search for num
    Node cur = tree.root; // current node being searched on
    // continue binary search while the bottom of the tree hasn't been reached
    while (cur != null)
    {
        // return true if num was found
        if (cur.value == num)
        {
            return true;
        }
        // move to children based on the current node's value
        else if (cur.value < num)
        {
            cur = cur.right;
        }
        else
        {
            cur = cur.left;
        }
    }

    // num not found
    return false;
}
```

Comments can be given prefix characters to provide immediate information about their purpose. Editor extensions can be used to color code these comments as well.

Prefixes:
- `?` : "Need-to-know" quirks about how a piece of code works.
- `#` : Section titles, comments meant to visually separate sections of code.
- `!` : Known, but low priority bugs, such as an unhandled edge-case that's not expected to ever happen.
- `*` : File and section owner names.
- `TODO:` : Tasks that need to be completed. All developers should have a way to easily query the code base for `TODO`s in their editor.
