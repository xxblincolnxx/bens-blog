+++
date = '2025-02-16T00:18:44-05:00'
author = 'Ben Prothe'
title = 'Why I Made a Blog, Not a Portfolio'
tags = ['Hugo', 'Personal Growth']
+++

{{< rawhtml >}}

<div class='gradient-keyboard'></div>
{{< /rawhtml >}}

## TL;DR

For a dev like me, a portfolio doesn't do the job of illuminating my value and could be a waste of time. Using a blog made with Hugo allows me to save time in development and provides a medium through which I can share what I know and what I'm doing in smaller, bite sized chunks.

## My portfolio disaster

One of the things I find facinating about life is how every iteration of my "self" views myself of 5 years ago as complete and utter "Cringe". I have to assume at least some of you know this feeling. If you haven't experienced it, go dig up your oldest social media posts or read an old journal...

My old portfolio brings me into that "Cringe" state. Portfolios are pretty common projects for junior and mid-level frontend engineers-- and I built one some time before I landed my first engineering job. [Here](https://benprothe-portfolio.netlify.app/) it is in all its unforunate glory. This is all I had until I made this site (we'll get there). I actually did start to update it but I kept coming face to face with the issues I'll outline.

{{< rawhtml >}}
<img align="right" src="/img/old_portfolio.png"  style="width: 400px; border-radius: .5em; margin: 10px;"/>
{{< /rawhtml >}}

Before I conduct the autopsy, I wanted to explain what I hope to accomplish in doing so. I'm here to show what I've learned for both self-promotion and educational purposes. I think this is actually a pitfall other junior and mid-level devs could fall into and I want to fast track them towards a better path. As for self-promotion, a key skill for devs is the ability to evaluate one's self, without letting your ego get in the way. It's how we get better and learn from others. It's part of being teachable and likeable in a workplace. That's not a narrative you get to see on a portfolio site... but a blog? Well, I'm getting ahead of myself...

So, objectivity in hand and ego aside, here's what painfully sticks out to me today, 5 years later:

### Over-engineering

Importantly, in saying that this site is "over-engineered", I do not mean that the code quality is high or complex. I mean that I'm using a sledge hammer to tap in finishing nails. This isn't obvious without a little digging, but the portfolio is a full React app. At the time my goal in using React for this was to demonstrate that I could... use React (I even put the portfolio, _in_ the portfolio :joy:).

But a portfolio just isn't a good medium with which to demonstrate skill in React. Don't get me wrong, it took plenty of time to create and I'd even say it was hard. But the difficulty had more to do with making design decisions that felt extra "heavy" because of the professional implications of each decision. It wasn't hard because the React was hard to write. This isn't a web-app-- it's a one page view. I didn't need state management. I didn't need to handle asychronous code. I didn't even really need a router.

The "portfolio problem" is very, very, very solved. Content management systems have existed for a long time and you just don't need React to create these kinds of personal sites.

{{< rawhtml >}}
<span style = "color:#969191; font-weight: bold;">
Takeaways: This iteration of my site should use existing tools optimized for content management, freeing up some time to work on other things. Speaking of which...
</span>
{{< /rawhtml >}}

### Project Quality

This line item is probably the most egregious. The most valuable projects (Flashcards and BevDev) were hosted on Heroku Free Tier (RIP) back in the day, so the links don't go anywhere. Even still, those were projects I completed during my bootcamp. They don't stand up next to what I can do today. All of the other projects listed could have easily come from a tutorial. Then there’s the “coming soon” category (I CAN PHYSICALLY FEEL THE PAIN OF THE CRINGE 😑)... I included projects that I hadn’t even started. I even provided filters for the technologies I would hypothetically use. It was bad.

Let's think in terms of "root cause". What realy happened is that I had an empty grid for projects and to find a way to fill it. Like, that's kind of the problem with being a Junior Dev... you don't have a wealth of things to show off. The projects you _do_ have aren't impressive. Building something impressive is time intensive... If you omit filler content, your portfolio will be empty until you finish the good project. There's nothing in a portfolio to illuminate the things you learn in the process of building the "new thing". So it kind of doesn't make sense for a junior to have a portfolio at all. Why highlight a weak spot?

Now consider this... I just worked as a full stack developer for 2 fruitful years on a very complex private project. Today, I don't even have a git-repo I can link to show off my work. I wasn't creating end-to-end projects. I was working on features and enhancements for an existing project. That's what actual development looks like unless you're a founder. I learned so much, but I still don't have any new line-items for my portfolio grid. Now the projects I do have pale in comparisson to what I am capable of now.

{{< rawhtml >}}
<span style = "color:#969191; font-weight: bold;">
Takeaways: This iteration of my site needs to highlight smaller learnings and incremental progress on large projects. Everything on display should be quality and demonstrative; no filler. I also need to make sure projects don't get Heroku'd.
</span>
{{< /rawhtml >}}

### Incomplete Profile

This point has more to do with my personal strengths. I am a good communicator. I love to share information with people as I learn, which helps everyone in a team setting. I can write documentation - I have training in technical writing and lots of experience writing things like SOPs and compliance audits. I could _tell_ you about this in an "about" section in my portfolio... but in a blog I can _show_ these qualities.

{{< rawhtml >}}
<span style = "color:#969191; font-weight: bold;">
Takeaways: This iteration of my site should demonstrate my softer skills that help differentiate me as a developer.
</span>
{{< /rawhtml >}}

---

My new opinion is that a portfolio is only an appropriate solution for freelance devs who have a bunch of tangible projects they can display. A freelancer needs to be able to show their work and their projects are more finite. They make for a nice, full grid of things to show off. I am not a freelance developer at this time. I like working on teams and have great collaboration skills. I'm not 100% convinced I even need a personal site. I _would_ still like one but I want to get away from the need to fill a grid of projects. So let's turn my takaways into a spec sheet for my next iteration on a personal site:

- I should use existing tools optimized for content management, not React.
- I need the ability to highlight smaller learnings and incremental progress on large projects.
- Content on display should be at least somewhat robust against depreciated hosting solutions like Heroku.
- The other things that make me a good developer and employee should have some kind of medium by which they can be displayed.

## The Path Forward

First and foremost, let me talk for a moment about open-source. I can hear you shouting from the rooftops that _this_ is the answer. I agree with you for the most part. But open-source takes time to ramp. You can start small with comments and issues but anything more is probably going to take some time and energy. That's actually a big commitment up front for an unpaid effort from a developer who has a family and real-life obligations. Thankfully, open-source and other options are not mutually exclusive.

A blog is where I landed for the _first_ of my next steps. Here you are reading it. These are the benefits I gain:

- I can showcase complete projects but also incremental progress and accrued learnings; I can talk about things that don't even make it into my projects.
- I used [Hugo](https://gohugo.io/) to generate the whole site. Its not over-engineered and is optimized as a static site for SEO and performance.
- I will be able to share cheatsheets and guides I already create for myself.
- If I take on open-source contribution, I can document what I learn as I learn it. Funny enough, I actually found a potential place where I can contribute in learning Hugo and utilizing pre-built themes.
- If i take a break to grind some leetcode, I can show what I learn here.
- When I do get my next job, I can still post things here without syphoning too much bandwidth or context switching.
- My entries can be as brief or exhaustive as I want them to be. Nothing actually has to be "Finished" to do so.
- My documentation and communication will be on full display. Obviously, I have no shortage of things to say :smile:.

I'm optimistic that this blog will be a good solution for me. I am skeptical it ever reach a broad audience, but thats totally okay! I'll have something tangible for my job applications and my LinkedIn. At the very least I'll have a fun little creative outlet. So that's it for this "hello world". On to the meat and potatoes.

-Ben
