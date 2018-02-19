# data-creative.info

A business [website](http://data-creative.info/) and [blog](http://data-creative.info/blog/).

## Usage

### Blog Post Filtering API

#### Filtering by Technology

Filter blog posts by technology. Specify the name of one technology using the url parameter, `tech`, replacing spaces with dashes as necessary.

```sh
GET http://data-creative.info/blog/?tech=ruby
GET http://data-creative.info/blog/?tech=JSON
GET http://data-creative.info/blog/?tech=d3.js
GET http://data-creative.info/blog/?tech=amazon-web-services
```

#### Filtering by Category

Filter blog posts by category. Specify the name of one category using the url parameter, `cat`, replacing spaces with dashes as necessary.

```sh
GET http://data-creative.info/blog/?cat=projects
GET http://data-creative.info/blog/?cat=reference-docs
```

## Contributing

Content edits and code contributions are both welcome. Fork the repo and submit a pull request. Thanks!

### Installation

Clone the repo and install dependencies.

```` sh
git clone git@github.com:data-creative/data-creative.github.io.git
cd data-creative.github.io
bundle install
````

Start local web server and view site at localhost:4000.

```` sh
bundle exec jekyll serve --watch
````

### Production

This site is hosted by [github pages](https://pages.github.com/). To update, push commits to master or merge another branch into master.

```` sh
git push origin master
````

## License

Content: [Creative Commons, BY-SA](http://creativecommons.org/licenses/by-sa/4.0/)

Code: [MIT](http://opensource.org/licenses/mit-license.php)
