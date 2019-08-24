## presentations i've done for things

The presentations in this folder use reveal-js and reveal-md, so that each presentation is a Markdown file with associated assets. These are published as HTML pages to http://matthewhanson.github.io/presentations.


## Serving and building presentations

Most presentations here use custom CSS and designed to work with a specific theme. Unless specified otherwise this is the `solarized` theme. Serve the presentation with:

    $ reveal-md path/to/markdown.md --css path/to/style.css --theme solarized

To create the HTML files:

    $ reveal-md <path/to/markdown.md> --css path/to/style.css --theme solarized  --static ../docs/<some-directory> --static-dirs assets
