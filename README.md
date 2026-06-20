# mre-oxfmt-collapses-embedded-markdown-list-in-jsx

A minimal reproduction example of a bug occurring in oxfmt 0.55.0.

When oxfmt formats an MDX file containing a Markdown list embedded inside a JSX
element (here Astro Starlight's `<FileTree>`), it flattens the entire list onto a
single line, destroying the list structure and producing wrong output. This is a
correctness bug: the formatter corrupts valid input.

## Steps to reproduce

1. `bun install`
2. `bun run repro` (runs `oxfmt --write repro.mdx`)
3. Inspect the file: `git diff repro.mdx` (or open `repro.mdx`)
4. Observe the `<FileTree>` list has been collapsed onto a single line

## Expected behavior

The Markdown list embedded in `<FileTree>` is preserved as a multi-line list:

```mdx
   <FileTree>
   - documentation
     - public/
       - favicon.svg
     - src/
       - assets/
         - logo.svg
   </FileTree>
```

## Actual behavior

The list is flattened onto one line. The list structure is gone and the rendered
output is wrong:

```mdx
   <FileTree>- documentation - public/ - favicon.svg - src/ - assets/ - logo.svg</FileTree>
```

## Environment

- oxfmt: 0.55.0 (also reproduces on 0.52.0)
- Bun: 1.3.13
- OS: macOS (Darwin 25.5.0)

Run with oxfmt's default configuration (no config file). Distinct from
[oxc-project/oxc#16608](https://github.com/oxc-project/oxc/issues/16608)
(js-in-xxx): this is a Markdown list that is a *child* of a JSX element, not
embedded JS/TS.

## License

Since this repository contains a minimal reproduction example of a bug
occurring in an open-source technology, this code is hereby released into the
public domain to serve the greater good of improving open-source tooling. See
[LICENSE](LICENSE) for the full terms.
