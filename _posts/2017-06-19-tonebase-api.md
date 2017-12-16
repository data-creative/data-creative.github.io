---
layout: post
title:  "ToneBase API 1.0"
author: MJ Rossetti
published: true
repo_url: https://github.com/data-creative/tonebase-api
project_url: https://tonebase-api.herokuapp.com/
img: tonebase-logo.jpg # rails-logo.png
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

The [ToneBase Team](https://twitter.com/tonebaseteam) comprised of student fellows from the [Yale Entrepreneurial Institute](https://www.city.yale.edu/) (YEI) commissioned Data Creative to build the [ToneBase API 1.0](https://tonebase-api.herokuapp.com/), a RESTful back-end web service to power the [front-end client application](https://tonebase.co/) they built themselves.

During the project engagement, I was inspired by the vision and development capabilities of the ToneBase team. Their collaboration led to a successful project which met requirements and was delivered on-time.

## Project Management

This project was planned, designed, and developed from conception to final delivery in less than 45 days using a traditional ("waterfall") systems development methodology, infused with some "agile" system development techniques.

Project efforts consisted of an initial "Discovery Phase" followed eventually by a ten day "Planning Phase", three week-long "Development Sprints", and a final week-long "Maintenance Phase".

### Discovery Phase

The client posted the job over email to a local technology group. I responded to the request and met with clients twice to further discuss overall product vision and scope. As a result, I prepared and delivered a "Scope of Services" document which would outline the terms of the engagement.

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

### Planning Phase

It took around ten days of intermittent client meetings to translate conceptual ideas and user stories into a tangible system design.

As a result of this planning effort, I produced a "Technical Requirements" document, which narrowly defined system scope and requirements, and formed a basis for shared expectations between developer and clients.

#### Technical Requirements Document

The Scope of Services document was an essential part of the success of this project. It established mutual expectations and a shared understanding between developers and clients.

The Technical Requirements document included the following sections:

  1. System Description
  2. Information Requirements
  3. System Architecture
  4. User Roles and Permissions
  5. System Functionality

The Information Requirements section included a Data Flow Diagram, accompanied by a written explanations.

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-data-flow-diagram.png" alt="">

The System Architecture section included a Database Architecture Diagram and a Server Architecture Diagram, accompanied by written explanations of each.

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-database-architecture-diagram.png" alt="">

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-server-architecture-diagram.png" alt="">

The User Roles and Permissions section defined the following in-scope roles:

  + **Visitor**: Non-users, and any users who are not logged-in.
  + **User**: Music students who consume system content.
  + **Artist**: Professional musicians and music instructors who contribute videos and other content to the system.
  + **Admin**: ToneBase team members, and any other individual responsible for maintaining and monitoring the system.

It also defined a set of permissions for each user role, including a direct mapping to development prioritization and specific database resources and controller actions.

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-permissions-visitor.png" alt="">

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-permissions-user.png" alt="">

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-permissions-artist.png" alt="">

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-req-permissions-admin.png" alt="">

The System Functionality section further defined desired features for each role. For each feature, it described the desired user experience, client application responsibilities (including example requests), and API server responsibilities (including example responses).

### Development Phase

Crafting a quality requirements document is essential to minimizing ambiguity and maximizing productivity and efficiency during the development phase. A good technical requirements document lends itself to be directly translated into subcomponent developer tasks.

I used a simple spreadsheet to track development priorities and share progress with clients.

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-dev-priorities.png" alt="">

The purple highlighted items were not originally included in project planning, but arose out of necessity during the development process. In any project, its important to leave a buffer for unplanned efforts.






Based on ongoing collaborative feature prioritization efforts, some of the planned functionality was deemed out of scope of the agreed-upon development terms of this project.

#### Source Code

...

#### API Documentation


Included example requests and responses

Final system domain model:

<img class="img-responsive" src="{{ site.base_url }}/assets/img/posts/tonebase-final-erd.png" alt="">
