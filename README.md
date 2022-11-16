# CUBE CSS with Tailwind CSS

⚠️ **THIS IS VERY MUCH A PROTOTYPE AND LIKELY BROKEN** ⚠️

👀 **DEMO: <https://cube-css-with-tailwind.netlify.app/>** 👀

📝 **MORE INFO [I wrote up about this whole thing here](https://piccalil.li/blog/i-used-tailwind-for-the-u-in-cube-css-and-i-liked-it/)**

An example of how you can use [Tailwind CSS](https://github.com/tailwindlabs/tailwindcss) as the U (utilities) in
[CUBE CSS](https://cube.fyi/). It also uses the following:

1. [Every Layout](https://every-layout.dev/) layouts as compositional layouts (the C in CUBE CSS)
2. [Utopia](https://utopia.fyi/) for fluid type and spacing scales
3. [Post CSS](https://postcss.org/) to bundle up CSS files

## Why?

I work with clients that like to use Tailwind and I, as part of the
consultancy work I do, try to encourage them down the path of a CSS
methodology, rather than leaning all-in on a CSS framework.

Tailwind is _fantastic_ at generating utility classes with an
easy-to-understand config file, but I find it very limiting and rigid as the
entire CSS codebase, so this project uses it how I would ideally use it:
purely as the utility generator.

Consider this project diplomacy.

## Getting started

1. Run `npm install`
2. Run `npm start` to spin up a little local server and watch for CSS changes
3. Run `npm build` to compress CSS etc

## Contributing

No thanks, but if you see something fundamentally wrong (not just your
opinions, thanks), then let me know in the issues!



I used Tailwind for the U in CUBE CSS and I liked it
28th of January 2022
Categories
CSS
A client project requirement meant I made a CUBE CSS with Tailwind prototype that actually turned out really well.
I’ve recently started a new client project where there’s a few developers involved. These developers vary in speciality and I of course, specialise in design implementation—what the folks at Clearleft describe accurately as Design Engineering.
With that, I’ve mostly taken charge of the CSS stuff and naturally suggested CUBE CSS, Every Layout and a fluid type and space system, powered by Utopia. This landed well with the developers, but there was an ask: “Can we use Tailwind, because we’ve used that for the last couple of projects”.
Normally, I’ll push hard on that sort of ask, because from lots of experience: very often when teams lean heavily into a framework, it accumulates a lot of expensive technical debt. But, a combination of a tight timeline and a need to practice what I preach—prototypes, prototypes, prototypes—I suggested that Tailwind should be part of the U in CUBE CSS for this project and I went ahead and made a prototype to test that approach.
This article is a report of sorts on that prototype and using Tailwind like this (and generally, I guess).
Why I didn’t want us to use Tailwind for everything permalink
A big part of how I structure CSS—with CUBE CSS—is to reduce how much CSS is written and maintained over time. It’s also very important to present the browser with rules and systems that let them do the work, rather than micro-managing them to render exactly what you think the design has to look like.
This is firstly antithetical to the web as a whole: it’s a flexible medium that is open an accessible to anyone with an internet connection and a browser. With that, the sheer number of different devices, connection speeds, broken devices, dodgy software and dodgy connections is eye-watering. Writing very specific rules in that context seems a bit silly doesn’t it? I certainly think so. That’s why I aim to make things as flexible as possible with what I produce.
In my opinion, this is difficult when using Tailwind as the central, standalone CSS codebase. Sure, you can gain a lot of consistency with utility-first tools like Tailwind and Tachyons. Heck, Nicole Sullivan has been teaching us all to split out CSS like this since around 2009 with OOCSS. I even used to write extremely rigid CSS with BEM, mainly because browser compatibility was shocking last decade, so I wanted to make my code as safe as possible. This isn’t the case now, nor has it been for at least half a decade, so tools that write so-called “safe” CSS are much less useful than they were previously.
I look at this a bit how I look at CSS layout. For years we hacked layout together with floats, display: table etc. and it worked pretty well. Later on though, when Flexbox and Grid arrived, the need to hack layout together was redundant. That’s how I see super specific CSS now: because browser compatibility is stable and CSS is so much more powerful now, we can trust it do deliver truly stunning designs to everyone by writing it flexibly with progressive enhancement.
The proposed project setup permalink
With that context and back-story out of the way, let’s get into the meat and taters of this article. The prototype’s CSS structure is as follows:
CUBE CSS as the overarching methodology
Every Layout layouts providing the C in CUBE
Tailwind utilities providing the U in CUBE
Utopia providing a fluid type scale and fluid space scale
On paper this looks like a real mashup, but aside from using Tailwind (I’ve been using Gorko for utilities), this is how I’ve been writing CSS for a couple of years now and it’s been very successful. A recent example of this is that it powers the web.dev front-end and design system.
A challenge for me in this project is that if we’re going to run with this approach, it’s looking we can’t really use Sass, which I am a fan of for keeping my CSS nice and organised. The way I see it though is that you need to meet each other in the middle in these situations, so I’ll pass-up Sass, just like the other developers are passing-up using Tailwind for everything.
What I want to really avoid is the PostCSS pretending to be Sass setup—which is very possible—but oh-so-fragile. I’ve had awful experiences with this too. Instead, I’ll be recommending that we lean into CSS Custom Properties. We could use Tailwind’s @apply to apply utilities in our components (Blocks in CUBE CSS), but that in my opinion is some potentially expensive technical debt. Mainly because I can already see issues with how Tailwind might clash with upcoming CSS features, such as @layer.
In the space of very little time, I managed to roll-out a little generator that takes Tailwind config groups and generates CSS Custom Properties. This is a good start and could rather easily create a solid system for creating reusable tokenised CSS—even if—or even, when—Tailwind is no longer preferable to the wider team.
So now, at this point, everything has a place. I like how adding a fluid scale was effortless in the Tailwind config too. That makes things much easier.
The CSS directory structure looks like this now:
Code language
TEXT
COPY TO CLIPBOARD
src
└── css
    ├── blocks
    ├── compositions
    ├── utilities
    ├── cube.css
    ├── custom-props.css (generated)
    ├── global.scss
    └── reset.scss
Notice that there is a utilities directory. This is because—as per CUBE CSS—there are utilities that do one job and do that job well. The .visually-hidden utility is a great example of that. It just visibly hides content while making it available to assertive technology users, such as screenreader users.
Rather annoyingly, because PostCSS imports need to be at the top of a file, I had to import blocks, compositions and utilities in a separate cube.css file. This was because I wanted the Tailwind utilities to render last and the other partials to come after the global CSS, so I can lean into the cascade. Any tips on how I could not have to do that would be appreciated!
Things I like and dislike about Tailwind permalink
One thing about Tailwind that I found super frustrating was that it took me ages to get up and running with it. This is often the case with tools that you don’t use a lot—I get that—and the problem with me is that I really didn’t want to use their whole config, which added time. That full config is massive. What would be nice would be a skeletal config that you can generate with the CLI. Something that just provides the bare basics. A bit like I do with Gorko.
A good example here is that the full config ships a lot of layout stuff—even bizarrely, Grid—which because we’ve got Every Layout and the Composition layer, we just don’t need them. I get that a lot of folks would find that stuff useful, but a middle-ground config without this stuff would be very helpful for others too.
On the subject of the config, I love that it’s JavaScript-based. Being able to manipulate data in place will undoubtedly make it useful when dealing with externally defined design tokens. This type of thing is just lush:
Code language
JAVASCRIPT
COPY TO CLIPBOARD
margin: ({theme}) => ({
  auto: 'auto',
  ...theme('spacing')
}),
The bit about the config I find extremely impressive and useful: the content section of the config. I piped the path to my little HTML page and bosh, it only generated the utilities I need. Versus what I’ve seen before—and talked about a lot—where Tailwind would generate a lot of cruft: this is a huge, positive step. Very nice work indeed, folks.
Lastly, the documentation site is excellent. I found nearly everything I needed, really easily. Well, it is fantastic, once you get past the frankly, outrageous copy on the homepage, such as this howler:
“Best practices” don’t actually work.
I don’t think I’ll ever stop side-eyeing that one 🥴
One thing that is frustrating about searching for Tailwind stuff on the web is that because it’s the Hot Framework Of The Moment™, there’s a lot of garbage articles out there, jumping on the hype train. That’s always the case, though, and I remember finding the same when searching React stuff, a few years ago, so I can’t really blame Tailwind for that.
Wrapping up permalink
All of the above resulted in a nice little prototype, which you can see here. You can also see the source code here. It’s super rough around the edges, and you’ll see where I was exploring using @apply in places, but it should hopefully give you an idea of where I’m at with this.
I’m very often accused of being a “Tailwind hater”, but the reality is that I see flaws in using it for everything and people don’t like that. I also vehemently dislike how it is marketed (like the above quote), which is often mistaken for “hate” of the whole project. I actually used Tailwind for a project way back in 2018 when it was a much younger tool, so it’s not like I haven’t had exposure to it to form my opinions, which from that experience, were not overly positive. That is less the case now, as I’ve just demonstrated.
The thing I do actually hate about Tailwind is that when someone criticises it, a swarm of petulant men-children harass them on social media. I don’t imagine that will ever change—I can’t see the Tailwind folks actually doing anything to counter that either (although I would love to be proven wrong). What Tailwind is though, is a tool that has evolved very nicely and in the context I’m using it for in this prototype—as a utility generator—it works an absolute charm.
It’s likely this won’t be the last time it meets CUBE CSS in my projects.
