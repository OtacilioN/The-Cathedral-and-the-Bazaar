# The Cathedral and the Bazaar Book
  
  Eric Steven Raymond cf text and copyright at: www.tuxedo.org/~esr/writings

  This book is in public domain and his author is Eric Steven Raymond, this repository hosts the original version of the book, written in Markdown and hosted with :heart: at GitHub :octocat: 

## Abstract

  _I anatomize a successful open-source project, fetchmail, that was run as a deliberate test of some surprising theories about sofware engineering suggested by the history of Linux. I discuss these theories in terms of two fundamentally diferent development styles, the “cathedral” model of most of the commercial world versus the “bazaar” model of the Linux world. I show that these models derive from opposing assumptions about the nature of the sofware-debugging task. I then make a sustained argument from the Linux experience for the proposition that “Given enough eyeballs, all bugs are shallow”, suggest productive analogies with other self-correcting systems of selfish agents, and conclude with some exploration of the implications of this insight for the future of so=ware._

## 1 - The Cathedral and the Bazaar

  Linux is subversive. Who would have thought even five years ago (1991) that a world-class operating system could coalesce as if by magic out of part-time hacking by several thousand developers scattered all over the planet, connected only by the tenuous strands of the Internet?
  
  Certainly not I. By the time Linux swam onto my radar screen in early 1993, I had already been involved in Unix and open-source development for ten years. I was one of the first **gnu** contributors in the mid-1980s. I had released a good deal of open-source software onto the net, developing or co-developing several programs (nethack, Emacs’s vc and gud modes, xlife, and others) that are still in wide use today. I thought I knew how it was done.
  
  Linux overturned much of what I thought I knew. I had been preaching the Unix gospel of small tools, rapid prototyping and evolutionary programming for years. But I also believed there was a certain critical complexity above which a more centralized, a priori approach was required. I believed that the most important sofware (operating systems and really large tools like the Emacs programming editor) needed to be built like cathedrals, carefully crafted by individual wizards or small bands of mages working in splendid isolation, with no beta to be released before its time.
  
  Linus Torvalds’s style of development – release early and often, delegate everything you can, be open to the point of promiscuity – _came as a surprise. No quiet, reverent cathedral-building here_ – rather, the Linux community seemed to resemble a great babbling bazaar of differing agendas and approaches (aptly symbolized by the Linux archive sites, who’d take submissions from anyone) out of which a coherent and stable system could seemingly emerge only by a succession of miracles.
  
  The fact that this bazaar style seemed to work, and work well, came as a distinct shock. As I learned my way around, I worked hard not just at individual projects, but also at trying to understand why the Linux world not only didn’t fly apart in confusion but seemed to go from strength to strength at a speed barely imaginable to cathedral-builders.
  
  By mid-1996 I thought I was beginning to understand. Chance handed me a perfect way to test my theory, in the form of an opensource project that I could consciously try to run in the bazaar style. So I did – and it was a significant success.
  
  This is the story of that project. I’ll use it to propose some aphorisms about effective open-source development. Not all of these are things I first learned in the Linux world, but we’ll see how the Linux world gives them particular point. If I’m correct, they’ll help you understand exactly what it is that makes the Linux community such a fountain of good software – and, perhaps, they will help you become more productive yourself.
