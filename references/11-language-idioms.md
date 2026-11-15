# Language Idioms

This derived guidance covers the TypeScript core and web, Rust/Tauri POS, and Flutter manager app in the current plan. Use the package's established formatter, naming conventions, and test runner. [ERP tech stack](../../../project/erp-tech-stack.md)

| Language | Absence and failure | Data and branching | Verification |
| --- | --- | --- | --- |
| TypeScript | Under strict null checks, use optional properties for omission and explicit `null` when the contract gives it meaning. A discriminated union can represent expected outcomes. Translate exceptions at the appropriate boundary. | Use `camelCase` for values and `PascalCase` for types. `readonly` restricts assignment through that type. An exhaustive switch over a discriminated union can keep decisions visible. | Use the package's runner to exercise exported behavior and run the shared JSON cases. |
| Rust | Use `Option<T>` for absence and `Result<T, E>` for recoverable failure. Propagate recoverable errors with `?` when the caller should decide how to handle them. | Bindings are immutable by default. Follow `snake_case` for functions and `UpperCamelCase` for types. Use an exhaustive `match` for closed enums. | Use `cargo test` for unit and integration tests. Replay the shared cases for the logic implemented in Rust. |
| Dart | Use `T?` for legitimate absence and preserve omitted fields when decoding or encoding a contract that distinguishes them from `null`. Handle expected offline outcomes explicitly at the service boundary. | Follow `lowerCamelCase` for values and `UpperCamelCase` for types. `final` fixes a binding. Use immutable values or unmodifiable collections where callers rely on immutability. | Use `flutter test` for manager app logic and widgets. Replay shared fixtures where the app implements the contract or rule. |

References for language features are available in the [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#null-and-undefined), [Rust book](https://doc.rust-lang.org/book/ch09-00-error-handling.html), and [Dart null safety guide](https://dart.dev/null-safety).
