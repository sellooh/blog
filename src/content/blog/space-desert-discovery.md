---
title: 'Space Desert Discovery'
description: 'Article about a personal project called Space Desert'
pubDate: 'Sep 23 2023'
heroImage: '/space-desert-discovery/b.png'
---

In the "real world" of software development, the main questions usually are:

- Why build something?
- What to build exactly?

The interesting thing is that those questions are not technical at all.

**"Why"** is related to understanding the business goals: Reduce costs, increase productivity, improve user experience and so on.

**"What"** is about defining the thing that will achieve those goals: Implement feature X, collect metric Y, change technology Z, etc.

In that scenario a process of <u>discovery</u> is an important step to complete where you speak with multiple stakeholders to extract information. The technical "How" is a mere consequence of doing the previous steps well.

---

But since the discovery is a highly contextual process, it's hard to share an example that will be useful for the reader. In this article I have a special scenario that can display the process in a more tangible way.

---

I've merged 3 different interests into a personal project that I'm calling "Space Desert". The interests are:

- Learning Golang
- Studying AWS architecture
- Build a game

With that I have enough **"Why"** and **"What"** to start the discovery process.

## Starting the Discovery

To make things as easy as possible I started by drawing a wireframe of the game as seen bellow:

![wireframe](/space-desert-discovery/wireframe.png)

Game Description: A multiplayer game where players purchase resources, accumulate and build them in a central board. The goal by the end of the game is to get the most amount of points by building high scoring patterns.

Next, I tried to list down the key concepts, challenges and user actions that the system should support.

![concepts](/space-desert-discovery/concepts.png)

Some of those concepts start to give hints on which technical solutions should be designed such as:
- Invite Api: Component to connect users to a game
- Scheduling Service: Component responsible for performing an action if a user user perform an action within a certain time
- Game Api: The core component that should validate user actions, persists and transform the game state

It's an iterative process, so I scratched a _very_ simple data schema to help visualize the data.

![basic-schema](/space-desert-discovery/basic-schema.png)

Finally I got to a point where I understood enough of the domain and the features to define technological pieces.

> Note: I'm optimizing for time & cost in this prototype implementation. I've chosen technologies that are quick and easy to setup and implement and also that don't incur in high costs.

![arch](/space-desert-discovery/arch.png)

The UI has a lot of moving parts, as much as I want to use Astro I couldn't justify a MPA technology for this project. In the SPA world React and Angular would be solid choices, I ended up choosing Svelte and SvelteKit.

For an initial implementation of the backend I wanted to avoid a VPC and services that cost on a per hour basis. AWS provides enough managed services to expose an API, configure websockets, do computing, schedule tasks and store data that can cover all the needs.

SST to define the infrastructure and simplify the ops.

At a high level, the architecture is the simplest serverless setup one can come up with.

![services](/space-desert-discovery/services.png)

Now to get through a first iteration of the Discovery I decided to just focus on the core, the Game Api. Defining this api also helps with understanding the entities access patterns, which is needed to design the DynamoDB table.

![apis](/space-desert-discovery/apis.png)

> 💡 Tip: Make sure to identify the most complex components asap. That exposes the unknowns early and reduces the risk of making an incorrect assumption.

---

The table design currently shows the relationship between Users and a Game. The GSI1 allows for querying the user's resources, scores and board states by Game Id. Note that the images below are not detailing every single property, but the relationships which are more important to get right.

![main-index](/space-desert-discovery/main-index.png)
![gsi1](/space-desert-discovery/gsi1.png)

Lastly for this iteration I've included a sequence diagram to start getting into the details of the Game Api.

![seq](/space-desert-discovery/seq.png)

---

As continuation for this article I've been working on the algorithm to calculate the score of a board in Golang using goroutines and channels. That has been quite interesting so I'm excited to share it in the next article.


