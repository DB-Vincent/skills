---
name: document-code
description: Use when the user wants to document code (Go, TypeScript, Python, Terraform) or write a user guide for it.
---

# Document code

Add doc comments to code, or write a user guide. Match the style of the codebase; an existing convention (docstring style, JSDoc, README layout) overrides this skill.

## Workflow

1. Detect the language and any existing doc style in neighboring code.
2. Identify what to document: public/exported APIs, non-obvious behavior, error cases.
3. Write the docs.
4. Validate with the language command below.
5. Report what was documented and what was skipped.

Each documented function must state what it does, every argument (name, type, description), and what it returns, including the errors it can return.

Document why and edge cases, not what the name already says. Skip trivial code (obvious getters/setters, one-line wrappers).

## Go

`//` comments directly above the declaration, starting with the identifier name: `// FetchUser returns ...`.

- Every exported identifier gets a doc comment; unexported code only when non-obvious.
- Describe parameters and return values in the comment when they are not self-evident from the signature.
- Package comment: `// Package foo does X.` in `doc.go` or the first file.
- Validate: `gofmt -l .`, `go vet ./...`.

## TypeScript

JSDoc `/** */` above functions and classes. Types already carry much of the documentation.

```ts
/**
 * Fetch a page of products.
 *
 * @param {string} categoryId - Category to filter by.
 * @param {number} [page=1] - 1-indexed page number.
 * @returns {Promise<ProductPage>} The requested page.
 * @throws {NotFoundError} if the category does not exist
 */
async function fetchProducts(categoryId: string, page = 1): Promise<ProductPage> { ... }
```

- Document non-obvious parameters, error cases, side effects.
- Validate: `tsc --noEmit`.

## Python

Google-style docstrings:

```python
def fetch_user(user_id: int, active_only: bool = True) -> dict:
    """Fetch a single user by ID.

    Args:
        user_id: Unique identifier.
        active_only: When True, raise for inactive users.

    Returns:
        User record.

    Raises:
        UserNotFoundError: If no matching user exists.
    """
```

- Document public functions, classes, and modules.
- Validate: `python -m py_compile <file>`; if you add doctests, `python -m doctest <file>`.

## Terraform

HCL has no standard doc format. Use `#` comments for non-obvious blocks, and a README for modules.

- Module README: purpose, inputs (name, type, default, description), outputs, requirements (Terraform version, providers).
- Validate: `terraform fmt -check -recursive`, `terraform validate`.

## User guide

A single markdown file (e.g. `docs/user-guide.md`):

1. What it does, one paragraph.
2. Prerequisites.
3. Quick start: the shortest path to a working result.
4. Common tasks: one section each, copy-pasteable commands.
5. Troubleshooting: only errors users will actually hit.

Commands must be real: copy them from the codebase or run them; never invent flags.
