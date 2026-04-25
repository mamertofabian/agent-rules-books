# 📚 AGENTS Book Rules

![AGENTS Book Rules](books-ai-rules.png)

MIT licensed project rules for Codex, Cursor, and Claude Code.

This repository contains ready-to-use project rules for coding agents, based on well-known books about software design, architecture, refactoring, legacy code, reliability, and data-intensive systems.

Each set is a practical interpretation of a book's principles as instructions for AI coding tools. The goal is not to replace the books or copy their content. The goal is to make their most important engineering decisions easy to apply during everyday agent-assisted development.

The repository is organized as:

```text
<book>/
  codex/AGENTS.md
  cursor/.cursor/rules/<book>.mdc
  claude/.claude/rules/<book>.md
```

The repository also includes a synthesized default rule set:

```text
unified-software-engineering/
  codex/AGENTS.md
  cursor/.cursor/rules/unified-software-engineering.mdc
  claude/.claude/rules/unified-software-engineering.md
```

## Books

### A Philosophy of Software Design

Author: John Ousterhout

The book focuses on fighting complexity through deep modules, simple interfaces, information hiding, and design choices that reduce cognitive load. This rule set is especially useful for API design, module design, and refactoring shallow abstractions.

### Clean Architecture

Author: Robert C. Martin

The book describes designing systems around stable boundaries, the dependency rule, and the separation of business policies from details such as frameworks, databases, and UI. This rule set helps keep code resistant to technology churn.

### Clean Code

Author: Robert C. Martin

The book focuses on readability, naming, small functions, responsibilities, tests, and simplicity. This rule set is a strong default for everyday coding and code review.

### Code Complete

Author: Steve McConnell

The book covers a broad range of software construction practices: routine design, variables, classes, control flow, defensive programming, coding standards, and testing. This rule set helps agents make disciplined implementation decisions.

### Design Patterns: Elements of Reusable Object-Oriented Software

Authors: Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides (the Gang of Four)

The book catalogues 23 classic object-oriented design patterns organized into creational, structural, and behavioral categories. This rule set adapts the catalog through a modern lens: it identifies which patterns remain genuinely useful, which are now handled by language features (closures, pattern matching, generics, reactive streams), and when to skip a pattern entirely. It covers the four guiding principles as core agent behavior, calls out language-native replacements, and documents common anti-patterns and misuse.

### Designing Data-Intensive Applications

Author: Martin Kleppmann

The book covers reliability, scalability, consistency, replication, partitioning, transactions, data streams, and schema evolution. This rule set is intended for systems where data ownership, event flows, and consistency semantics matter.

### Domain-Driven Design

Author: Eric Evans

The book introduces domain modeling, ubiquitous language, bounded contexts, tactical patterns, and strategic design. This rule set helps agents think in terms of the business model rather than tables, controllers, or DTOs.

### Domain-Driven Design Distilled

Author: Vaughn Vernon

The book is a short, practical introduction to DDD. It focuses on subdomains, bounded contexts, context mapping, and basic tactical patterns. This rule set is a good fit when you want the benefits of DDD without excessive ceremony.

### Implementing Domain-Driven Design

Author: Vaughn Vernon

The book shows how to apply DDD in real systems: aggregates, domain events, contexts, integrations, and application architecture. This rule set is more implementation-focused than `domain-driven-design-distilled`.

### Language Implementation Patterns

Author: Terence Parr

The book describes practical patterns for building interpreters, compilers, parsers, and domain-specific languages: recursive descent parsing, Pratt expression parsing, AST construction, visitor traversals, direct interpretation, virtual machines, code generation, type checking, and runtime support. This rule set is intended for projects that implement language tooling, DSL engines, or query processors.

### Patterns of Enterprise Application Architecture

Author: Martin Fowler

The book catalogues enterprise application patterns: layers, service layer, transaction script, domain model, data mapper, repository, unit of work, identity map, DTO, and integration patterns. This rule set helps choose an appropriate pattern instead of mixing responsibilities accidentally.

### Refactoring

Author: Martin Fowler

The book describes safe ways to improve code structure without changing observable behavior. This rule set emphasizes small steps, tests, code smell detection, and keeping refactoring separate from feature changes.

### Release It!

Author: Michael T. Nygard

The book focuses on systems that survive production reality: failures, overload, timeouts, retries, circuit breakers, bulkheads, backpressure, observability, and deployment behavior. This rule set is useful for services, APIs, queues, integrations, and critical production paths.

### The Pragmatic Programmer

Authors: Andrew Hunt, David Thomas

The book describes a pragmatic approach to software development: responsibility, DRY at the knowledge level, orthogonality, automation, fast feedback, prototyping, and adaptability. This rule set works well as a general engineering layer.

### Unit Testing: Principles, Practices, and Patterns

Author: Vladimir Khorikov

The book describes a disciplined approach to unit testing: AAA structure, full isolation, fast deterministic execution, meaningful assertions on behavior rather than implementation, and clear test naming. This rule set is essential for writing tests that are maintainable, readable, and survive refactoring.

### Working Effectively with Legacy Code

Author: Michael Feathers

