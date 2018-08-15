# The Cathedral and the Bazaar Book
  
  Eric Steven Raymond cf text and copyright at: www.tuxedo.org/~esr/writings

  This book is in public domain and his author is Eric Steven Raymond, this repository hosts the original version of the book, written in Markdown and hosted with :heart: at GitHub :octocat:
  
  Contribute with the project by translating and improving the formating by opening a **Pull Request**.

## Abstract

  _I anatomize a successful open-source project, fetchmail, that was run as a deliberate test of some surprising theories about sofware engineering suggested by the history of Linux. I discuss these theories in terms of two fundamentally diferent development styles, the “cathedral” model of most of the commercial world versus the “bazaar” model of the Linux world. I show that these models derive from opposing assumptions about the nature of the sofware-debugging task. I then make a sustained argument from the Linux experience for the proposition that “Given enough eyeballs, all bugs are shallow”, suggest productive analogies with other self-correcting systems of selfish agents, and conclude with some exploration of the implications of this insight for the future of software._

## 1 - The Cathedral and the Bazaar

  Linux is subversive. Who would have thought even five years ago (1991) that a world-class operating system could coalesce as if by magic out of part-time hacking by several thousand developers scattered all over the planet, connected only by the tenuous strands of the Internet?
  
  Certainly not I. By the time Linux swam onto my radar screen in early 1993, I had already been involved in Unix and open-source development for ten years. I was one of the first **gnu** contributors in the mid-1980s. I had released a good deal of open-source software onto the net, developing or co-developing several programs (nethack, Emacs’s vc and gud modes, xlife, and others) that are still in wide use today. I thought I knew how it was done.
  
  Linux overturned much of what I thought I knew. I had been preaching the Unix gospel of small tools, rapid prototyping and evolutionary programming for years. But I also believed there was a certain critical complexity above which a more centralized, a priori approach was required. I believed that the most important sofware (operating systems and really large tools like the Emacs programming editor) needed to be built like cathedrals, carefully crafted by individual wizards or small bands of mages working in splendid isolation, with no beta to be released before its time.
  
  Linus Torvalds’s style of development – release early and often, delegate everything you can, be open to the point of promiscuity – _came as a surprise. No quiet, reverent cathedral-building here_ – rather, the Linux community seemed to resemble a great babbling bazaar of differing agendas and approaches (aptly symbolized by the Linux archive sites, who’d take submissions from anyone) out of which a coherent and stable system could seemingly emerge only by a succession of miracles.
  
  The fact that this bazaar style seemed to work, and work well, came as a distinct shock. As I learned my way around, I worked hard not just at individual projects, but also at trying to understand why the Linux world not only didn’t fly apart in confusion but seemed to go from strength to strength at a speed barely imaginable to cathedral-builders.
  
  By mid-1996 I thought I was beginning to understand. Chance handed me a perfect way to test my theory, in the form of an opensource project that I could consciously try to run in the bazaar style. So I did – and it was a significant success.
  
  This is the story of that project. I’ll use it to propose some aphorisms about effective open-source development. Not all of these are things I first learned in the Linux world, but we’ll see how the Linux world gives them particular point. If I’m correct, they’ll help you understand exactly what it is that makes the Linux community such a fountain of good software – and, perhaps, they will help you become more productive yourself.
  
