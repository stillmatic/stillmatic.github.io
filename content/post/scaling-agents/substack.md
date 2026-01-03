---
title: Scaling Agents
author: Chris Hua
date: '2026-01-02'
description: --dangerously-skip-permissions
slug: scaling-agents
categories: [articles]
tags: [agents, product]
---

You might imagine modern farming is peasants, but with tractors. It's not. It's [this](https://www.youtube.com/watch?v=Qa1EBYiPfHk)—specialized machines that would be unrecognizable to someone from a few hundred years ago. Software engineering is about to undergo the same transformation.

Opus 4.5 is an unbelievable tool in the hands of a competent driver, and it's rapidly changing how frontier labs and companies write code. "Coding," in the sense of humans writing programming languages, is on its last legs. There are unsolved edge cases, to be sure, but the end is nigh. Software engineering, though—the practice of executing ideas into outputs—will continue. It just won't look like what we're used to.

The traditional tool was the IDE. Humans wrote code, saw outputs, thought about it, made it better. Our limiting factors were skill and iteration speed—how quickly we could see problems and update our understanding.

One day we'll look at software written by humans as specialty products. Artisanal, single-author programs. But artisanal food production looks nothing like commercial production, at any part of the supply chain. Your restaurant food comes prepped from Sysco and anonymous commissary kitchens. They get them from mechanized farms, with rows of specialized machines but very few humans.


## The Levels

There are levels to how AI coding works. Steve Yegge independently came up with a [similar taxonomy](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04) which I think is interesting, but I'd add a focus on what limits each level — what each stage is gated by.  

**Gated by human writing speed:**

1. Human full manual in VSCode
2. Human copy-pasting to ChatGPT
3. Human writing with inline suggestions / copilot

**Gated by human thinking speed:**

4. Human working with Claude Code in CLI
5. Human working with multiple instances of Claude Code

At level 5, you hit logistical problems: concurrent file access, CPU limits, handling git worktrees. We've always tried to speed up iteration through compute. CGI artists went from local GPUs to render farms. Mobile developers went from test devices to emulators to device farms. With agentic coding, we will also move beyond the constraints of developers' laptops.

6. Human orchestration of many agents in the cloud

Now you can have many branches, infinitely scalable compute. But you're still limited by human driving. This is probably where the most advanced setups are today, lots of people are here with different permutations. I've scaled this kind of setup over the last few weeks with a few approaches - [see my writeup](https://hingeloss.com/post/2026/01/02/scaling-agents).

## The Shift

Everything up to level 6 is **human push / bot pull**. Human tells bot what to do; bot executes.

Level 7 is different:

7. AI-driven orchestration — many agents, cloud

This is **bot push / human pull**. The bot proposes; the human confirms, redirects, or rolls back. The human isn't driving anymore — they're steering.

The best analogy might be trading. You might imagine modern trading floors are the old hand-signal pits, but with Bloomberg terminals. They're not. They're mostly empty rooms with servers. But even firms with the most technical expertise and reputation still rely heavily on human trader intuition. They may have world-class infrastructure and technical execution, but they still hire poker players and IMO medalists.  

The execution is systematized. The trading strategies are systematized. The taste and gut are not.

Same thing is coming for software. Levels 1-5: execution, mostly done. Level 6: orchestration, becoming systematizable. Level 7+: taste, intent, reading whether this feature will actually solve the user's problem. That part is still human, and maybe indefinitely so.

## The Problem

The question isn't "when do humans leave the loop." It's "what is the right human surface area." We're moving up the layer of abstraction, and the next set of tools will too.

A Jane Street trader isn't deciding which API to call. They're reading the room. Similarly, the level 7+ human isn't choosing function signatures. They're judging whether the agent's proposal actually solves the problem.

To get to level 7, we need agents that can propose good directions, not just execute instructions well. That's a different capability, and we don't quite have the training signal for it yet. We need trajectories of entire programs and products, along with the human signal of which suggestions were accepted.

We don't have this today because:

1. Tokens are expensive. Tab autocompletes are speculative—we generate them and willfully throw them away. As models get better and cheaper, we should expect to do the same at higher levels of abstraction: agents implementing multiple paths or features, humans choosing which to accept. But speculative generation at the feature level is still costly.
2. Long context is hard. Reasoning over entire codebases and product directions requires capabilities we're still developing.
3. Not enough human feedback at this level. We don't have the same density of signal for features and products as we do for code.

Decision-making in software is challenging. To pass more decision-making responsiblity from product managers to AI, we need to lower the risk. To do so, we can make it 1) easier to make the right decisions, and 2) less consequential to get things wrong. The path is probably better simulation: testing for correctness (did the code work?) and simulated users for hypothesis validation (was this the right thing to build?). My hunch is that "simulation" will support a few very successful startups.

This will happen in 2026. We RL'd models to write code using autocomplete feedback. Now we will RL them to propose features. We're just moving up the levels of abstraction.