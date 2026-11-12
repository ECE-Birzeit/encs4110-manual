<a href="https://encs4110.birzeit.me"><img src="https://raw.githubusercontent.com/ECE-Birzeit/encs4110/main/docs/public/og.png" alt="ENCS4110 Computer Design Laboratory" width="100%"></a>

# ENCS4110 · Lab manual (LaTeX)

LaTeX sources for the ENCS4110 Computer Design Laboratory manual at Birzeit University. The PDFs built here are published on [encs4110.birzeit.me](https://encs4110.birzeit.me).

**[Full manual PDF](https://encs4110.birzeit.me/manual/encs4110_manual.pdf)** · [Website](https://encs4110.birzeit.me) · [Website source](https://github.com/ECE-Birzeit/encs4110) · [All labs](https://birzeit.me)

## Branches

- **`main`** — LaTeX source (chapters, preamble, resources, code snippets).
- **`pdf`** — published output only: the per-experiment PDFs and the full manual
  (`encs4110_1.pdf` … `encs4110_10.pdf`, `encs4110_manual.pdf`). This branch is
  consumed as a git submodule by the [encs4110](https://github.com/ECE-Birzeit/encs4110)
  website at `public/manual`.

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

GitHub Actions ([`.github/workflows/build.yml`](.github/workflows/build.yml)) builds
every experiment and the full manual in a TeX Live container:

| Event | What happens |
|---|---|
| Pull request | Build only, so a chapter that doesn't compile is caught before merging |
| Push to `main` | Build, then commit the PDFs to the `pdf` branch |
| Push a tag `v*` | Build, then create a GitHub Release with the PDFs attached |

Every run also keeps the PDFs as a downloadable workflow artifact for 30 days.

So the normal flow is: edit a chapter, push to `main`, and the `pdf` branch updates
itself. Then bump the submodule pointer in the website repo to put the new PDFs live:

```sh
cd ../encs4110            # the website repo
git submodule update --remote public/manual
git add public/manual && git commit -m "Update manual PDFs"
```

To mark a version (for example at the start of a semester), tag `main` and push the tag.
The release page then keeps that exact set of PDFs, and the release notes list the
changes since the previous tag:

```sh
git tag -a v1.1.0 -m "Fall 2026"
git push origin v1.1.0
```

Building and publishing by hand still works: with `dist/` as a worktree of `pdf`,
`make all`, then commit and push from inside `dist/`.

## Contributing

Found a mistake in the PDF? [Open an issue](https://github.com/ECE-Birzeit/encs4110-manual/issues/new/choose) or send a pull request against the chapter in `chapters/`. Run `make` for the experiment you changed before pushing, to make sure it still compiles. See the [contributing guide](https://github.com/ECE-Birzeit/.github/blob/main/CONTRIBUTING.md).

## License

The written content is licensed under [CC BY 4.0](LICENSE): you may share and adapt it, including for your own course, as long as you credit the source. Code, meaning the code listings in `snippets/` and the `Makefile`, is licensed under [MIT](LICENSE-CODE). © 2026 Mohammad Abu-Shelbaia.

Not covered by either license, and still owned by their authors:

- `resources/birzeit-logo.png` and `resources/birzeit-logo.svg`

The Birzeit University name and logo are the university's; they are not licensed here. Screenshots of third-party software, and product names and logos, belong to their respective owners.

---

<sub>Part of [ECE Labs](https://birzeit.me) · Maintained by [Mohammad Abu-Shelbaia](https://github.com/mabushelbaia)</sub>