## 2 - The Mail Must Get Through
  
  Since 1993 I’d been running the technical side of a small _free-access_ Internet service provider called Chester County InterLink (_ccil_) in West Chester, Pennsylvania. I co-founded _ccil_ and wrote our unique multiuser bulletin-board software – you can check it out by telnetting to ```locke.ccil.org```. Today it supports almost three thousand users on thirty lines. The job allowed me 24-hour-a-day access to the net through _ccil’s_ 56K line – in fact, the job practically demanded it!
  
  I had gotten quite used to instant Internet email. I found having to periodically telnet over to _LOCKE_ to check my mail annoying. What I wanted was for my mail to be delivered on snark (my home system) so that I would be notified when it arrived and could handle it using all my local tools.
  
  The Internet’s nativemail forwarding protocol, _smtp_ (SimpleMail Transfer Protocol), wouldn’t suit, because it works best when machines are connected full-time, while my personal machine isn’t always on the net, and doesn’t have a static _ip_ address. What I needed was a program that would reach out overmy intermittent dialup connection and pull across my mail to be delivered locally. I knew such things existed, and that most of them used a simple application protocol called _pop_ (Post O?ce Protocol). _pop_ is now widely supported by most common mail clients, but at the time, it wasn’t built-in to the mail reader I was using.

  I needed a _pop3_ client. So I went out on the net and found one. Actually, I found three or four. I used one of them for a while, but it was missing what seemed an obvious feature, the ability to hack the addresses on fetched mail so replies would work properly.

  The problem was this: suppose someone named ‘joe’ on _LOCKE_ sent me mail. If I fetched the mail to snark and then tried to reply to it, my mailer would cheerfully try to ship it to a nonexistent ‘joe’ on snark. Hand-editing reply addresses to tack on ‘@ccil.org’ quickly got to be a serious pain.
  
  This was clearly something the computer ought to be doing for me. But none of the existing pop clients knew how! And this brings us to the first lesson:
  
  ```
# 1 Every good work of software starts by scratching a developer’s personal itch.
  ```
  
  Perhaps this should have been obvious (it’s long been proverbial that “Necessity is the mother of invention”) but too often software developers spend their days grinding away for pay at programs they neither need nor love. But not in the Linux world – which may explain why the average quality of software originated in the Linux community is so high.
  
  So, did I immediately launch into a furious whirl of coding up a brand-new pop3 client to compete with the existing ones ? Not on your life! I looked carefully at the pop utilities I had in hand, asking myself “which one is closest to what I want?” Because
  
  ```
  # 2 Good programmers know what to write. Great ones know what to rewrite (and reuse).
  ```
  
  While I don’t claim to be a great programmer, I try to imitate one. An important trait of the great ones is constructive laziness. They know that you get an A not for effort but for results, and that it’s almost always easier to start from a good partial solution than from nothing at all.
  
  **Linus Torvalds**, for example, didn’t actually try to write Linux from scratch. Instead, he started by reusing code and ideas from Minix, a tiny Unix-like operating system for _pc_ clones. Eventually all the Minix code went away or was completely rewritten – but while it was there, it provided scaffolding for the infant that would eventually become Linux.
  
  In the same spirit, I went looking for an existing _pop_ utility that was reasonably well coded, to use as a development base.
  
  The source-sharing tradition of the Unix world has always been friendly to code reuse (this is why the _gnu_ project chose Unix as a base os, in spite of serious reservations about the os itself). The Linux world has taken this tradition nearly to its technological limit; it has terabytes of open sources generally available. So spending time looking for some else’s almost-good-enough is more likely to give you good results in the Linux world than anywhere else.
  
  And it did for me. With those I’d found earlier, my second search made up a total of nine candidates – fetchpop, PopTart, get-mail, gwpop, pimp, pop-perl, popc, popmail and upop. The one I first settled on was ‘fetchpop’ by Seung-Hong Oh. I put my header-rewrite feature in it, and made various other improvements which the author accepted into his 1.9 release.
  
  A few weeks later, though, I stumbled across the code for ‘popclieznt’ by Carl Harris, and found I had a problem. Though fetchpop had some good original ideas in it (such as its backgrounddaemon mode), it could only handle _pop3_ and was rather amateurishly coded (Seung-Hong was at that time a bright but inexperienced programmer, and both traits showed). Carl’s code was better, quite 5 professional and solid, but his program lacked several important and rather tricky-to-implement fetchpop features (including those I’d coded myself).

  Stay or switch ? If I switched, I’d be throwing away the coding I’d already done in exchange for a better development base.
  
  A practical motive to switch was the presence ofmultiple-protocol support. pop3 is the most commonly used of the post-o?ce server protocols, but not the only one. Fetchpop and the other competition didn’t do pop2, rpop, or apop, and I was already having vague thoughts of perhaps adding **imap** (Internet Message Access Protocol, the most recently designed and most powerful post-office protocol) just for fun.
  
  But I had a more theoretical reason to think switching might be as good an idea as well, something I learned long before Linux.
  
  ```
  # 3 “Plan to throw one away; you will, anyhow.” (Fred Brooks, “The Mythical Man-Month”, Chapter 11)
  ```
  
  Or, to put it another way, you often don’t really understand the problem until after the first time you implement a solution. The second time, maybe you know enough to do it right. So if you want to get it right, be ready to start over at least once [jb].
  
  Well (I told myself) the changes to fetchpop had been my first try. So I switched.
  
  After I sent my first set of popclient patches to Carl Harris on 25 June 1996, I found out that he had basically lost interest in popclient some time before. The code was a bit dusty, with minor bugs hanging out. I had many changes to make, and we quickly agreed that the logical thing for me to do was take over the program.
  
  Withoutmy actually noticing, the project had escalated. No longer was I just contemplating minor patches to an existing pop client. I took on maintaining an entire one, and there were ideas bubbling in my head that I knew would probably lead to major changes. In a software culture that encourages code-sharing, this is a natural way for a project to evolve. I was acting out this principle:
  
  ```
  # 4 If you have the right attitude, interesting problems will find you.
  ```
  
  But Carl Harris’s attitude was even more important. He understood that
  
  ```
  # 5 When you lose interest in a program, your last duty to it is to hand it o: to a competent successor.
  ```
  
  Without ever having to discuss it, Carl and I knew we had a common goal of having the best solution out there. The only question for either of us was whether I could establish that I was a safe pair of hands. Once I did that, he acted with grace and dispatch. I hope I will do as well when it comes my turn.
  
