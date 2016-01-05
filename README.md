# servemd

__`servemd`__ is a minimal server which supports markdown and authentication
for secured paths. The example directory shows typical usage, which looks as
follows:

```
.
├── docs
│   ├── secret
│   │   └── index.md
│   ├── index.md
│   ├── page.html
│   ├── paper.pdf
│   └── video.mp4
├── md.tpl
└── settings.yaml
```

## Installation

```
go get github.com/lukedmor/servemd
```

## Usage

To run the server, simply run:
```sh
servemd settings.yaml
```

The settings.yaml file specifies the location of the serving directory and
the markdown template, as well as the port and any secured root paths.
```yaml
dir: path/to/docs
port: 80
template: path/to/md.tpl
secrets:
  my_dir: my_password
  other_dir: other_password
```

The template file uses the format described at
[golang.org/pkg/text/template](http://golang.org/pkg/text/template).

__`servemd`__ first authenticates using HTTP Digest Access Authentication if
necessary. Literal matches to the path are served first, followed by files
matching an implicit extension, and finally a directory index if applicable.

Markdown is parsed using
[blackfriday](https://github.com/russross/blackfriday)'s `MarkdownCommon`
implementation, which features common extensions including fenced code
blocks and strikethroughs. Syntax highlighting can be done easily with
[Prism](http://prismjs.com) in the markdown template.

