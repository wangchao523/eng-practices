# 写出好的ChangeList说明

一个ChangeList说明应该包含**当前做的事情**和**为什么这么做**。它将成为我们的版本控制历史的永久部分，并且除了你的评论者之外，几年以后还有数百人可能阅读它。

Future developers will search for your CL based on its description. Someone in
the future might be looking for your change because of a faint memory of its
relevance but without the specifics handy. If all the important information is
in the code and not the description, it's going to be a lot harder for them to
locate your CL.

## 第一行 {#firstline}

*   正在做什么的简短摘要。
*   完整的句子，就好像是订单的写法。
*   后边跟一个空行。

The **first line** of a CL description should be a short summary of
*specifically* **what** *is being done by the CL*, followed by a blank line.
This is what most future code searchers will see when they are browsing the
version control history of a piece of code, so this first line should be
informative enough that they don't have to read your CL or its whole description
just to get a general idea of what your CL actually *did*.

By tradition, the first line of a CL description is a complete sentence, written
as though it were an order (an imperative sentence). For example, say
\"**Delete** the FizzBuzz RPC and **replace** it with the new system." instead
of \"**Deleting** the FizzBuzz RPC and **replacing** it with the new system."
You don't have to write the rest of the description as an imperative sentence,
though.

## Body is Informative {#informative}

The rest of the description should be informative. It might include a brief
description of the problem that's being solved, and why this is the best
approach. If there are any shortcomings to the approach, they should be
mentioned. If relevant, include background information such as bug numbers,
benchmark results, and links to design documents.

Even small CLs deserve a little attention to detail. Put the CL in context.

## 糟糕的ChangeList说明 {#bad}

"Fix bug" is an inadequate CL description. What bug? What did you do to fix it?
Other similarly bad descriptions include:

-   "Fix build."
-   "Add patch."
-   "Moving code from A to B."
-   "Phase 1."
-   "Add convenience functions."
-   "kill weird URLs."

Some of those are real CL descriptions. Their authors may believe they are
providing useful information, but they are not serving the purpose of a CL
description.

## 不错的ChangeList说明 {#good}

Here are some examples of good descriptions.

### Functionality change

> rpc: remove size limit on RPC server message freelist.
>
> Servers like FizzBuzz have very large messages and would benefit from reuse.
> Make the freelist larger, and add a goroutine that frees the freelist entries
> slowly over time, so that idle servers eventually release all freelist
> entries.

The first few words describe what the CL actually does. The rest of the
description talks about the problem being solved, why this is a good solution,
and a bit more information about the specific implementation.

### 重构

> Construct a Task with a TimeKeeper to use its TimeStr and Now methods.
>
> Add a Now method to Task, so the borglet() getter method can be removed (which
> was only used by OOMCandidate to call borglet's Now method). This replaces the
> methods on Borglet that delegate to a TimeKeeper.
>
> Allowing Tasks to supply Now is a step toward eliminating the dependency on
> Borglet. Eventually, collaborators that depend on getting Now from the Task
> should be changed to use a TimeKeeper directly, but this has been an
> accommodation to refactoring in small steps.
>
> Continuing the long-range goal of refactoring the Borglet Hierarchy.

The first line describes what the CL does and how this is a change from the
past. The rest of the description talks about the specific implementation, the
context of the CL, that the solution isn't ideal, and possible future direction.
It also explains *why* this change is being made.

### 简单的ChangeList需要有上下文介绍

> Create a Python3 build rule for status.py.
>
> This allows consumers who are already using this as in Python3 to depend on a
> rule that is next to the original status build rule instead of somewhere in
> their own tree. It encourages new consumers to use Python3 if they can,
> instead of Python2, and significantly simplifies some automated build file
> refactoring tools being worked on currently.

The first sentence describes what's actually being done. The rest of the
description explains *why* the change is being made and gives the reviewer a lot
of context.

## 在提交代码前，自己审查ChangeList说明

CLs can undergo significant change during review. It can be worthwhile to review
a CL description before submitting the CL, to ensure that the description still
reflects what the CL does.

Next: [Small CLs](small-cls.md)
