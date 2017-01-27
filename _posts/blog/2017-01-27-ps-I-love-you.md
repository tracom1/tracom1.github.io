---
layout: post
title: "ps, I love you"
excerpt: "Advanced Usage of ps Command"
categories: blog
date: 2017-01-27T07:30:00-07:00
---

<iframe src="//giphy.com/embed/vrKNvQCPLdWms" width="480" height="122" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/ps-vrKNvQCPLdWms">via GIPHY</a></p>

I’m not really a fan of the <font color="cyan"><i>top</i></font> command.  

Now, before you start typing something up decrying my stance, let me say that it's a great command and provides a lot of useful knowledge.  It gives you access to information about your computer system quickly and informatively (e.g., memory usage, cpu usage, running processes)

However, it’s just too busy for me.  There’s so much going on!  And things changes so frequently; I mean predominantly because <font color="cyan"><i>top</i></font> provides a dynamic real-time view of your system, but still!  Again, I know can change the refresh rate by just going <font color="cyan"><i>top -d 30</i></font>, but that does not decrease the amount of characters on the screen.  It is true, <font color="cyan"><i>top</i></font> can do many, many things.

But I still prefer <font color="cyan"><i>ps</i></font>, because it’s awesome!  Let's discuss.


<h2>THE BASICS</h2>

Some people are familiar with the “basics” of <font color="cyan"><i>ps</i></font>.  The command can be used to provide a snapshot view of your computer system.  The commands most commonly used are <font color="cyan"><i>ps -elf</i></font> or <font color="cyan"><i>ps aux</i></font>, which tell you what processes your system is currently running.

If you need a refresher on what all the abbreviations and options mean, which I don't have the space to cover in this post, please refer to the <font color="cyan"><i>ps</i></font> <a href="https://linux.die.net/man/1/ps">man page</a> or simply type <font color="cyan"><i>man ps</i></font> on a Linux based machine.

I personally like to throw in the f in aux because it shows tree inheritance of a process, and because then I can make my command "fake":


```bash
-sh-4.2$ ps faux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0 125220  3712 ?        Ss   Jan08   1:01 /usr/lib/systemd/systemd --switche
root       341  0.0  0.3  46164 15336 ?        Ss   Jan08   0:14 /usr/lib/systemd/systemd-journald
root       366  0.0  0.0  43524  1836 ?        Ss   Jan08   0:00 /usr/lib/systemd/systemd-udevd
root       378  0.0  0.0  21372   672 ?        Ss   Jan08   0:00 /usr/sbin/rpc.idmapd
root     25320  0.0  0.1 169568  5104 ?        Ss   18:55   0:00  \_ sshd: tracy [priv]
tracy    25328  0.0  0.0 169568  2248 ?        S    18:55   0:00      \_ sshd: tracy@pts/0
tracy    25329  0.0  0.0 128232  2552 pts/0    Ss   18:55   0:00          \_ -sh
tracy    25385  0.0  0.0 161488  1924 pts/0    R+   18:58   0:00              \_ ps faux
```
Generally I use something like <font color="cyan"><i>ps -elf | grep java</i></font> to narrow down a process I’m looking for (or <font color="cyan"><i>ps -elf | grep [j]ava</i></font> to eliminate the grep from your results).  The grep will take all the processes currently running on a system and then only search for ones that match the subsequent string (or regex). 

For example:

```bash
[root@[host] ~]# ps -elf | grep java
0 S 1000      3152     1  0  85   0 - 452154 futex Jan08 ?        00:04:45 /opt/jre8/bin/java
0 R root     15641 15223  0  78   0 - 15300 -      19:00 pts/1    00:00:00 grep --color=auto java
```

These few commands cover roughly 90% of a person's usage.  But(!) we have some pretty strange situations that have called for some fancy finagling of this fabulous fiat.  A good reference for some more basic ps commanding can be found <a href="http://www.binarytides.com/linux-ps-command/">here!</a>


<h2>TIP AND TRICK #1 - IT'S GOT GOOD THREADS</h2>

As a system administrator, I generally set limits on how many threads any particular user can have (in /etc/security/limits.conf).  Not just because I want to prevent devs from having access to the resources they need <font color="red">(insert evil laugh here)</font>, but also because it's good practice to prevent applications from cobbling your computer system through hogging more and more of your limited resources.  If the value is not set, it will default to 1024.

Once, we ran into a situation where a bunch of our java apps were getting the error “Unable to create a new Native thread” on a server that runs multiple applications.

To determine if our errors were being generated as a result of our limits.conf,  I leveraged the -u <<username>> command along with some other ps *magic*.