## 3 The Importance of Having Users

  And so I inherited popclient. Just as importantly, I inherited popclient’s user base. Users are wonderful things to have, and not just because they demonstrate that you’re serving a need, that you’ve done something right. Properly cultivated, they can become co-developers.
  
  Another strength of the Unix tradition, one that Linux pushes to a happy extreme, is that a lot of users are hackers too. Because source code is available, they can be _effective_ hackers. This can be tremendously useful for shortening debugging time. Given a bit of encouragement, your users will diagnose problems, suggest fixes, and help improve the code far more quickly than you could unaided.
  
  ```
  # 6 Treating your users as co-developers is your least-hassle route to
rapid code improvement and e:ective debugging.
  ```
  
  The power of this effect is easy to underestimate. In fact, pretty well all of us in the open-source world drastically underestimated how well it would scale up with number of users and against system complexity, until Linus Torvalds showed us differently
  
  In fact, I think Linus’s cleverest and most consequential hack was not the construction of the Linux kernel itself, but rather his invention of the Linux development model. When I expressed this opinion in his presence once, he smiled and quietly repeated something he has often said: “I’m basically a very lazy person who likes to get credit for things other people actually do.” Lazy like a fox. Or, as Robert Heinlein famously wrote of one of his characters, too lazy to fail.
  
  In retrospect, one precedent for the methods and success of Linux can be seen in the development of the gnu Emacs Lisp library and Lisp code archives. In contrast to the cathedral-building style of the Emacs C core and most other _gnu_ tools, the evolution of the Lisp code pool was fluid and very user-driven. Ideas and prototype modes were often rewritten three or four times before reaching a stable final form. And loosely-coupled collaborations enabled by the Internet, a la Linux, were frequent.
  
  Indeed, my own most successful single hack previous to fetchmail was probably Emacs vc (version control) mode, a Linux-like collaboration by email with three other people, only one of whom (Richard Stallman, the author of Emacs and founder of the **Free Software Foundation**) I have met to this day. It was a front- end for sccs, rcs and later cvs from within Emacs that oftered “one-touch” version control operations. It evolved from a tiny, crude sccs.el mode somebody else had written. And the development of vc succeeded because, unlike Emacs itself, Emacs Lisp code could go through release/test/improve generations very quickly.
