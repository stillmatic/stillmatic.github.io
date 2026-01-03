---
title: Scaling Agents
author: Chris Hua
date: '2026-01-02'
description: Two approaches to running many agents
slug: scaling-agents
categories: [articles]
tags: [agents, product]
---

(This is a companion piece to [link])

A good chunk of my professional career has been spent trying to _spend more money_. Pre-LLMs, this was trying to scale fraud pipelines at close to Google search scale. With LLMs, I've spent probably O($1M) to programatically label and distill data. Being able to spend that money somewhat intelligently requires real infrastructure and planning to set up.

Agentic coding setups are similar. Just using Claude Code by itself won't spend tokens fast enough. Running multiple instances makes more sense, especially since we usually have a bunch of tasks that we want to do at once. But then you run into problems of merge conflicts, limited CPU/RAM contention, so on. In my case, I sometimes will type a bunch of commands, then go play a video game while I wait, then tab through a few changes. Can't have RAM contention be a problem if I'm in match!! (kidding...)

So we want to use more compute, and the easiest way to do that is to put it into the cloud. I tried two approaches for doing so.

The first approach I tried is to put the [entire agent into the cloud](https://hingeloss.com/post/2025/12/26/agents-at-home/), including the conversation interface. 

Pros: 
- available everywhere (desktop, mobile)

Cons:
- more maintenance to keep up to date with upstream SDK features
- more work to create the UI and other states

In my case, I built this for a very specific workflow that needed the uniqueness, as well as being programatically driven.


The second approach I tried is to only put the [agent environment](https://github.com/stillmatic/sandbox-mcp-modal) into the cloud, and reuse the Claude Code interface. This is kinda similar to the idea of a Backend-for-Frontend (BFF), keep Claude Code and proxy calls elsewhere. The goal is to be an easy drop in that users can install.

Pros:

- Keeps the Claude Code UI + UX, basically identical to before, just with a backend in the cloud
- Easy install to existing workflows

Cons:
- Modal Sandboxes die in somewhat unpredictable manners and are quite a bit more expensive than functions. But these are just implementation details and much less than the tokens anyways.
- With restarts, quite a bit more lag (ie sandbox creation time). 
- Need to do additional work to sync logs to other computers / visual logs

So what will work?

My guess is that the right UI is a combination of these. Run the code in the cloud, allow any session to be resumed locally or cloud. Step in and see the state of the workflow / agentic trace at any point. Not sure anything really works yet.

More work to be done! 