```bash
[(root@[host[ ~]# ps -Led -o user | sort | uniq -c | sort -n
      1 ntp
      2 postfix
      2 tracy
     82 wwwrun
    142 root
   1306 runner_agent
```

This command finds all process results under a particular user,  sorts it so they can be properly "consolidated", groups them by unique user, and finally re-sorts.  The -o command here specifies a particular user format, where we can then specify which columns we want to display, and the -L command designates that this command is interested in threads.

<font color="cyan"><i>NOTE: If you don’t do the sort before the uniq -c, it won’t aggregate correctly and you’ll get extra lines.  Then you have to do MATH, which while good for the brain, is annoying.</i></font>

```bash
[root@[host] ~]# ps -Led -o user | uniq -c | sort -n
      1 ntp
      1 postfix
      1 postfix
      1 root
      3 root
      1 USER
      1 wwwrun
      2 root
      2 tracy
     15 root
     11 root
     49 root
     60 root
     81 wwwrun
    157 runner_agent
    209 runner_agent
    940 runner_agent
```

Now we know runner_agent has too many threads, so let's figure out which app running under that is the most problematic -

```bash
[root@[host] ~]# ps -o nlwp,pid,lwp,args -u runner_agent | sort -n
NLWP   PID   LWP COMMAND
  33  3313  3313 /opt/jre/bin/java -Djava.util.logging.config.file=/opt/AppA…
  53 17419 17419 /opt/jre8/bin/java -Djava.util.logging.config.file=/opt/AppB….
 169  4892  4892 /opt/jre8/bin/java -Djava.util.logging.config.file=/opt/AppC…
```

Again, we are specifying which fields we want displayed: number of threads (nlwp), the pid, the thread id (lwp), and the arguments for the process.  That simple command well help you determine and quash the rebellion of your rogue app.  In my example above, AppC has 169 threads open under the runner_agent user.

<h2>TIP AND TRICK #2 - I REMEMBER WHEN...</h2>

I enjoy using ps for memory determination, because it's cool.  While ps aux will show memory usage per app by default (%CPU and %MEM), regular ps sometimes needs some coaxing.

We often have hungry services that steal memory from others on the server and need to determine the offending app.
<center><iframe src="//lyyn.fr.nf/photoshow/?t=Thb&f=Troll+languages%2Fau8hs2.jpg" width="520" height="300" frameBorder="0" allowFullScreen></iframe><p><a href="http://lyyn.fr.nf/photoshow/?t=Thb&f=Troll+languages%2Fau8hs2.jpg">via http://lyyn.fr.nf/photoshow/?t=Thb&f=Troll+languages%2Fau8hs2.jpg
</a></p></center>

If you want to figure out what service refuses to share and play nice, you can do this:

```bash
[root@[host] ~]# ps -eo pmem,pcpu,vsize,pid,cmd | sort -k 1 -nr | head -3
12.7  1.0 4228104 4892 /opt/jre8/bin/java -Djava.util.logging.config.file=/opt/AppA…
12.6  0.3 2892564 4034 /opt/jre/bin/java -Djava.util.logging.config.file=/opt/AppB
 9.9  0.0 3885252 4338 /opt/jre8/bin/java -Djava.util.logging.config.file=/opt/AppC
```

It even sorts it conveniently!  If you want to see all your applications, you can remove the <font color="cyan"><i>| head -3</i></font>, but generally you only need the first few to determine the problematic application.

<h2>TIP & TRICK #3 - I CAN SEE THE TREES THROUGH THE FOREST</h2>

One more easy ps win.  I use pstree a lot for viewing child processes that spawn out.  For that, I use the following command:

```bash
-sh-4.2$ pstree -alp | grep ntp
  |-ntpd,479 -u ntp:ntp -x -u ntp:ntp -p /var/run/ntpd.pid -g
  |               |-grep,21705 --color=auto ntp
```

Which shows you the hierarchy of applications.  It's similar to using ps faux, but with a slightly better display.  I often wrap this in a <font color="cyan"><i>watch</i></font> command when viewing the status of something like a cron, or a deploy service that I've orchestrated.


<h2>HUZZAH!  YOU'RE A PS GURU NOW!</h2>

And those are just a few of the amazing things you can do with <font color="cyan"><i>ps</i></font>!  

There's some great references online - here are a few if you feel like reading some more.

<a href="http://linoxide.com/how-tos/linux-ps-command-examples/">Linux ps Command Examples</a><br>
<a href="http://www.thegeekstuff.com/2011/04/ps-command-examples">ps Command Examples</a>


Now you probably all know a ton more about this little command than you ever did (or cared to) before.  Next week we'll tackle a softer skills topic - the numbers of women in tech.  Thanks for reading!
