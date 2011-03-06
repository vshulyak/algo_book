Scalability
==============

In the course of reading lots of tech news, and occasionally hanging out with other hackers, I've come under the impression that there is some kind of intense, difficult black art to building large services that scale to 10^N number of users, for any impressive value of N.
I'm a dev of 20+ years who has worked on everything from games, to desktop apps, to web apps. I worked as a front-end web development lead at Microsoft on several small web apps, and as the back-end dev on a small web service.

I think I could get interviews at any number of interesting places, but if I was going for back-end work (the part that interests me), I feel like I would simply be out of my depth. How do I get there? I'm sure there are obvious answers like "read books", "learn on the job", or "try to build youtube/facebook/twitter, and succeed". So specifically, my question is for those who have already learned to build medium-to-large services: what was your path to acquiring these skills, and do you have any advice for people like me?

Ans1
-------

http://news.ycombinator.com/item?id=2290180

I'll share my experience, which may differ from other people's. The largest system that I've worked on was Amazon S3. At the time that I worked there we were doing 100,000+ requests per second (peak), storing 100+ billion objects (aka files), and growing our stored object count by more than double every year. The most important skills for that job were distributed system theory, managing complexity, and operations. I can't explain all of these skills in depth, but I will try to give you enough pointers to learn on your own.
For distributed systems there are two main things to learn from: good papers and good deployed systems. A researcher named Leslie Lamport invented a number of key ideas such as Lamport timestamps and Byzantine failure models. Some other basic ideas include quorums for replicated data storage and the linearizability consistency model. Google has published some good papers about their systems like MapReduce, BigTable, Dapper, and Percolator. Amazon's Dynamo paper was very influential. The Facebook engineering "notes" blog also has good content. Netflix has been blogging about their move to AWS.
Every software engineer needs to manage complexity, but there are some kinds of complexity that only show up in big systems. First, your system's modules wil be running on many different machines. The most important advice I can give is to have your modules separated by very simple APIs. Joshua Bloch has written a great presentation on how to do that. Think about what happens when you do a rolling upgrade of a 1,000 node system. It might take days to complete. All the systems have to interoperate correctly during the upgrade. The fewer, simpler interactions between components the better.
The best advice I know of about operating a big distributed system is this paper[1] by James Hamilton. I won't repeat its contents, but I can tell you that every time that we didn't follow its guidelines we ended up regretting it. The other important thing is to get really good with the Unix command line. You'll need to run ad-hoc commands on many machines, slice and dice log files, etc.
How did I learn these skills? The usual mix of how people learn anything - independent study, school, and building both experimental and production systems.

http://www.usenix.org/event/lisa07/tech/full_papers/hamilton/hamilton_html/
