---
title: The good, bad and the ugly in Micro-services
author: Sandun Perera
linkedin: https://lk.linkedin.com/in/sandunangelo
description : Micro-services, design Pattern
---


Micro-services is a software system design pattern and also known as a
software architecture. This pattern is mostly applied and suitable for
large and complex systems. In simple terms, the business components are
logically divided into independent services, is called Micro-services
pattern.

**Points of interest in Micro-services**

-   Services does not directly communicate with each other
    (inter-service communication is highly discouraged by the pattern).

-   Developers need to be creative enough to find a way to share the
    data among services without compromising the design principles.

-   Separation of concern is a must in your thinking.

**The Good**

-   *Independent*

> Each micro-service is independent from other micro-services and
> exposes the data and functionalities through APIs. They contains all
> the depending data or metadata resides within the micro-service to
> execute the logics within the service.

-   *Technology*

> The micro-service have the ability to use any technology stack without
> a dependency, what matters is how to communicate with the outside of
> the service only. Also the location where the service is deployed also
> not a problem till others can communicate with it.

-   *Stability*

> Since the micro-service is only focused on the service itself,
> developers can do a better implementation work of the service and
> with the decoupled nature of the service, the possibility of
> failures are lesser compared to monolithic systems.

-   *Performance*

> No other component is consuming the resources allocated to the
> micro-service, and the developers can allocate more resources as
> they needed, hence the performance can be easily maintained over the
> time and load of data.

-   *Scalability*

> Since the micro-service can be independently developed, moved, scale
> out and deployable to any location, scalability is just needed to be
> identified what need to be done in the terms of scaling, not what
> needs to do to the service. Developers only need to allocate more
> resources, partitioning, indexing or any other
> performance improvements.

-   *Maintainability*

> The nature of the isolation of the services, developers are allowed
> to maintain the service by adding more features, fixes and any other
> development work without impacting the other areas of the
> big system. Services can be deployed in a better seamless manner.

**The Bad**

-   *Hassle of starting a new project*

> When starting a new project with micro-services pattern, there are
> lots of project structuring related work to be done. You may be lucky
> if your company at least have a project template to start with.

-   *Too many technology involvement*

> At the beginning this may not feel as a big deal, but when your
> project gets growing with many services and other components, it
> feels like there is a heavy technology stack to work with and feels
> so heavy on it.

-   *Identifying the data to be redundant*

> Starting to split out into two micro-services is the beginning of
> identification of the data to be redundant over the multiple services
> in order to keep the principles of the architecture to be maintained
> and also the logics to work nicely.

-   *One brain may not enough*

> Doing a project in micro-services architecture needs lots of thinking
> and creativity to come up with a solid implementation, hence once
> person may not be able to do it by themselves, few people might need
> to get together and work to design the system.

-   *Too many brains spoil the design*

> The people who are going to involve in designing the system need to
> keep as smaller circle as possible. Because more the people involve
> more the ideas are distributed. Then by too many ideas there will be
> contradictory designs which make system consist of weak points at the
> end.

-   *Tough coordination of development team*

> Compared to a monolithic system, micro-services system takes more time
> to come into a stable process of development. In all the phases
> micro-services architecture system’s team management will be tougher
> than a monolithic system. Because there will be more dependencies for
> developers when development work is progressing.

-   *Experience matters*

> From seniors to juniors in the project team, the past experience on
> their respective subject area is a matter to pay attention. They
> should thoroughly understand the differences and similarities of
> monolithic and micro-services patterns and surrounding theories and
> practice on them.

**The Ugly**

-   *Same project micro-services may differ by everything*

> Even within the same project, each micro-service may have different
> project structure, technology stack, design patterns, etc. So the
> developer has to learn each micro-service implementation separately
> before doing any development work with them.

-   *New comer has to do a degree to start working*

> When a new person joins in the project team, they have to learn a lots
> of stuff before really get into the development work confidently.

-   *Additional documentations*

> Other than the usual documentations involving with the project,
> there may be more additional documents with diagrams, and
> explanations as learning resources for the team members to
> understand the complex areas of the implementations and related
> technical areas.

-   *Full deployment is a massive operation*

> Doing a full deployment usually takes longer time, but with
> micro-services its way more time consuming. Also the build and
> deployment steps has to be carefully scripted or configured to make
> the process smoother as possible.

-   *Setting up environments and developer machines*

> Setting up a developer machine was nothing much with the normal
> projects, but with micro-services, developers have to install several
> additional developer tools, emulators for services, and for all of
> those developers need to have better performing hardware and software
> to cater the load of applications to run simultaneously.

-   *Tendency of producing domain experts*

> When the toughness hit the team members, it is possible that some
> people may get lost in the middle, which leads that remaining people
> becomes the domain experts and everyone have to seek/depend on them in
> future needs of the project.

-   *Technology upgrades*

> A system development based on micro-services architecture takes more
> time and the technologies may also get outdated or newer versions are
> available. The team has to upgrade the system to latest technologies
> as a maintenance task time to time. Since there are many services to
> consider and each technology should be evaluated for compatibilities
> with other upgrades. This will need lots of concentration, patience
> and time.

-   *Data migrations*

> When releasing the system into production, or due to a later
> modification, when the team have to do a data migration, it is not
> going to be just another database. Since there are quiet possible
> data redundancy took place, they all need to take into consideration
> before the migrations starts.

-   *Expensive environments*

> Lots of services, and other supportive technologies to facilitate
> them, and may be lots of storage space and processing needs. The
> deployment environment need to a solid ground for such a massive
> system probably a cloud environment. Depending on the hosting vendor
> the each technology service adds up into to the total
> hosting expenses.

-   *Not an everyday project*

>  Micro-services architecture is much suitable for a system with high
> performance, complex, massive number of concurrent users, and
> geographically distributed system. For many developers these type of
> projects are not an everyday project. In a way it is a good
> opportunity for them to learn and practice such a massive system
> related knowledge.

**Conclusion**

Micro-services or Monolithic? Nothing is worst, nothing is the best,
developers have to figure it out the architecture to apply based on the
characteristics and requirements (current and possible future needs like
scale outs) of the system. And also the feasibility of the selected
architecture with the resources available and timeframe with the budget.

Back link:
<http://sandunangelo.blogspot.com/2018/02/the-good-bad-and-ugly-in-microservices.html>
