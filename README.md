# Presentations I've done for things

The presentations in this folder use reveal-js and reveal-md, so that each presentation is a Markdown file with associated assets. These are published as HTML pages to http://matthewhanson.github.io/presentations. However, there is currently no directory listing so you need to know the complete path to a specific presentation.


## Listing

### 2019

- FOSS4G-NA San Diego
  - STAC and sat-utils

### 2018

- SatSummit 3
- FOSS4G Dar es Salaam
- FOSS4G-NA St Louis


## Serving and building presentations

Most presentations here use custom CSS and designed to work with a specific theme. Unless specified otherwise this is the `solarized` theme. Serve the presentation with:

    $ reveal-md path/to/markdown.md --css path/to/style.css --theme solarized

To create the HTML files:

    $ reveal-md path/to/markdown.md --css path/to/style.css --theme solarized

    $ reveal-md . --theme solarized --css style.css --static ../docs/2019 --static-dirs assets

