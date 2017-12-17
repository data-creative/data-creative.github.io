---
layout: post
title:  "ToneBase API 1.0, a Triumph of Project Planning"
author: MJ Rossetti
published: true
repo_url: https://github.com/data-creative/tonebase-api
project_url: https://tonebase-api.herokuapp.com/
img: rails-logo.svg # tonebase-client-app-homepage.png # tonebase-logo.jpg
use_img_as_post_header: true
categories:
 - open-source-application
technologies:
  - bundler
  - ruby
  - rails
  - rspec
  - jbuilder
  - heroku
  - git
---

ToneBase is a web-based information system designed to facilitate online music instruction through the posting, curation, and viewing of online videos.

The [ToneBase Team](https://twitter.com/tonebaseteam) comprised student fellows from the [Yale Entrepreneurial Institute](https://www.city.yale.edu/) (YEI). They commissioned Data Creative to build the [ToneBase API 1.0](https://tonebase-api.herokuapp.com/), a RESTful back-end web service to power the [front-end client application](https://tonebase.co/) they built themselves.

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-client-app-homepage.png" alt="">

Throughout the entirety of the project engagement, I was inspired by the passion, vision, and development capabilities of the ToneBase team. Their collaboration led to a successful project which met requirements and was delivered on-time.

## Project Management

I planned, designed, and developed this project from conception to final delivery in less than 45 days using a traditional ("waterfall") systems development methodology, infused with some "agile" techniques.

> There's a lot of waterfall-bashing going around these days, but I find its the most efficient methodology, especially for client engagements, and especially if you are experienced and capable, and have developed similar systems in the past. Waterfall forces you to have a plan, and a good plan allows you to push back effectively against scope creep. For a straightforward REST API based on clear requirements, I stand by the waterfall method. It doesn't mean you have to abandon agile practices like version control, collaborative issue-tracking, and iterative releases.

The project consisted of an initial "Discovery Phase", followed by a ten day "Planning Phase", followed by three week-long "Development Sprints", followed by a final week-long "Maintenance and Integration Phase".

### Discovery Phase

The clients posted the job to a local New Haven technology meetup group email list. I responded to the request via email and met with clients in-person twice to further discuss overall product vision and scope. As a result, I prepared and delivered a "Scope of Services" document which would outline the terms of the engagement.

#### Scope of Services Document

The Scope of Services document was an essential part of the success of this project. It established mutual expectations and a shared understanding between developers and clients. It specifically defined the system scope and goals, but did not assume to predefine any specific technical requirements.

The document included the following sections:

  1. Project Description
  2. Participant Roles
  3. Project Goals
  4. Project Phases (including dates)
  5. Project Deliverables (including dates)
  6. Developer Services
  7. Terms of Participation
  8. Payment Schedule and Methods
  9. Signatures

I was thankful the Terms of Participation allowed me to develop the API using open source technologies like PostgreSQL and Ruby on Rails, and release the software according to an [open source license](https://github.com/data-creative/tonebase-api/blob/master/LICENSE.md).

### Planning Phase

It took around ten days of intermittent client meetings and whiteboard sessions to translate conceptual ideas and user stories into a tangible system design.

> It can be tempting to leave some planning efforts for future determination, but I advise you invest time up-front to establish a solid plan. It makes the development process so much easier. Even if the planning phase takes a few more days, this will likely lead to development phase efficiencies which by far outweigh the initial time investment. Comprehensive planning efforts also preserve the relationship between client and developer by preventing conflicts over differences of expectations caused by plan ambiguity. I advise you don't move into the development phase without first establishing and agreeing upon a solid, comprehensive plan.

As a result of this planning effort, I produced a "Technical Requirements" document, which narrowly and specifically defined system scope and requirements.

#### Technical Requirements Document

Like the Scope of Services document, the Technical Requirements document was also an essential part of the success of this project. And it also served to establish mutual expectations and a shared understanding between developers and clients.

> Crafting a quality requirements document is essential to minimizing ambiguity and maximizing productivity during the development phase. Especially because thoughtful technical requirements lend themselves to be directly translated into subcomponent developer tasks, and allow the developer to focus on implementing instead of asking questions and waiting for further refinement of requirements. Even if you are using an agile approach, I would recommend you define requirements up-front in as comprehensive a manner as possible.

The Technical Requirements document included the following sections:

  1. System Description
  2. Information Requirements
  3. System Architecture
  4. User Roles and Permissions
  5. System Functionality

The Information Requirements section included a Data Flow Diagram, accompanied by a written explanation.

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-data-flow-diagram.png" alt="">

The System Architecture section included a Database Architecture Diagram and a Network Architecture Diagram, accompanied by written explanations of each.

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-database-architecture-diagram.png" alt="">

> The final database architecture would end up slightly differing from the draft design, but the draft was instrumental in its ability to guide development efforts. When planning a system design, don't be afraid to give it your best attempt and leave room for future changes, as long as they are principled and justified.

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-server-architecture-diagram.png" alt="">

> Not all parts of the system architecture were deemed in-scope for this project's development efforts, but it was still good to outline the entire system this way. Diagrams like this help clients understand how the system will work, and what the relationship is between various system components.

The User Roles and Permissions section defined the following roles:

  + **Visitor**: Non-users, and any users who are not logged-in.
  + **User**: Music students who consume system content.
  + **Artist**: Professional musicians and music instructors who contribute videos and other content to the system.
  + **Admin**: ToneBase team members, and anyone else responsible for maintaining and monitoring the system.

It also defined a set of permissions for each user role. These permissions specified which actions could be performed by each role, and directly mapped user capabilities with specific database resources and CRUD operations (e.g. "List", "Show", "Create", "Update", and "Destroy").

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-permissions-visitor.png" alt="">

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-permissions-user.png" alt="">

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-permissions-artist.png" alt="">

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-permissions-admin.png" alt="">

> These CRUD operation mappings provided clear requirements which would directly translate into developer efforts and aid development efficiency. Also, the "Notes" column formed the basis of test scenarios and use cases which enabled a test-driven development approach.

The System Functionality section further elaborated on the set of in-scope features for each role. These feature descriptions included user experience narratives, client application responsibilities (including example requests), and API server responsibilities (including example responses).

### Development Phase

After compiling an inventory of feature requirements, I used a simple spreadsheet to track development priorities and share progress with clients.

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-dev-priorities.png" alt="">

> The purple highlighted items were not originally included in project planning, but arose out of necessity during the development process. In any project, its important to leave a buffer for unplanned efforts, especially things you "know you don't know". Also, you'll notice the features did not always get implemented in their planned priority order. As a  developer in-tune with your own creative instincts, its important to be somewhat flexible and follow your gut feeling about when the time is right to develop any given feature.

I started with a document-driven development approach whereby I [populated a list of API endpoints](https://github.com/data-creative/tonebase-api/commit/f199b9929aede8ac427106e55bc809d08b276df3) which had been defined as part of the technical requirements document. Over time, I would iteratively update the API documentation as I implemented each one.

After generating a new Rails app, I [configured](https://github.com/data-creative/tonebase-api/commit/a5267a1ddd24954fd9d53689ad29dd1a4e2a1166) the RSpec test suite and [deployed](https://github.com/data-creative/tonebase-api/commit/1f4ae0c62dddd2ed5c903861b41233f40cc8d1c5) to a Heroku-hosted production server as soon as possible. This enabled me to practice test-driven development and iterative deployment throughout the entire development process.

After prepping the application for development of business logic, I started by [implementing](https://github.com/data-creative/tonebase-api/commit/220ef1a2c9e3768329205a2a2ce563cf97bd6ff0) tests and functionality for the `Instrument` resource. This resource was my top priority because the database architecture diagram showed it didn't depend on any other resources but many others either directly or indirectly depended on it. I figured it would be more efficient to start with an up-stream resource than to start with a down-stream resource which would need to be revised later after the implementation of the up-stream resource.

I then [implemented](https://github.com/data-creative/tonebase-api/commit/c635700cfb394623e4516f2ca483c5498194ce9c) tests and functionality for advertisement-related resources because they depended on the `Instrument` resource but had no other dependencies. I saw this group of related resources as comprising one of the application's stand-alone modules that included its own set of logic. I wanted to be able to complete this straightforward and somewhat isolated part of the system's domain and move on. After implementing these resources, I noticed commonalities among the application's controllers and [refactored](https://github.com/data-creative/tonebase-api/commit/c635700cfb394623e4516f2ca483c5498194ce9c#diff-834a6b4cfa87ae87e2e480a4674d916b) them into a generic [`ApiController`](https://github.com/data-creative/tonebase-api/commit/c635700cfb394623e4516f2ca483c5498194ce9c#diff-e3dabcd50af3f695337100894a51d413) from which the resource-specific controllers could inherit. The most important decision I made at this time was to also refactor shared test code into [RSpec "shared examples"](https://github.com/data-creative/tonebase-api/commit/c635700cfb394623e4516f2ca483c5498194ce9c#diff-b92771eca6fa696ee86650b591b81e21). This decision would drastically increase the pace and ease of future development, as it allowed me to write tests like `it_behaves_like "an index endpoint"` instead of writing a large file full of somewhat duplicative test code. Investing time early on to refactor tests was one of the best and most productive decisions I made during the development process, even though it initially set me back a day or two.

At this point, I was ready to [turn my focus to the application's views](https://github.com/data-creative/tonebase-api/commit/49c5ab6751eb45ad76419fd69cbdf3f73bbb4a40). I wrote view tests and configured JBuilder to render the views. I had not previously been very familiar with JBuilder, so I deliberately waited until I had some preliminary API responses working before I incorporated it into the project. It ended up being very helpful and efficient in its own right, and another great decision. I would highly recommend JBuilder for future projects, and have since incorporated it into my standard Rails configuration.

By this time, I had also configured the application for continuous integration using the [Travis](https://travis-ci.org/) platform.

My initial investments in application setup and refactoring started to pay off, allowing me to implement tests and functionality for a number of additional resources in quick succession.

I eventually [implemented](https://github.com/data-creative/tonebase-api/commit/b18375796cba9a48d91b8667900cfd2afcae4393) tests and functionality for the `User` resource. Initially, I thought I might need to configure [Devise](https://github.com/plataformatec/devise) to handle user authentication, but since the client application would be handling user authentication, I was able to implement this resource like any other normal resource. I used a `:role` attribute in conjunction with an `:access_level` attribute to distinguish between the various user roles and permissions.

Then I implemented various resources which depended on the `User` resource but no other resources. This included a mechanism to allow certain users to "follow" others. I knew this would involve a bi-directional self-referential relationship between users, and wasn't sure where to begin. So I started by [creating](https://github.com/data-creative/tonebase-api/commit/f8e904e41ede562a575ec2b922f406222e9d28f7) a resource called `UserFollow` to see how it would work. But I quickly learned my choice of name was producing undesired, counter-intuitive relationship names and as a result [changed](https://github.com/data-creative/tonebase-api/commit/36fe020e6e91331d8f7b9ac58575edd504a375e0) the name to `UserFollowship`.

The two resources at the heart of this domain model were `User` and `Video`, the latter depending on the former. So after feeling comfortable with my implementation of user-related resources I [implemented](https://github.com/data-creative/tonebase-api/commit/16513a670bd048d13668e61809db0320511528bf) tests and functionality which would allow them to upload associated videos. I knew there would be additional video-related associations (e.g. `VideoPart` and `VideoScore`), but I wasn't sure how to implement these nested associations, so I skipped them and resolved to return to them later.

As I mentioned, there was a time I thought I might be using Devise for user authentication, so I had put off incorporating an authentication mechanism until after implementing user-related resources. We knew the client application would be making requests which needed to pass an API key. I had previously built read-only APIs which passed API keys as part of the request's URL parameters, but the solution for this API needed to be more secure, so we decided to pass API keys via request headers. I had never done this before, so it took some research and integration testing using an [example Node.js client application](https://github.com/s2t2/tonebase-api-client-example/blob/master/views/index.ejs) to figure out how to do it successfully. When integration testing, I ended up needing to [modify](https://github.com/data-creative/tonebase-api/commit/2dd14a22e42a8dcba1f924efcc44e5b309acbc28) the `APIController` to `skip_before_action :verify_authenticity_token`, and [configure](https://github.com/data-creative/tonebase-api/commit/87dd37e2f01bfb9a7525d19a5085fe68eebb9f50) `Rack::Cors` to enable cross-origin requests. And after successfully demonstrating the client application's ability to issue GET requests, I finally [configured](https://github.com/data-creative/tonebase-api/commit/e1ad6f99c9c15ab0b301105515d49786fdcdd349) the API to force SSL and enforce API Key authentication.

At this time I decided to turn back to the challenge of creating nested associations via a single client request (i.e. the nested video-related associations I had been putting off). I [started with](https://github.com/data-creative/tonebase-api/commit/f0ddf770cfa3b452f2ebe05640f9da3f02beb27c) nested user-related associations (e.g. `UserProfile` and `UserMusicProfile`), before [turning to](https://github.com/data-creative/tonebase-api/commit/d2f136b8c93da412b070ac80a01b3ce7012dfc1c) the video-related associations. My implementation required me to revise some of my RSpec shared examples, and to add some workaround methods to the `User` and `Video` models which would only be used for the purpose of enabling these shared examples. It wasn't the prettiest solution, but it worked and allowed me to keep leveraging my shared examples, so I was happy to move on.

Then came a final flurry of resource implementations to complete the domain model. Afterwards, I implemented some final touches, undertaking efforts to [paginate responses](https://github.com/data-creative/tonebase-api/commit/470cd376ed4e04537fb074ea84ce810dd9b8b405) and improve the mechanism by which the client application could [search](https://github.com/data-creative/tonebase-api/commit/d82a63068170d8f47263dfb8d9368c134ed10eaf) for specific records.

By this time, I had been sprinting for a few weeks around the clock without much rest, so I was happy to finish development and deliver the software!

#### Source Code

Delivering the system's [final source code](https://github.com/data-creative/tonebase-api/) was easy because I had been using version control from the beginning, pushing commits to a remote GitHub repository multiple times per day. The GitHub interface allowed my clients to easily copy, download, investigate, and deploy the application's source code. Version control also facilitated a transparent development process, enabling my clients to see development progress unfold in real-time.

#### API Documentation

The [final documentation deliverable](https://github.com/data-creative/tonebase-api/blob/master/DOCS.md) included instructions on how to authenticate to the API, listed all database resources and corresponding API endpoints, and provided example requests and responses for each endpoint. It also included a final depiction of the API's domain model (below), and the aforementioned [example client application](https://github.com/s2t2/tonebase-api-client-example) to show the client how to integrate with the API using their desired technologies (in this case, a Node.js application using the Express framework).

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-final-erd.png" alt="">

> The "Rails ERD" gem auto-generated this diagram based on actual properties and architecture of the final database. So easy!
