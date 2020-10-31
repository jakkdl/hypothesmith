# hypothesmith
Hypothesis strategies for generating Python programs, something like CSmith.

This is definitely pre-alpha, but if you want to play with it feel free!
You can even keep the shiny pieces when - not if - it breaks.

Get it today with [`pip install hypothesmith`](https://pypi.org/project/hypothesmith/),
or by cloning [the GitHub repo](https://github.com/Zac-HD/hypothesmith).

You can run the tests, such as they are, with `tox` on Python 3.6 or later.
Use `tox -va` to see what environments are available.

## Usage
This package provides two Hypothesis strategies for generating Python source code.

The generated code will always be syntatically valid, and is useful for testing
parsers, linters, auto-formatters, and other tools that operate on source code.

> DO NOT EXECUTE CODE GENERATED BY THESE STRATEGIES.
>
> It could do literally anything that running Python code is able to do,
> including changing, deleting, or uploading important data.  Arbitrary
> code can be useful, but "arbitrary code execution" can be very, very bad.

#### `hypothesmith.from_grammar(start="file_input", *, auto_target=True)`

Generates syntactically-valid Python source code based on the grammar.

Valid values for ``start`` are ``"single_input"``, ``"file_input"``, or
``"eval_input"``; respectively a single interactive statement, a module or
sequence of commands read from a file, and input for the eval() function.

If ``auto_target`` is ``True``, this strategy uses ``hypothesis.target()``
internally to drive towards larger and more complex examples.  We recommend
leaving this enabled, as the grammar is quite complex and only simple examples
tend to be generated otherwise.

#### `hypothesmith.from_node(node=libcst.Module, *, auto_target=True)`

Generates syntactically-valid Python source code based on the node types
defined by the [`LibCST`](https://libcst.readthedocs.io/en/latest/) project.

You can pass any subtype of `libcst.CSTNode`.  Alternatively, you can use
Hypothesis' built-in `from_type(node_type).map(lambda n: libcst.Module([n]).code`,
after Hypothesmith has registered the required strategies.  However, this does
not include automatic targeting and limitations of LibCST may lead to invalid
code being generated.

## Notable bugs found with Hypothesmith
- [BPO-40661, a segfault in the new parser](https://bugs.python.org/issue40661),
  was given maximum priority and blocked the planned release of CPython 3.9 beta1.
- [BPO-38953](https://bugs.python.org/issue38953) `tokenize` -> `untokenize` roundtrip bugs.
- [BPO-42218](https://bugs.python.org/issue42218) mishandled error case in new PEG parser.
- [`lib2to3` errors on \r in comment](https://github.com/psf/black/issues/970)
- [Black fails on files ending in a backslash](https://github.com/psf/black/issues/1012)
- [At least three round-trip bugs in LibCST](https://github.com/Instagram/LibCST#acknowledgements)
  (search commits for "hypothesis")
- [Invalid code generated by LibCST](https://github.com/Instagram/LibCST/issues/287)

### Changelog

Patch notes [can be found in `CHANGELOG.md`](https://github.com/Zac-HD/hypothesmith/blob/master/CHANGELOG.md).
