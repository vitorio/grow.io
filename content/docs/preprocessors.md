---
$title: Preprocessors
$category: Reference
$order: 8
---
# Preprocessors

[TOC]

Preprocessors do things like optimization and code generation against your pod's source files. Grow runs preprocessors every time affected source files change, allowing you to "save and refresh" to preview your changes. Preprocessors are also run at build time.

Grow includes the below preprocessors as built-ins with the SDK, and you'll always be free to bring your own external processing tools (such as Grunt or Gulp).

## Global settings

By default, preprocessors run when the development server is started and when your site is built. You can control preprocessor execution by using the `autorun` parameter and optionally by using the `name` parameter.

[sourcecode:yaml]
preprocessors:
- name: my_preprocessor
  kind: <kind>
  autorun: false
  tags:
  - red
  - blue
[/sourcecode]

A preprocessor with the above configuration will only be run when using `grow preprocess -A` or when using `grow preprocess -p my_preprocessor`. This can be handy to control how often a data-importing preprocessor (such as Google Sheets or Google Docs importers) run. You may not want to run those preprocessors each time your site is built.

## Kinds

### Google Docs

The Google Docs preprocessor downloads a Google Doc and saves it to a data file within your pod. It can optionally convert the Google Doc to Markdown, ready for use with the `markdown` filter in templates. Specifying an `.md` file extension for the `path` parameter will convert the output to Markdown.

After enabling this preprocessor, when you run the `grow build` command, you will be prompted (once on the command line) to authorize Grow to read your Google Drive files.

[sourcecode:yaml]
preprocessors:
- kind: google_docs
  path: /data/filename.{html|md}                        # Where to save downloaded file.
  id: 1ZhJshmT2pZq_IALA6leXJ0oRuKVO12N9BbjAarAT8kI      # File ID.
  convert: true                                         # Whether to convert the file to Markdown.
[/sourcecode]

### Google Sheets

The Google Sheets preprocessor downloads data from a Google Sheet and saves it to a data file within your pod. The data can then be consumed by the `g.csv` tag, for example, in templates.

After enabling this preprocessor, when you run the `grow build` command, you will be prompted (once on the command line) to authorize Grow to read your Google Drive files.

Grow will export data from Google Sheets as either JSON or CSV, depending on the extension used in the `path` field.

[sourcecode:yaml]
preprocessors:
- kind: google_sheets
  path: /data/filename.{csv|json}                       # Where to save downloaded file.
  id: 1ZhJshmT2pZq_IALA6leXJ0oRuKVO12N9BbjAarAT8kI      # Spreadsheet ID.
  gid: 0                                                # Worksheet ID (optional).
  pretty_print: false                                   # Whether to format the output with "pretty print". (optional)
[/sourcecode]

### Gulp

The Gulp preprocessor simplifies using Gulp in conjunction with Grow. Instead of running Gulp and Grow separately, Grow can manage Gulp as a subprocess and run different Gulp tasks at build and run time.

Typically, you would pair `grow run` with a Gulp task that watches for changes and rebuilds static assets; and you would pair `grow build` with a Gulp task that builds static assets for release. You can use the `build_task` and `run_task` options to control the Gulp task that is run for these two Grow commands.

[sourcecode:yaml]
preprocessors:
- kind: gulp
  build_task: "build"             # Task to run at build time (optional).
  run_task: ""                    # Task to run when the development server runs (optional).
[/sourcecode]

### Sass

Use the [Sass CSS extension language](http://sass-lang.com/) to create, minify, and maintain your site's CSS files. Sass files in `sass_dir` must have either a `.scss` or `.sass` extension and are automatically generated to files ending in `.min.css`.

[sourcecode:yaml]
kind: sass
preprocessors:
- sass_dir: /source/sass/         # Where to find source Sass files.
  out_dir: /static/css/           # Where to write generated CSS files.
  suffix: .min.css                # Suffix of the generated filename (optional).
  # Style of compiled result (optional, default: compressed).
  output_style: {nested|expanded|compact|compressed}
  source_comments: {yes|no}       # Whether to add comments about source lines (optional).
  image_path: /static/images/     # Where to find images (optional).
[/sourcecode]
