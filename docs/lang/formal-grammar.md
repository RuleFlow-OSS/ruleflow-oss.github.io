---
icon: lucide/whole-word
---

# Formal Grammar

A [formal grammar](https://en.wikipedia.org/wiki/Formal_grammar) is a set of structural rules that defines how to form valid statements in a specific language. It acts as the blueprint for the [language parser](https://en.wikipedia.org/wiki/Parsing), specifying the exact mathematical arrangement of [tokens](https://en.wikipedia.org/wiki/Lexical_analysis), keywords, and [expressions](https://en.wikipedia.org/wiki/Expression_(computer_science)) required for the interpreter to understand and execute the code.

In FlowLang, the grammar dictates how transformation rules, global directives, and modifiers combine to define universe evolutions. Below is a streamlined, human-readable [Extended Backus-Naur Form (EBNF)](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form) representation of the FlowLang syntax.

```ebnf
// ==========================================
// Top-Level Entry Point
// ==========================================
program ::= (global_flags | block | instruction_sequence | directive)*

// ==========================================
// High-Level Constructs (Statements)
// ==========================================
directive            ::= "@" <expression> ";"
global_flags         ::= flags
block                ::= "(" flags ")" "(" instruction_sequence* ")"
instruction_sequence ::= (instruction ";")+

// ==========================================
// Instructions & Expressions
// ==========================================
instruction ::= selector* operator target* [flags]

selector ::= regex_term 
           | range_term 
           | literal_term 
           | literal_chars_term 
           | fn_term

target   ::= literal_term 
           | literal_chars_term 
           | fn_term

// Term Definitions
fn_term            ::= SIMPLE_LITERAL "<" <arguments> ">"
literal_term       ::= "(" <text> ")"
literal_chars_term ::= SIMPLE_LITERAL
regex_term         ::= STRING_LITERAL
range_term         ::= "[" [<number> | "inf" | "-inf"] ["," [<number> | "inf" | "-inf"]] "]"

// Operators
operator ::= "-->"  // Overwrite
           | "><"   // Delete
           | "->"   // Substitute
           | ">"    // Insert

// Flags
flags ::= flag+
flag  ::= "-" <identifier> ["[" <arguments> "]"]

// ==========================================
// Lexer Terminals (Tokens)
// ==========================================
STRING_LITERAL ::= '"' <text> '"'
SIMPLE_LITERAL ::= [a-zA-Z0-9_.*]+
```

## Example: Pushing the Grammar to its Limits

To see how flexible and permissive these formal rules are, consider the following script. It showcases the absolute limits of the FlowLang syntax:

```python
# Directives (Includes a macro that will unpack)
@macro("/lang/ca.preset");
@example_directive1(1, "12", (2,2), k=2);
@example_directive_thats_really_an_exposed_object.all()[0].yup(1, 2, 3);

# Global Flags (applied to all instructions)
-example_flag[slice(1, 1, 1)]
-globalDict[{"key": "value"}]

# Block with grouped flags (-a and -j[2] are appled to all instructions in the block)
(-a -j[2])(
    # Multiple mixed selectors and targets
    "AAB" AB -> ABA CBA (1, 2);

    # Function term selector and Literal target with nested tuple structures
    fn<1, 2> -> (1, 2, 3, "A") -ttt;

    # Ranges with open ends, infinities, and empty brackets
    [-inf, inf] --> AB;
    [, 5] > AB;
    [5, ] >< ;
    [] -> ();

    # Literal term with negative integers
    (1, 2, -3, 4) --> AB;
)

# Standalone instruction outside the block
(1, 2) ->;

# This is an example of a single line comment!
"""
This is an example of multiline comment.
It is a cool feature!
"""
```


### Why This Example is Valid

This script maps just fine to the `program` root rule (although it doesn't mean anything unless specified in the interpreter implementation; this is simply a [well-formed](https://en.wikipedia.org/wiki/Well-formed_formula) example) and demonstrates the flexibility of FlowLang's parser design.

* **Directives as Python Expressions:** The parser captures the expression inside the `@...;` directive as a raw string and evaluates it natively in Python using `eval()`. This is why complex, chained object calls like `@example_directive_thats_really_an_exposed_object.all()[0].yup(1, 2, 3);` are perfectly valid and easily handled by the underlying execution scope.
* **Dynamic Flag Arguments:** Similar to directives, the arguments passed to flags (inside the `[...]`) are evaluated as Python expressions. This natively allows passing complex Python objects like `slice(1, 1, 1)` or dictionaries like `{"key": "value"}` directly into the rule's state.
* **Loose Arity in Instructions:** The `instruction` grammar explicitly allows zero or more selectors (`selector*`) and zero or more targets (`target*`). This makes syntactically strange but functionally valid instructions like `(1, 2) ->;` (which substitutes a literal term with nothing) entirely permissible (of course the actual behavior depends on the implementation).
* **Mixed Sequences:** A single instruction can seamlessly chain different token types without issue. `"AAB" AB` marries a `regex_term` and a `literal_chars_term` on the left side of the operator, while `ABA CBA (1, 2)` marries two `literal_chars_term` tokens with a `literal_term` on the right side.
* **Boundary-Free Ranges:** The `range_term` syntax natively supports missing elements, empty boundaries, and infinity keywords. The parser safely interprets partial strings like `[, 5]` as `0` to `5`, and empty brackets `[]` as an empty tuple.
