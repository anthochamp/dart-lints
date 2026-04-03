# Lints rules

Lint configurations for Dart libraries and Flutter libraries and applications.

## Usage

Add `ac_lints` to your `dev_dependencies` in `pubspec.yaml`:

```yaml
dev_dependencies:
  ac_lints: ^0.5.0
```

Then create an `analysis_options.yaml` at the root of your project including the
appropriate config file for your project type (see below).

## Configuration files

### `dart.yaml` — Dart libraries

For Dart packages published to pub.dev.

```yaml
include: package:ac_lints/dart.yaml
```

Enforces a strict ruleset appropriate for published library code: API documentation
(`package_api_docs`), type annotations on public APIs, const-correctness, and
resource-management rules. All rules include comments explaining their intent.

### `flutter_lib.yaml` — Flutter libraries

For Flutter packages published to pub.dev.

```yaml
include: package:ac_lints/flutter_lib.yaml
```

Extends the same strict standard as `dart.yaml` with additional Flutter-specific rules
(`prefer_if_elements_to_conditional_expressions`, `use_build_context_synchronously`).
Use this for any Flutter package whose code is consumed by other developers.

### `flutter_app.yaml` — Flutter applications

For Flutter application projects (not published as a library).

```yaml
include: package:ac_lints/flutter_app.yaml
```

A lighter ruleset compared to `flutter_lib.yaml`. API documentation, type annotation,
and const-correctness rules are omitted because applications do not expose a public API
to external consumers. Only safety and correctness rules (resource management, error
handling, async correctness) are retained.

## Rule philosophy

These configurations build on [lints/recommended](https://pub.dev/packages/lints) (Dart)
and [flutter_lints/flutter](https://pub.dev/packages/flutter_lints) (Flutter), adding
rules primarily sourced from the [AWS Amplify Flutter monorepo](https://github.com/aws-amplify/amplify-flutter).

The guiding principles are:

- **Library code is held to a stricter standard than application code.** Libraries form
  public APIs that other developers depend on, so documentation, type annotations, and
  const-correctness rules are enforced. Applications are responsible only to their own
  team and do not require the same level of external-facing discipline.
- **Rules target correctness, performance, and safety over style.** Purely stylistic
  rules (e.g. `directives_ordering`, `sort_pub_dependencies`) are disabled to avoid
  friction without meaningful benefit.
- **`package_api_docs` over `public_member_api_docs`.** Documentation is required for
  published packages only. Requiring docs on all public members in non-published code
  (apps, internal tooling) is unnecessarily strict for the projects these configs target.
