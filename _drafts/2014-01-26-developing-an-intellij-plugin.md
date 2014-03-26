---
layout: post
category: java
---

So; I come to Intellij plugin development a semi-veteran and refugee from the
acrid hell of Eclipse plugin development (please stop me if I'm overdoing
this). I can authoritatively say Intellij plugins are ok; not great, but if
you want to make something, you *can* realise your goal without triggering
a serious nervous breakdown; Intellij > Eclipse, QED.

I started with a tough one, a new Run Configuration, and within a couple of
days I'd got pretty much what I wanted. I found Intellij has a much more
complex workspace model than Eclipse, and that the documentation, while
at a glance seeming good, is often old and misleading (which is worse than
none).

I'll be breaking this into three parts, because I hear the Internet likes
that. Here, I'm going to cover setting up a workspace, in part 2 I'll cover
a simple Run Configuration, and in part 3 I'll show how I hooked in the
behaviour I wanted.

### Setting up a development environment

You can see what jetbrains has to say on the subject [over at their wiki](
http://www.jetbrains.org/display/IJOS/Writing+Plug-ins
).
I suppose it depends on how you feel about following unnecessary directions.
I prefer to set up using the following steps:

1. Unless you already have Intellij installed, [download it here](http://www.jetbrains.com/idea/download/).
2. Check out the source: `git clone --depth 1 git://git.jetbrains.org/idea/community.git idea`
3. Install an independent version of Intellij Community Edition. Things you
alter while testing won't affect your development environment this way.
4. Start the Intellij instance you will use for development, and open a new
project. Locate the sources you downloaded from git, and `File >
Import > Create project from existing sources` and select the `samples` directory.
5. Configure the SDK...
6. `Modules > Sources` and add the sources checked out of git. You should be
able to step into the Intellij code, which is often helpful.
6. Run the Actions, HelloWorldAction plugin. Intellij CE should open.

If this works, you can use the same process to set up a new plugin development
project.
