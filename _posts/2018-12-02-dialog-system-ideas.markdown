---
layout: post
title:  "Dialog System Ideas"
date:   2018-12-02 10:47:01 -0800
categories: dialog design
---
One of the first systems I've decided to revamp is the dialog system since at the moment it has the following limitations:

- Fully text based
- No interactions
- Can complete quests

In my desire for a more fleshed out system that I can play around with a bit more, some of the things I'd like to be able to include are:

- Contextual dialog (e.g. before/during/after quest completion)
- Interaction/choices (e.g. good/bad conversation options)
- Images for the speakers

The biggest problem to solve is how to go about different voice lines based on the current state of the game. The current script uses a Lines `List` in the following way:

{% highlight json %}
{
  0: "n-NPC"
  1: "Hello"
  2: "n-Player"
  3: "Hi!"
}
{% endhighlight %}

This works by swapping the speaker identifier with the `n-` label and then any other line replaces the text, it will loop through until it changes the text (so it can process two lines at once). This is a great basic system, but expanding on this system would just mean adding more Lines arrays, but it would need a fixed condition that seems to get restrictive fast. Thus a new solution is required.

What seems like it might work better is a branching tree structure where I can have any kind of conditional inserted at any point, whether that's a player line choice or a game state variable.

As far as implementing this system goes, there are a few challenges, primarily driven from my lack of c# knowledge. The next is to decide whether or not to try develop my own system or to use one of the existing solutions. Two of the most popular solutions I've come across are [Yarn](https://github.com/InfiniteAmmoInc/Yarn) and [Ink](https://github.com/inkle/ink).

Ultimately, what I decided to go with was a fixed state machine that is based on whatever quest a character was associated with. With this solution there are 3 possible states:

- Before Quest Accepted
- While Quest is in Progress
- After Quest Completion

Any of these states can optionally create a new quest or fixing an existing quest. For example, when you talk to NPC1 before a quest is accepted, it's likely that they will give you a quest so then if you try to talk to them straight after, the dialog would switch to the quest in progress lines.

This is not necessarily the optimal solution as it doesn't leave much room for expanding and in future it would be fun to put in branching dialog paths as well, but for now this seems to be a great start to making a more interactive RPG dialog system.
