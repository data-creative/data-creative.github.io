# data-creative.info

A business [website](http://data-creative.info/) and [blog](http://data-creative.info/blog/).

## Usage

### Blog Post Filtering API

Filter Blog Posts by technology. Specify the name of one technology using the url parameter, `tech`, replacing spaces with dashes as necessary.

```sh
curl http://data-creative.info/blog/?tech=ruby
curl http://data-creative.info/blog/?tech=JSON
curl http://data-creative.info/blog/?tech=d3.js
curl http://data-creative.info/blog/?tech=amazon-web-services
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
