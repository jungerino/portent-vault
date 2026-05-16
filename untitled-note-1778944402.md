---
type: Note
---
# How I Run a Big Open Source Project

Hey, Luca here! As you probably know by now, two weeks ago I launched [Tolaria](https://github.com/refactoringhq/tolaria), a desktop app for Mac, Linux, and Windows, to manage markdown knowledge bases — for humans and AI.

I did that to solve my own problems and to get my hands dirty with AI coding. In other words, I wanted to *walk the talk*, and then write about it here on Refactoring.

Little I knew this would feel more like *speedrunning* the talk.

As we speak, Tolaria has several thousands daily users, 10K+ stars on Github, and is the 10th fastest growing repo… in the world, as measured by Star History. And we also made it to the [weekly trending](https://github.com/trending/) page of Github.

![chart.jpeg](/Users/luca/Workspace/refactoring-vault/attachments/1777975101674-chart.jpeg)

To put things in context, there are only about ~5K repos in the world with more than 10K stars, out of ~30M.

This is of course, incredibly flattering and energizing, but I also joked with friends that I have been glued to my chair non-stop for these two weeks, because all this interest also posed very real operational problems.

Users, in fact, who are mostly engineers, contributed hard — and I am glad they did! As of today, Tolaria has received:

- 200+ issues on Github
- 250+ PRs
- 150+ feature requests on [Canny](https://tolaria.canny.io/)

So I had to create workflows to help handle all of this: to ensure issues would be fixed fast and people would get feedback about ideas and contributions.

Fast forward to today, I am pretty happy with how it is going, and as I write this, we are down to:

- 15 open issues
- 19 PRs
- 81 feature requests

Today’s newsletter covers all these workflows and how I “survived” these two weeks, while only working part-time on Tolaria, and the rest of the time on Refactoring — in the meantime I also had to publish 4 newsletter editions and 2 podcast episodes.

So here is the agenda:

- 📥 **Inputs** — mapping and normalizing sources and channels.
- 🗃️ **Backlog** — turning everything into a single prioritized list from which agents can take work.
- 🔍 **Validation** — the one true bottleneck, as everyone is fond of saying these days.
- 🚀 **Release** — making work available to everyone and updating all the input sources.
- 📊 **Analytics** — tracing what happens in production.

Let's dive in!

***

## 📥 Inputs

I have never maintained a sizable open source project before, so you'll forgive me if some of what I am about to say will sound trivial to you.

At any given time, there are several types of inputs Tolaria receives, that can *spawn* work to be done:

1. Bug reports from users
2. Feature requests from users
3. PRs from contributors
4. Crash reports from telemetry (Sentry)
5. My own ideas (hello 👋)

These are all separate channels but, with some degree of simplification, they spawn two types of work: *bugs* and *features*.

More specifically:

- #1 and #4 spawn bugs
- #2 and #5 spawn features.
- #3 (PRs) can spawn either, but mostly features.

#1, #2, and #3 originally all lived in Github Issues, but bugs and feature requests have different *needs* and I believe they deserve to live in separate places. In fact:

- **Bugs** — only need to be replicated and squashed as soon as possible. I am a strong believer in zero-bugs policy, and I have a dedicated swimlane of work just to catch bugs as soon as they are reported, and fix them (more on that later). There is very little to decide about bugs.
- **Feature requests** — are another story. They benefit from being voted on (you want to see how many people want something), are generally *bigger,* so you may want to flag things as *planned* or *in progress* so users are notified, and of course, you will *not want *to build features that don't match your vision.

From a workflow standpoint, bugs are easy, features are not — which is why today we are down to only 15 open bugs out of 200, but still 80+ open feature requests out of 150.

The way I manage this is by keeping bugs on [Github Issues](https://github.com/refactoringhq/tolaria/issues), and feature requests on [separate product board](http://tolaria.canny.io) on Canny.

To make people understand this, I created a rich "contribute" panel inside Tolaria where you are *routed* to different places based on how you want to help:

![BlockNote image](/Users/luca/Workspace/refactoring-vault/attachments/1777900671321-CleanShot_2026-05-04_at_15.17.04_2x.png)

Still a lot of things go wrong all the time:

- People submit feature requests on Github
- People submit duplicated feature requests on Canny
- People submit duplicated feature requests... on Github
- People submit bug reports on Canny
- ...

To solve this, for each channel I have a hourly procedure run by Brian, my Openclaw, that does *intake* and traffic control: it closes duplicate entries, moves misplaced ones to the right channel, and turns new ones into actual tasks in the backlog (more on that later).

![BlockNote image](/Users/luca/Workspace/refactoring-vault/attachments/1777901024081-CleanShot_2026-05-04_at_15.20.13_2x.png)

It runs automatically and notifies me on Telegram when something new is found / moved / created.

So yes, this means that some people on Github and Canny get automated responses when this routing happens, generated by the AI. 👇

![BlockNote image](/Users/luca/Workspace/refactoring-vault/attachments/1777901252694-CleanShot_2026-05-04_at_15.26.14_2x.png)

Is it ok to have AI automatically reply to people? I asked myself this question, and in this case I think *yes* — as it's simple operational communication, aimed at giving clear info and speeing up the feedback loop.

There is a dedicated *intake *procedure for crashes too, which uses Sentry MCP to fetch new issues, and create tasks for them.

Now, after all this stuff is surfaced and routed, where does it go? 👇

***

## 🗃️ Backlog

At some point, all of this just becomes stuff to be done, and I believe it should get to a **single backlog** for the agents to pick up from.

*What* gets into this backlog, though, and in which order, is one of the things I *personally* need to decide. So for me to being able to operate this, all these tasks need to be ported to a single place, from which eventually the *single backlog* is created.

This place, for me, it's the giant **Tolaria Board on Todoist**.

Yep, Todoist is no Linear, has no fancy support for various dev workflows, but I have been using it for years, all my personal tasks live too, and I haven't found blockers or things I am not able to do here that would justify the migration.

![mega todoist board.jpeg](/Users/luca/Workspace/refactoring-vault/attachments/1777903173917-mega_todoist_board.jpeg)

So, planning work for me means moving tasks from various *draft* columns, into the main **Open** one. Codex then works sequentially, 24/7, taking items from the Open column, implementing things and moving them to "in review".

The draft columns are the following:

- **Drafts** — actual drafts of feature requests from users. These are the results of inputs taken by Brian from the various channels, tentatively spec'd following our templates and principles, given an initial priority value, and put into Draft.
- **Contributor PRs** — PRs people submit on Github, triaged and evaluated based on our coding standards, security, and product fit.
- **Someday** — big ideas I jot down and that I still need to figure out myself.

Where are bugs?

Bugs, either from Sentry or from Github, are created directly in *Open*. So yes, it can happen that a user reports a bug, the AI reproduces it, it considers it worthy to be fixed, it fixes it, and goes in review before I realize the bug was even reported.

Does this pose a security risk? Can users *inject* changes to be made to Tolaria?

I personally think this risk is very low.

Requests, and later code, pass through various gates: Codex evalutes whether the request is legitimate or not, reproduces it before fixing it, and the actual code is scanned for security issues by Codacy and CodeScene. The combined judgment of all these steps *vastly* surpasses my own abilities and, in any case, fixes are not promoted to *stable* until I personally go through them.

***

## 🖥️ Coding

I will publish a separate newsletter article about the coding workflow, but in the meantime you can read [last month's update](https://refactoring.fm/p/updates-to-my-ai-coding-workflow), which is still 80% correct.

One thing that surprises many people is that I run only one agent at a time. It's a single instance of Codex that goes through the Open queue and does tasks one by one. It lives on my Mac Mini (together with OpenClaw) so it doesn't hog my Macbook, from which I work.

I work this way because I find that this already produces more than enough material to max out my* *ability to review tasks and produce new ones.

By running an agent 24 hours, and assuming an avg time of ~40 min to complete a task (including 10 mins of CI), this means every day I need to:

- Review between 30 and 40 pieces of work
- Create just as many to keep the run going

It's just a ton of work already.

Occasionally, I may use Codex on my Macbook do specific work that I want to get done immediately, without polluting the queue. Might be a specific bug fix, or tweaking a feature in review. But this happens kinda rarely, and accounts for probably ~5% of the work.

***

## 🔍 Validation

So are the pundits (like myself) right? Is *validation* the new bottleneck? I think it depends, but I surely spend a lot of time on it.

Every task that gets completed by the AI is shipped in a new alpha version and moved to the "in review" section, where I review it against my production vault.

Here, a few things may happen:

- **It's good** — I move the task to "to release"
- **It's ~90% good** — I do the final tweaks from my Macbook's Codex
- **Needs work, but it's still good progress** — this is the trickiest to decide. If it's incomplete but still useful, I have a bias towards *progress *and I release it anyway, creating a separate task for the improvements.
- **Wrong or not usable** — this is where the task is sent "to rework", with a QA comment about what needs to be fixed.

I do a big batch of reviews first thing in the morning, for tasks that were done overnight, and then usually again after lunch, and before dinner. During the one before dinner I also try to make sure the AI has enough to do for the night, but who am I kidding, these days I am also checking before going to bed 🫠

So today validation indeed feels like the biggest bottleneck, also because I believe the right course of action for most features is to keep specs light and let the AI figure out stuff by itself. At least this feels true for Tolaria, where a ton of things are well documented (ADRs, principles, vision, workflows) so the AI gets a lot right even with little input.

Also, for most features, even when the first pass is not *exactly* how I wanted it, there is an argument to be made that if it's good progress it should be released anyway, and have users chip in and give feedback. This has worked well many times, and evolved the product in the right direction faster than if I had waited for a perfect first version.

Of course nothing of this is exactly new thinking (MVPs, etc), but I think what's changing here is the *magnitude*. I find that the right course of action is constantly to release a bit more control than what I am comfortable with, and this boundary keeps moving.

***

## 🚚 Release

In Tolaria, there are two release channels: *alpha* and *stable*.

- **Alpha releases** — are created for literally every commit, and are designed for me to test new things.
- **Stable releases** — are those meant for final users, where all the work has been reviewed and *stamped*.

(for anyone who likes to live dangerously, they can choose what release channel to be subscribed to in the settings)

So, for any given day, there are usually tens of alpha releases, and at most one stable release. I name stable releases as days—e.g. v2026.5.4—breaking semver semantics a bit. I copied this from the OpenClaw repo because I found it very clear.

![BlockNote image](/Users/luca/Workspace/refactoring-vault/attachments/1777972833086-CleanShot_2026-05-05_at_11.03.57_2x.png)

The *release* procedure is managed completely by OpenClaw, to which I only say "*promote to stable*". This involves three main operations:

- **Git work** — performing the relevant git commands
- **Creating human-readable release notes** — as a separate md file that gets dynamically loaded in the [release page](https://refactoringhq.github.io/tolaria/)
- **Cleaning up** — dealing with the aftermath.

Cleaning up is particularly heavy, as it involves going after all the original tasks in the various channels (Github issues, Canny, Sentry errors) and closing them with a comment that explains that this is now available in the latest release.

Sentry issues are also closed optimistically, and a task is created again if an issue surfaces again in the latest release.

***

## 📊 Analytics

I use two analytics products:

- **Sentry** — for tech issues
- **PostHog** — for product analytics

They are both *opt-in*, meaning that as a user you need to explicitly accept sending these from the app. Events and logs are also sanitized and anonymized, so no content from your actual vault gets sent. No note names, paths, actual md, etc.

I use Sentry as explained above, and I use PostHog to track broad adoption trends.

***

## 📌 Bottom line

This is it! Overall, this workflow is optimized to let me spend my time only on the highest leverage tasks. Right now, it feels these are about deciding the following:

1. **What should be built next** — prioritizing product items.
2. **How a feature should be built** — largely from a product perspective.
3. **What's good enough** — whether an implemented feature is good enough to be shipped, or needs rework.

So far, it seems to work:

- Adoption grows without issues also growing
- Issues are closed fast. Will try to calculate* how fast* on average — I suspect it's within 2 days.
- People get timely feedback on what they report
- I am stretched out but not completely overwhelmed

Things change quickly though, and will keep you posted about this process!

And that's it for today! I wish you a great week

Sincerely 👋\
\
Luca
