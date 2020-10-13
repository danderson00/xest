# xest

## Background

xest is the culmination of 20 years of discussion, and nearly 10 years of research and development driven by a desire 
to address the extremely high degree of incidental complexity and duplication of effort in the software development 
process. It represents a fundamental shift in how we think about modelling software, from very "noun centric" reasoning 
about static "snapshots" of the state of a system, to directly describing the actors in a system as products of the 
events that occur within a system.

It's important to note that xest does not ignore the progress that the industry has made during those years, but
merely rearranges some of the fundamental pieces, resulting in powerful new mechanisms for modelling the real world.
These mechanisms give tantalising glimpses of solutions to some of the industry's long held but as yet unsolved goals 
like plain language specifications, visual process design and enabling significantly more business participation in the
software process.

## A Word on Complexity

"Complexity" is undoubtedly the number one cited reason for the widespread failure of software projects, failure that
has led to an extremely risk averse environment in the enterprise. Bespoke software development is considered
too risky and the customisation of large commercial off-the-shelf (COTS) products is now the preferred approach to
delivering large scale software projects. This has led to a stifling of innovation in the industry where the tech
giants have no incentive to risk significant disruption to the status quo.

So what is complexity? From our necessarily very developer centric perspective, incidental complexity is any artifact
a developer must reason about that is orthogonal to the business domain, i.e. does not directly relate to the business
problem we are attempting to solve. The list is virtually endless. (image: concept word cloud)

It's not hard to see how the sheer number of concepts a developer must understand to effectively perform their job 
inevitably leads to a fragmentation and specialisation of skills across teams (i.e. front-end developer, full 
stack developer, etc.), introducing new human elements to the problem. Anyone who has been in the game for any
significant period of time knows how challenging the human element is to manage in any project!

## The Status Quo

- static snapshots
- large, isolated monolithic systems (Blackpearl)
- dependency management - npm clusterfuck

### "Implicit" Coding

- state machines
  - visualisation
- types
- we're actually just capturing events

## The Grand Vision

- plain text specifications
- visual process / navigation design
- WYSIWYG UI design fed by strongly typed models
- better business participation
- fully automated, visual analytics
- fully automated optimisation and scaling
- a curated set of UI components

- a full suite of tools encapsulating all of the above

## Baby Steps

- What is xest?
- Mathematical background (Rx / Erik Meijer)
- Think about events - every piece of business software is about recording, transforming and displaying event data:
  - practice management (scheduling, journaling, etc.), 
  - utility management (meter readings, maintenance, invoicing, etc),
  - customer relationship management (recording customer interactions, data points)
  - sooner or later you start thinking about EVERYTHING as derivations of past events - even you
- Code fragment comparisons / demos
- Re-use / vocabulary
- xstate visualiser

## The Immediate Benefits

- Reduction in complexity!
- Improved toolset - query tools, visualisation tools
- Real time distribution
- Security
- Concurrency / conflict resolution
- Auditing
- Debugging
- Profiling

- Peer to peer!

## Looking to the Enterprise

- "Microservices" / Autonomous Units
- Aggregations
- Vastly improved understanding of data flow
- Visualisations
- Better, automated scalability
- Integration with existing systems

## The Path to Commercialisation

### Immediate Goals

- PaaS offering
- Open source base elements
  - Create community
  - Encourage innovation
- Closed source "value add"
  - Hosting ("serverless")
  - Visual designers
- "Middle ground" document database built on xest?
  
### Medium Term Goals

- Procure enterprise customer
- Build out enterprise capabilities
- Build key apps "in house"
  - Peer to peer social media
- Leverage community
  - curated UI components

### Long Term / Exit Strategy

- Acquisition
- Significant third party investment

## Elaborating on the "Grand Vision"

- AI / ML to interpret plain language specifications
- Full browser based, visual set of design tools
  - "Rules engine" - for want of a better phrase
  - WYSIWYG UI designer
    - curated components
    - third party integrations (google maps, etc.)
  - Strongly typed models (TypeScript)