The book explains how to safely change difficult, poorly tested code: characterization tests, seams, dependency breaking, sprout method, wrap method, and incremental risk reduction. This rule set is best for legacy work where the first goal is regaining control.

## Unified Rule Set

`unified-software-engineering` is a synthesized rule set that combines the unique principles from all supported books into one coherent agent instruction file.

It is not a concatenation of the book-specific rules. Repeated guidance is merged, and apparent conflicts are resolved through context-specific decision rules. For example, it tells the agent when a simple transaction script is enough, when richer domain modeling is justified, when production resilience matters more than ideal-path elegance, and how to preserve behavior during refactoring.

Use it when you want one broad default for general engineering work. Do not usually enable it together with every individual book rule set; that duplicates context and can make the agent less consistent. If a task needs a strong specialized lens, use `unified-software-engineering` alone or pair it with one focused rule set such as `release-it`, `refactoring`, or `working-effectively-with-legacy-code`.

## Choosing Rules

Do not enable all rules at once.

Reasons:

- they consume model context
- they may repeat the same ideas in different words
- they may have different priorities depending on the work
- the agent may follow an overly broad or inconsistent instruction set less reliably
- some books are situational, for example `Release It!` is critical for production systems but should not necessarily steer every simple UI change

Choose rules based on the task:

- broad default: `unified-software-engineering`
- everyday code quality: `clean-code`, `code-complete`
- architecture and boundaries: `clean-architecture`, `domain-driven-design`, `patterns-of-enterprise-application-architecture`
- design patterns and object-oriented structure: `design-patterns-gof`
- domain modeling: `domain-driven-design`, `domain-driven-design-distilled`, `implementing-domain-driven-design`
- refactoring: `refactoring`, `a-philosophy-of-software-design`
- legacy code: `working-effectively-with-legacy-code`, optionally `refactoring`
- production systems: `release-it`
- data systems: `designing-data-intensive-applications`
- language tooling and DSLs: `language-implementation-patterns`
- general engineering style: `the-pragmatic-programmer`
- unit testing: `unit-testing-principles`

A good default is to start with one primary rule set and add a second only when it materially changes the agent's decisions. For a specific task, you can temporarily copy a rule into the project, use it during the work, then remove or disable it.

## Repository Layout

Each book directory contains the same three tool-specific variants:

```text
clean-code/
  codex/
    AGENTS.md
  cursor/
    .cursor/
      rules/
        clean-code.mdc
  claude/
    .claude/
      rules/
        clean-code.md
```

The naming convention is lowercase kebab-case:

```text
working-effectively-with-legacy-code/
designing-data-intensive-applications/
patterns-of-enterprise-application-architecture/
```

## Supported Tools

### Codex

Convention: `AGENTS.md`

Usage:

```bash
cp clean-code/codex/AGENTS.md /path/to/project/AGENTS.md
```

Codex loads `AGENTS.md` from the project directory and relevant parent directories based on where it is started. If you want rules to apply only to part of a repository, place `AGENTS.md` closer to that subdirectory.

### Cursor

Convention: `.cursor/rules/*.mdc`

Usage:

```bash
mkdir -p /path/to/project/.cursor/rules
cp refactoring/cursor/.cursor/rules/refactoring.mdc /path/to/project/.cursor/rules/
```

Cursor rule files include frontmatter with `description` and `alwaysApply`. If you copy multiple rules, consider changing `alwaysApply: true` to a more selective mode, or keep only the rules that should genuinely apply to every task.

### Claude Code

Convention: `.claude/rules/*.md`

Usage:

```bash
mkdir -p /path/to/project/.claude/rules
cp working-effectively-with-legacy-code/claude/.claude/rules/working-effectively-with-legacy-code.md /path/to/project/.claude/rules/
```

Claude can load rules from `.claude/rules/`. For larger rule sets, prefer a few well-chosen rules over one huge instruction file. Shorter, more specific context is easier for the model to follow consistently.

## Important Note

These rules are inspired by the books listed below. They are not official materials from the authors or publishers, and they are not a substitute for reading the books.

The files in this repository are practical engineering instructions written for AI coding tools. They intentionally avoid reproducing book text. Use them as lightweight working agreements, not as summaries or study notes.

## Adding a Book

Use this convention when adding a new rule set:

```text
new-book-title/
  codex/AGENTS.md
  cursor/.cursor/rules/new-book-title.mdc
  claude/.claude/rules/new-book-title.md
```

Guidelines:

- use lowercase kebab-case for directory and file names
- provide all three variants: Codex, Cursor, and Claude Code
- keep rules operational and specific, not descriptive summaries
- avoid copying book text
- adapt principles into project guidance for implementation, refactoring, review, and testing
- keep Cursor `.mdc` files with frontmatter including `description` and `alwaysApply`
- add the book to the Books section with author and a short description
- avoid enabling every rule by default in downstream projects

## License

The code and rules in this repository are released under the MIT License. It is one of the simplest and most widely accepted open source licenses: it allows use, copying, modification, publication, distribution, and sublicensing with minimal restrictions.

See [LICENSE](LICENSE) for details.

## Author

[Maciej Ciemborowicz](https://maciej-ciemborowicz.eu)
