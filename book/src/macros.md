# Mutating code using macros

cargo-mutants will mutate the contents of `#[proc_macro]` functions defined in the current crate, and run tests to see if those mutations are caught.

cargo-mutants does not currently mutate calls to macros, or the expansion of a macro, or the definition of declarative `macro_rules` macros. As a result on code that is mostly produced by macro expansion it may not find many mutation opportunities.

## Modules declared inside macros

`mod foo;` statements inside an item-position macro invocation are followed, so that the files they name are still discovered and mutated. This covers macros such as `cfg_if! { if #[cfg(unix)] { mod unix; } }`, macros that wrap items in a braced body like `scope!(LEVEL { mod foo; })`, and macros that take a module as an argument like `declare!(LEVEL, pub mod foo)`. The macro body itself is still not mutated, and `macro_rules!` definitions are not searched.

Because a macro body is not necessarily made of Rust items, this is a heuristic: a `mod` statement that is assembled by the macro from separate tokens is not found.
