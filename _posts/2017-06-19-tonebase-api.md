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

The System Architecture section included a Database Architecture Diagram and a Server Architecture Diagram, accompanied by written explanations of each.

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

The System Functionality section further elaborated on the set of in-scope features for each role. The description of each feature included a user experience narrative, client application responsibilities (including example requests), and API server responsibilities (including example responses).

### Development Phase

After compiling an inventory of feature requirements, I used a simple spreadsheet to track development priorities and share progress with clients.

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-dev-priorities.png" alt="">

> The purple highlighted items were not originally included in project planning, but arose out of necessity during the development process. In any project, its important to leave a buffer for unplanned efforts.

#### Setup

I started with a document-driven development approach which [populated a list of API endpoints](https://github.com/data-creative/tonebase-api/commit/f199b9929aede8ac427106e55bc809d08b276df3) specified by the technical requirements document. Over time, I would iteratively update the API documentation as I implemented each one.

Then I [configured the RSpec test suite](https://github.com/data-creative/tonebase-api/commit/a5267a1ddd24954fd9d53689ad29dd1a4e2a1166) and [deployed to a Heroku-hosted production server](https://github.com/data-creative/tonebase-api/commit/1f4ae0c62dddd2ed5c903861b41233f40cc8d1c5) as soon as possible. This enabled me to practice test-driven development and iterative deployment.

#### Quick Wins

After prepping the application for development of business logic, I started by [implementing](https://github.com/data-creative/tonebase-api/commit/220ef1a2c9e3768329205a2a2ce563cf97bd6ff0) tests and functionality for the "Instruments" resource. This resource was my top priority because it had straightforward attributes, but more importantly because the database architecture diagram showed it didn't depend on any other resources but many others either directly or indirectly depended on it.

Then I [implemented](https://github.com/data-creative/tonebase-api/commit/c635700cfb394623e4516f2ca483c5498194ce9c) tests and functionality for advertisement-related resources because they depended on the instruments resource but had no other dependencies. After implementing these resources, I noticed commonalities among the corresponding controllers and [refactored](https://github.com/data-creative/tonebase-api/commit/c635700cfb394623e4516f2ca483c5498194ce9c#diff-834a6b4cfa87ae87e2e480a4674d916b) them into a generic [`ApiController`](https://github.com/data-creative/tonebase-api/commit/c635700cfb394623e4516f2ca483c5498194ce9c#diff-e3dabcd50af3f695337100894a51d413) from which the resource-specific controllers could inherit. The most important decision I made at this time was to also refactor shared test code into ["shared examples"](https://github.com/data-creative/tonebase-api/commit/c635700cfb394623e4516f2ca483c5498194ce9c#diff-b92771eca6fa696ee86650b591b81e21). This decision would drastically increase the pace and ease of future development, as it allowed me to write tests like `it_behaves_like "an index endpoint"` instead of writing a large file full of somewhat duplicative test code. Investing time early on to refactor tests was one of the best decisions I made during the development process.

... api key authentication via request headers ...

... nested endpoints ...



#### Source Code

Delivering the system's [final source code](https://github.com/data-creative/tonebase-api/) was easy because I used version control from the beginning. Pushing commits to a remote GitHub repository provided clients with flexibility in their ability to copy, download, investigate, and deploy the application's source code. Version control also served to facilitate a transparent development process, as it enabled clients to see development process in real-time.

#### API Documentation

The [final documentation deliverable](https://github.com/data-creative/tonebase-api/blob/master/DOCS.md) included instructions on how to authenticate and issue requests to the API. It listed all database resources and corresponding API endpoints. It also included a final depiction of the API's domain model (below), and an [example client application](https://github.com/s2t2/tonebase-api-client-example) to show the client how to integrate with the API using their desired technologies (in this case, a Node.js application using the Express framework).

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-final-erd.png" alt="">

> The "Rails ERD" gem auto-generated this diagram based on actual properties and architecture of the final database. So easy!
