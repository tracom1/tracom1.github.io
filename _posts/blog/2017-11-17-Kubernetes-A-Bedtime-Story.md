---
layout: post
title: "Kubernetes - A Bedtime Story - Part 1"
excerpt: "I'm being buried alive by webpages"
categories: blog
date: 2017-11-17T02:00:00-06:00
---

<center>
<a data-flickr-embed="true"  href="https://www.flickr.com/photos/stephen-oung/5732917385/in/photolist-9JAGBc-4sJY58-f7FLKj-6utd7E-oZp9Kw-CN8trq-7k6Kmu-ahYpXr-514RxT-889mqu-7sDnRF-2vVkpu-tiR5Un-6Sgmk9-kqpcEr-nR53XB-4ABYyq-7r1AZJ-8cPDHL-aNLmv6-fmZ83G-7mfLm2-8xBoKb-aaTeid-pydDzb-o5UQna-WHYdrj-faSqeg-6UjWVb-bu2eao-T56cjY-8nX15C-bu2eqh-8Zcv56-6FerxM-oXNMbM-as9QWk-6Rmc2Q-fu4UrA-5aTKED-oXJLkM-okH8sA-bJVq3M-692nZV-4AC1nW-5mXNoS-EJU8a-9yDfGT-aaMyUQ-dJu49c" title="The Conductor"><img src="https://farm6.staticflickr.com/5150/5732917385_ef6e983da3.jpg" width="500" height="334" alt="The Conductor"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>
</center>

I'm in the process of doing some kubernetes at work -- something nobody has done before in the company.  And I'm doing it in AWS -- something I've never really worked with before.  So I've been spending a lot of time doing "deep research", namely, reading random pages on the internet -- thousands and thousands of posts and pages about how to do Kubernetes.

There are a lot of really good blog posts about how to set up a kubernetes cluster (either locally or on AWS using kops, an AWS command line tool), mostly involving nginx or a hello-world app, so I'm going to skip the step-by-step instructions.  Instead, I'm going to concatenate some of the things I've learned from these pages, and demonstrate some tips and tricks I wish I had known earlier.

