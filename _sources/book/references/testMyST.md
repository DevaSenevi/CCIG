To include this document within a Sphinx project,
include `myfile.md` in a [`toctree` directive](sphinx:toctree-directive) on an index page.

## Extend CommonMark with roles and directives

MyST allows any Sphinx role or directive to be used in a document.
These are extensions points allowing for richer features, such as admonitions and figures.

For example, add an `admonition` directive and `sup` role to your Markdown page, like so:

````md
# My nifty title

Some **text**!

```{admonition} Here's my title
:class: tip

Here's my admonition content.{sup}`1`
```
````

Then convert to HTML:

```html
$ myst-docutils-html5 --stylesheet= myfile.md
...
<div class="admonition tip">
<p class="admonition-title">Here's my title</p>
<p>Here's my admonition content.<sup>1</sup></p>
</div>
...
```

:::{seealso}
The full [](syntax/roles-and-directives.md) section
:::

(intro/reference)=
## Cross-referencing

MyST-Parser offers powerful cross-referencing features, to link to documents, headers, figures and more.

For example, to add a section *reference target*, and reference it:

```md
(header-label)=
# A header

[My reference](header-label)
```

```html
$ myst-docutils-html5 --stylesheet= myfile.md
...
<span id="header-label"></span>
<h1 class="title">A header</h1>

<p><a class="reference internal" href="#header-label">My reference</a></p>
...
```

:::{seealso}
The [](syntax/referencing) section,\
and the [ReadTheDocs cross-referencing](https://docs.readthedocs.io/en/stable/guides/cross-referencing-with-sphinx.html) documentation
:::

## Configuring MyST-Parser

The [](configuration.md) section contains a complete list of configuration options for the MyST-Parser.

These can be applied globally, e.g. in the sphinx `conf.py`:

```python
myst_enable_extensions = [
  "colon_fence",
]
```

Or they can be applied to specific documents, at the top of the document:

```yaml
---
myst:
  enable_extensions: ["colon_fence"]
---
```

## Extending Sphinx

The other way to extend MyST in Sphinx is to install Sphinx extensions that define new roles, directives, etc.

For example, let's install the `sphinxcontrib.mermaid` extension,
which will allow us to generate [Mermaid diagrams](https://mermaid-js.github.io/mermaid/#/) with MyST.

First, install `sphinxcontrib.mermaid`:

```shell
pip install sphinxcontrib-mermaid
```

Next, add it to your list of extensions in `conf.py`:

```python
extensions = [
  "myst_parser",
  "sphinxcontrib.mermaid",
]
```

Now, add a **mermaid directive** to your markdown file.
For example:

````md
# My nifty title

Some **text**!

```{admonition} Here's my title
:class: warning

Here's my admonition content
```

(section-two)=
## Here's another section

And some more content.

% This comment won't make it into the outputs!
And here's {ref}`a reference to this section <section-two>`.
I can also reference the section {ref}`section-two` without specifying my title.

:::{note}
And here's a note with a colon fence!
:::

And finally, here's a cool mermaid diagram!

```{mermaid}
sequenceDiagram
  participant Alice
  participant Bob
  Alice->John: Hello John, how are you?
  loop Healthcheck
      John->John: Fight against hypochondria
  end
  Note right of John: Rational thoughts <br/>prevail...
  John-->Alice: Great!
  John->Bob: How about you?
  Bob-->John: Jolly good!
```
````

When you build your documentation, you should see something like this:

```{mermaid}
sequenceDiagram
  participant Alice
  participant Bob
  Alice->John: Hello John, how are you?
  loop Healthcheck
      John->John: Fight against hypochondria
  end
  Note right of John: Rational thoughts <br/>prevail...
  John-->Alice: Great!
  John->Bob: How about you?
  Bob-->John: Jolly good!
```