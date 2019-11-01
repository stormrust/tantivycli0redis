## Creating the index:  `new`

Let's create a directory in which your index will be stored.

```bash
    # create the directory
    mkdir wikipedia-index
```

The new way is simply to do the above step and then copy
**meta.json** into that directory...

That will avoid doing all of this stuff below...

#### This was the old way to do it...

We will now initialize the index and create its schema.
The [schema](https://tantivy-search.github.io/tantivy/tantivy/schema/index.html) defines
the list of your fields, and for each field:
- its name
- its type, currently `u64`, `i64` or `str`
- how it should be indexed.

You can find more information about the latter on
[tantivy's schema documentation page](https://tantivy-search.github.io/tantivy/tantivy/schema/index.html)

In our case, our documents will contain
* a title
* a body
* a url

We want the title and the body to be tokenized and indexed. We also want
to add the term frequency and term positions to our index.
(To be honest, phrase queries are not yet implemented in tantivy,
so the positions won't be really useful in this tutorial.)

Running `tantivy new` will start a wizard that will help you
define the schema of the new index.

Like all the other commands of `tantivy`, you will have to
pass it your index directory via the `-i` or `--index`
parameter as follows:


```bash
    tantivy new -i wikipedia-index
```



Answer the questions as follows:

```none

    Creating new index
    Let's define its schema!



    New field name  ? title
    Text or unsigned 32-bit integer (T/I) ? T
    Should the field be stored (Y/N) ? Y
    Should the field be indexed (Y/N) ? Y
    Should the field be tokenized (Y/N) ? Y
    Should the term frequencies (per doc) be in the index (Y/N) ? Y
    Should the term positions (per doc) be in the index (Y/N) ? Y
    Add another field (Y/N) ? Y



    New field name  ? body
    Text or unsigned 32-bit integer (T/I) ? T
    Should the field be stored (Y/N) ? Y
    Should the field be indexed (Y/N) ? Y
    Should the field be tokenized (Y/N) ? Y
    Should the term frequencies (per doc) be in the index (Y/N) ? Y
    Should the term positions (per doc) be in the index (Y/N) ? Y
    Add another field (Y/N) ? Y



    New field name  ? url
    Text or unsigned 32-bit integer (T/I) ? T
    Should the field be stored (Y/N) ? Y
    Should the field be indexed (Y/N) ? N
    Add another field (Y/N) ? N

    [
    {
        "name": "title",
        "type": "text",
        "options": {
            "indexing": "position",
            "stored": true
        }
    },
    {
        "name": "body",
        "type": "text",
        "options": {
            "indexing": "position",
            "stored": true
        }
    },
    {
        "name": "url",
        "type": "text",
        "options": {
            "indexing": "unindexed",
            "stored": true
        }
    }
    ]


```

After the wizard has finished, a `meta.json` should exist in `wikipedia-index/meta.json`.
It is a fairly human readable JSON, so you can check its content.

It contains two sections:
- segments (currently empty, but we will change that soon)
- schema
