+++
date = '2025-02-16T00:18:44-05:00'
author = 'Ben P'
title = 'Why I Made a Blog, Not a Portfolio'
description = 'Why did I create this site the way that I did?'
tags = ['Hugo', 'Self-Critique']
+++

{{< rawhtml >}}

<div class='gradient-keyboard'></div>
{{< /rawhtml >}}

## TL;DR

For a dev like me, a portfolio doesn't do the job of illuminating my value and could be a waste of time. Using a blog made with Hugo allows me to save time in development and provides a medium through which I can share what I know and what I'm doing in smaller, bite sized chunks.

## My portfolio disaster

One of the things I find facinating about life is how every iteration of my "self" views myself of 5 years ago as complete and utter "Cringe". I have to assume at least some of you know this feeling. If you haven't experienced it, go dig up your oldest social media posts or read an old journal...

My old portfolio brings me into that "Cringe" state. Portfolios are pretty common projects for junior and mid-level frontend engineers-- and I built one some time before I landed my first engineering job. [Here](https://benprothe-portfolio.netlify.app/) it is in all its unforunate glory. But this is all I had until I made this site (we'll get there). I did not feel comfortable using this portfolio in my job applications or even linking it in my other socials. I actually did start to update it so I'd have something I could link but I kept coming face to face with the issues I'll outline.

Before I conduct the autopsy, I wanted to explain what I hope to accomplish in doing so. I'm here to show what I've learned for both self-promotion and educational purposes. I think this is actually a pitfall other junior and mid-level devs could fall into and I want to fast track them towards a better path. As for self-promotion, a key skill for devs is the ability to evaluate one's self, without letting your ego get in the way. Its how we get better and learn from others. Its part of being teachable and likeable in a workplace. That's not a narrative you get to see on a portfolio site... but a blog? Well, I'm getting ahead of myself...

So, objectivity in hand, ego aside, here's what painfully sticks out to me today, 5 years later:

### Over-engineered

Importantly, in saying that this site is "over-engineered", I do not mean that the code quality is high. I mean that I'm using a sledge hammer to tap in finishing nails. This isn't obvious without a little digging, but the portfolio is a full React app. At the time my goal in using React for this was to demonstrate that I could... use React (I even put the portfolio, _in_ the portfolio :joy:).

But a portfolio just isn't a good medium with which to demonstrate skill in React. Don't get me wrong, it took plenty of time to create and I'd even say it was hard. But the difficulty had more to do with making design decisions that felt extra "heavy" because it was for self-promotion. It wasn't hard because the React was hard to write. It's a one page view. I didn't need state management. I didn't need to handle asychronous code. I didn't even really need a router.

The "portfolio problem" is very, very, very solved. Content management systems have existed for a long time and you just don't need React to create these kinds of personal sites.

{{< rawhtml >}}
<span style = "color:#969191; font-weight: bold;">
Takeaways: This iteration of my site should use existing tools optimized for content management, freeing up some time to work on other things. Speaking of which...
</span>
{{< /rawhtml >}}

### Project Quality

This line item is probably the most egregious. The most valuable projects (Flashcards and BevDev) were hosted on Heroku Free Tier (RIP) back in the day, so the links don't go anywhere. Even still, those were projects I completed during my bootcamp. They don't stand up next to what I can do today. All of the other projects listed could have easily come from a tutorial. Then there’s the “coming soon” category (I CAN PHYSICALLY FEEL THE PAIN OF THE CRINGE 😑)... I included projects that I hadn’t even started. I even provided filters for the technologies I would hypothetically use. It was bad.

What that tells me is that I had an empty grid and had to dig deep to fill it. Like, that's kind of the problem with being a Junior Dev... you don't have a wealth of things to show off. The projects you _do_ have to show off aren't impressive. Building something impressive is time intensive... if you omit filler content, your portfolio will be empty until you finish the good project. There's nothing in a portfolio to illuminate the things you learn in the process of building the "new thing". There's nothing to illuminate anything you might have learned along the way, but don't use in the final product. So it kind of doesn't make sense for a junior to have a portfolio at all. Why highlight an empty or unimpressive list of projects?

Now consider this... I just worked for 2 years on a very complex project and I don't even have a git-repo I can link to show off my work. I wasn't creating end-to-end projects. I was working on features and enhancements for an existing project. I learned so much, but I still don't have any new items for my portfolio grid. So a portfolio doesn't provide any way to document what I've been doing during employment. I have nothing to show.

In making these observations, my new opinion is that a portfolio is only an appropriate solution for freelance devs who have a bunch of tangible projects they can actually display. They actually need to be able to show their work, like a catalogue. Their projects are more finite and make for a nice, full grid.

{{< rawhtml >}}
<span style = "color:#969191; font-weight: bold;">
Takeaways: This iteration of my site needs to highlight smaller learnings and incremental progress on large projects. Everything on display should be quality and demonstrative; no filler. I also need to make sure projects don't Heroku'd.
</span>
{{< /rawhtml >}}

### Sterility

This point has more to do with my personal strengths. I am a good communicator. I love to share information with people as I learn, which helps everyone in a team setting. I can write documentation - I have training in technical writing and lots of experience writing things like SOPs and compliance audits. I could _tell_ you about this in an "about" section in my portfolio... but in a blog I can _show_ these qualities.