In this first part, I will do a brief summary of what kubernetes is for those whom are unfamiliar with (and don't care about) infrastructure.

<center>
*****
</center>

<center><img src="http://staticr1.blastingcdn.com/media/photogallery/2017/5/16/660x290/b_586x276/once-upon-a-time-every-good-story-starts-with-once-upon-a-flickr-flickrcom_1331623.jpg"></center>

Once upon a time, in a land not so far away, lived a bunch of unhappy developers.  These developers were so unhappy because they worked long hours every day.  All day and into the night they worked developing code and fixing bugs.  And while they loved their Cobol and their Pascal and their Java, they didn't love other parts of their job.  You see, developers were under the laws and whims of the 'evil' System Administrators.

<center>
<iframe src="https://giphy.com/embed/xl5QdxfNonh3q" width="480" height="269" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/mike-korea-north-xl5QdxfNonh3q">via GIPHY</a></p></center>

Their System Administrators enforced 'unfair' rules about how their applications were deployed to production. They could only use certain languages, and not new ones like NodeJS or Python or Cotlin.  They had to bundle together their code into 'releases' or 'executables', like an rpm or tarball (not as much fun as it sounds). And then, they were subjected to the whims of their System Administrator overlords for when their beautiful code would make it into production.  And before that, they had to trust that their evil System Administrator even knew what they were doing!  Sometimes the Administrators would take so long delivering their code, having to make and compile the code from source <b><i>by themselves!</i></b>  Who can trust them to do things right?

<center>
<a data-flickr-embed="true"  href="https://www.flickr.com/photos/tomitapio/2122390433/" title="Ohnoes!"><img src="https://farm3.staticflickr.com/2218/2122390433_669c243174_m.jpg" width="240" height="240" alt="Ohnoes!"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script></center>


Plus, the developers had to worry about what existed on the servers where these applications were being placed -- a place where they didn't have rights to place anything!  They didn't like having to conform to certain major/minor versions of java, or depending on a SysAdmin to have installed the package they NEEDED to get their app to run.  And so they raged.

<center>
<a data-flickr-embed="true"  href="https://www.flickr.com/photos/brand0con/4947938736/" title="IMG_6074-a"><img src="https://farm5.staticflickr.com/4146/4947938736_206df0429f_m.jpg" width="240" height="237" alt="IMG_6074-a"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script></center>

Then one day (in 2012 or so), a brave man by the name of <a href="https://en.wikipedia.org/wiki/Docker_(software)">Solomon Hykes</a> decided that there was a better way!  Developers shouldn't have to rely on other people, they should be able to have their code, and all their library or Operating System dependencies bundled with their app!  In fact, Developers shouldn't have to rely on having to use a standard Operating System either!  Everything should be customizable and conformable based on what the developer wanted, and what tool was deemed best for the job.

So he created Docker containers.

And the world said:

<center><iframe src="https://giphy.com/embed/3oEjI5VtIhHvK37WYo" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/hollywoodsuite-80s-eddie-murphy-beverly-hills-cop-3oEjI5VtIhHvK37WYo">via GIPHY</a></p></center>

Multiple docker containers could run on one bare-metal server (or cloud server if you didn't want to pay for the hardware), and could be configured however you wanted it.  You could build them with just enough for your app to run -- and no more.  You could configure them to look however you wanted, as big or as small as you needed.  And, you could version them so the System Administrator simply had to put it on the server, and he could put it on any type of server he wanted, it didn't matter (as long as it was Linux).  

And developers thought this would solve all their problems. So they celebrated.

<center><img src="https://wallpaperscraft.com/image/funny_celebration_humor_animals_80607_400x300.jpg"><figcaption><a href="https://wallpaperscraft.com/image/funny_celebration_humor_animals_80607_400x300.jpg">Source: Wallpaperscraft.com</a></figcaption></center>

Then.

System Administrators came back to developers asking, "How can I manage all these different types of things?  You want me to deploy 6 containers of app A and four containers of app B and anywhere between two and ten of app C.  And all on the same server!  How can I possibly control all these?!  It's going to take forever for me to do this!"

And the developers began to despair, because this wasn't going to solve their problems.

And then the great Google in the sky appeared.

<center><img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRBvAV49g9kNPnLa_B_F3Tp-LyB3cRC40i7apaOxPU9OqHQm-3e8w"></center>

And it thundered back, "We Automate It!"

And <a href="https://en.wikipedia.org/wiki/Kubernetes">Kubernetes</a> was born in its first iteration.

The Google God explained, "You see, Kubernetes is an tool that will handle all your containers automatically." (once you configure it), it said as an aside.  "It's fully customizable and will manage all the containers for you.  It will help add containers for applications as you need them, and remove them when you don't.  It will also manage all the communication between your applications within the tool!"

"But, how will it be configured?" the System Administrators asked.

"Ah," said Google. "It will use a master-slave configuration, so you will never forget how you enslaved developers to your system standards and deploy schedule."

The developers laughed.

"Now developers can use whatever they want.  And changing to a new version is so simple (once you have it configured correctly), that they can deploy it whenever they want.  This way they can fully automate their Pipeline and have Continuous Delivery!"

<center><iframe src="https://giphy.com/embed/bSJB8Ju3063qU" width="480" height="360" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/cheezburger-chocolate-candy-bSJB8Ju3063qU">via GIPHY</a></p></center>

"But our power!," the System Administrators exclaimed.

"And as punishment for making developers comply to a standard set of tools and libraries, everybody will tell you how easy Kubernetes is to set up.  Of course, it will be a lie.  But all System Administrators will forget about the burden of setting it up once it is working, and proclaim the news of its ease to the world."

And with that, the Google God asked developers for collaboration help so kubernetes could have more and more tools over more and more iterations.

And then System Administrators were forced to learn development, and to integrate more and more tools into their environments to manage things like IAM, Security, Federation, and accessing Private Docker Registries from inside their cluster.  Leaving developers free to do whatever they wanted.

<center>
<img src="https://imgs.xkcd.com/comics/compiling.png"><figcaption><a href="https://xkcd.com/303/">XKCD: Compiling</a></figcaption></center>

The End.