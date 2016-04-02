---
layout: post
title:  "API Pagination is your friend"
author: MJ Rossetti
categories:
 - posts
 - best-practices
img: flipping-through-pages.jpg
use_img_as_post_header: true
tags: none
published: true
icon_class: none
technologies:
 - html
 - json
 - curl
---

Pagination has a mixed reputation among developers who write applications that consume API data.

Some developers cite
 presence of pagination
 as justification for
 qualifying an API as "bad" or otherwise lacking in design or implementation.

Indeed, pagination does place a higher barrier to entry on inexperienced developers. But for experienced developers, pagination presents only a marginal difficulty.

When working with an API that returns a small amount of data, for example around a few hundred JSON objects, a single network request to the API endpoint URL is sufficient to yield the entire dataset. For example:

```` sh
curl http://www.gtfs-data-exchange.com/api/agencies # yields JSON representing the entire dataset of transit agencies
````

```` sh
curl http://api.citybik.es/v2/networks # yields JSON representing the entire dataset of bikeshare networks
````

However, many APIs offer volumes of data too great to return in a single network request. These APIs usually instruct the developer to pass additional URL parameters to the API endpoint to designate which subset of data to return. And their responses usually include metadata about the identifiers of the current, previous, next, and/or last page(s).

The pagination pattern requires more developer effort because it requires multiple network requests, each yielding an incremental subset of the data, in order to yield the entire dataset.

```` sh
curl https://api.my.org/v1/objects
curl https://api.my.org/v1/objects?page=2
curl https://api.my.org/v1/objects?page=3
````

The experienced developer leverages a scripting language like JavaScript, Ruby, Python, et. al. to loop through multiple requests in order to retrieve the entire dataset. Pagination strategies differ from API to API because each offers its own URL structures, URL parameters, and response structures.

For example, to consume paginated data from the [Open FEC API](https://api.open.fec.gov/developers) ["candidates" endpoint](https://api.open.fec.gov/developers#!/candidate/get_candidates), the [Open FEC API ruby gem](https://github.com/data-creative/open-fec-api-ruby#usage) instructs the following usage:

```` rb
# a ruby script excerpt which uses a 'client' from the "open_fec_api" ruby gem to make requests...
options = {:page => 1}
response = client.candidates(options) # the first API call
while response.page < response.pages do # comparison of the current page number to the last page number, to determine whether or not to place future calls...
  options.merge!({:page => response.page + 1}) # incrementing of page number
  response = client.candidates(options) # subsequent API call(s)
end
````

Paginated APIs do place somewhat of a higher burden on developers, but usually not high enough to outweigh the benefits of transmitting high volumes of data. And usually the difficulty is marginal. So criticism of API Pagination is largely undeserved.
