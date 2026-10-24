<a href="https://encs4110.birzeit.me"><img src="https://raw.githubusercontent.com/ECE-Birzeit/encs4110/main/docs/public/og.png" alt="ENCS4110 Computer Design Laboratory" width="100%"></a>

# ENCS4110 · Lab manual (LaTeX)

LaTeX sources for the ENCS4110 Computer Design Laboratory manual at Birzeit University. The PDFs built here are published on [encs4110.birzeit.me](https://encs4110.birzeit.me).

**[Full manual PDF](https://encs4110.birzeit.me/manual/encs4110_manual.pdf)** · [Website](https://encs4110.birzeit.me) · [Website source](https://github.com/ECE-Birzeit/encs4110) · [All labs](https://birzeit.me)

## Branches

- **`main`** — LaTeX source (chapters, preamble, resources, code snippets).
- **`pdf`** — published output only: the per-experiment PDFs and the full manual
  (`encs4110_1.pdf` … `encs4110_9.pdf`, `encs4110_manual.pdf`). This branch is
  consumed as a git submodule by the [encs4110](https://github.com/ECE-Birzeit/encs4110)
  website at `docs/public/manual`.

## Output worktree

The `dist/` directory is a **git worktree of the `pdf` branch**, created once with:

```sh
git worktree add dist pdf
```

The build writes PDFs straight into `dist/`, so committing there commits to the
`pdf` branch — that is how the PDFs get synced to the website. File names use a
**single digit** (`encs4110_1.pdf`), matching the links the website expects.

## Building

Requires a TeX distribution with `pdflatex` (`-shell-escape`) and **Inkscape**
on `PATH` (the `svg` package shells out to it to render the SVG figures, e.g.
the Birzeit logo). Build everything with `make`:

```sh
make all      # every experiment (1-10) + the full manual -> dist/
make manual   # just the full manual -> dist/encs4110_manual.pdf
make 5        # just one standalone experiment -> dist/encs4110_5.pdf
make clean    # remove generated PDFs and LaTeX aux files

# override PDFLATEX if pdflatex is not on PATH
make all PDFLATEX="C:/Users/.../MiKTeX/.../pdflatex.exe"
```

## Publishing PDFs

Because `dist/` is the `pdf` branch, publishing is just a commit + push there:

```sh
cd dist
git add -A
git commit -m "Update built PDFs"
git push origin pdf
```

Then bump the submodule pointer in the website repo:

```sh
cd ../encs4110            # the website repo
git submodule update --remote docs/public/manual
git add docs/public/manual && git commit -m "Update manual PDFs"
```

## Contributing

Found a mistake in the PDF? [Open an issue](https://github.com/ECE-Birzeit/encs4110-manual/issues/new/choose) or send a pull request against the chapter in `chapters/`. Run `make` for the experiment you changed before pushing, to make sure it still compiles. See the [contributing guide](https://github.com/ECE-Birzeit/.github/blob/main/CONTRIBUTING.md).

## License

The written content is licensed under [CC BY 4.0](LICENSE): you may share and adapt it, including for your own course, as long as you credit the source. Code, meaning the code listings in `snippets/` and the `Makefile`, is licensed under [MIT](LICENSE-CODE). © 2026 Mohammad Abu-Shelbaia.

Not covered by either license, and still owned by their authors:

- `resources/birzeit-logo.png` and `resources/birzeit-logo.svg`

The Birzeit University name and logo are the university's; they are not licensed here. Screenshots of third-party software, and product names and logos, belong to their respective owners.

---

<sub>Part of [ECE Lab Manuals](https://birzeit.me) · Maintained by [Mohammad Abu-Shelbaia](https://github.com/mabushelbaia)</sub>
