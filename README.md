java c
CSCI4160
Fall   2024,   Raft   Assignment
Deadline:   Dec   3,   2024,   11:59am
1          IntroductionIn this   assignment, you will   build   a   distributed   replicated    (key-value)   map   using   Raft   [1].   Follow   the   Raft   paper   to   implement   and   pass   all   the   test   cases   in   the   grader.   We   thank   Shimin   Wang   from   Carnegie   Mellon   University   when   preparing   this   assignment.
For   the   details   of Raft   please   refer   to
•   The   detail   of   Raft   presented   by   their   authors:   https://youtu.be/YbZ3zDzDnrw   and   https://youtu.be/vYp4LYbnnW8
•   The   Raft   extended   paper   annotated   by   Shimin Wang,   which   is   included   in the   assign-   ment   package.
•   The   official   web   site   of   Raft, http://raft.github.io.   It provides a good   interactive   demo   with   the   Raft   consensus   protocol.
You   do   not   need   to   handle   all   cases   in   the   full   Raft   paper.   Specifically,
•   You   don’t   need   to   randomize   the   election   timeout.    The   election   timeout   value   would   be   changed   by   the   grader   at   run-time   to   simulate   different   situations.
•   You   don’t   need   to   implement   log   compaction   and   snapshotting.
•   We   don’t   need   to   handle   cluster   membership   changes.
•    No    disk   is   considered   in   this   implementation.       Your    key-value map is in    memory.   When   you   commit   any   log,   you   should   directly   apply   that   operation,   committed   will   be   treated   equally   as   applied.    Consequently,   the   lastApplied   operation   in   the   Raft   paper   is   unnecessary   to   you.
Name
Description
bin/
Student-compiled   binaries    and   grading   tools   binary.
scripts/
Grading   shell   scripts   (Don’t   touch).
tests/
Source   code   of   grading   tools.    (Don’t   touch).
yourCode/
Your   working   directory
yourCode/src/main/java/RaftRunner. java
For   Java.      Write   all   your   code   in   this   file   if   you   use   Java   (Work   on   it).
yourCode/main. go
For   Go.   Write   all your code   in   this   file   if you   use   Go   (Work   on   it).
yourCode/compile   . sh
Script   to   compile   your   program   (Don’t   touch   it).
raft   .   protoThe   gRPC   proto   file.       (Don’t      touch.       Just FYI   only,   since we   have   already   generated the   stubs   for   you.)
raft(anotated)   .   pdf
The   extended   raft   paper   with   some   hints   for   the   implementation   (Please   read   it).
   
Figure   1:   A   condensed   summary   of the   Raft   consensus   algorithm
2          Your   Assignment
2.1          Submission:    git   classroomWe use   Github   Classroom https://classroom.github.com   for   assignment managment.   Git   is   a   predominant version   control   system,   created   by the   inventor   of Linux.    Github   is   an   online   hosting   of   Git   and   it   is   now   hosting   thousands   of open-source   projects.
2.2          Get   the   assignment   packageYou   are   supposed   to   open   a   Github   account   using   your   CUHK   email   address   (it   is   fine   to   use   your   own   Github   account   if you   already   have)   and   follow   the   steps   below   to   register   an   account   and   get   the   starter   code.
1.   Use your Github account to come here   https://classroom.github.com/a/3qoRTJ1Z   to   clone   the   assignment   starter   package   to   be   under   your   Github   account.
2.    If   everything   runs   smoothly,   your   Github   account   will   show   a   new   repo   named   cuhk-   raft-[your-Github-account-name],   like   below:
3.   As   GitHub   shut   down   password   authentication   since   Aug.      2021,   you’re   required   to   setup   a   ssh   key   for   connection   following   the GitHub   Docs.
4.    Look   for   the   tab   “Code”   to   get   the   URL   of your   assignment   repo,   like   below:
5.    Open   a   terminal,   change   to   a   local   directory   of   your   desire,   type   “git      clone      {Your Repo    URL}”    and   your   username/password   to   get    a   local   copy   of   the   repo   on   your   computer.
6.    Finally,   we   should   set   the   user   name   and   email   such   that   the   git   system   recognizes   you.   You   should   do   this   once   only.   To   begin   with,   type   the   followings   in   the   terminal   and   replace   the   name   and   email   with   your   own   Git   account.
2.3          Get   your   starter   code   based   on   your   preferred   language
First   clone   the   project   from   your   repository.   If you   use   Java,   please   execute:   git         checkout         java
If you   use   Go,   please   execute:   git         checkout    go
2.4            SubmissionGit is actually a complicated version control   system   (VCS) that   has   a   lot   of features.    Briefly,   it installs a local VCS on your computer and   you   can   do   any   versioning   locally   (e.g.,   commit-   ting   some   changes,   reverting   some   changes).   If necessary, you   can    “push” your local   changes   from your   local   repo to   an   online   repo   (Github).   You   may   Google the   powerful   usage   of Git   online.    Here,   we   focus   on   how   you   can   commit   your   finished   assignment   to   your   local   repo   and   then   push   to   the   online   private   repo.    We   will   fetch   your   assignment   from   your   online   private   repo   for   grading.   The   process   is   essentially   the   “assignment   submission”   process:Assuming   you   have   done   some   edits   on   the   source   files,   the   command   git    add       .      states   your   interest   of   adding   everything   under   the   current   directory    “.”    to   your   local   Git   repo.   Next,   the   command   git      commit      tries   to   commit   your   changes   to   your   local   repo.       The parameter   -m    "comments"   is   your   comment   about   this   commit.    Lastly,   the   command   git push    origin   tries   to   synchronize   your   local   repo   with   your   online   repo.
For   Git   beginne代 写CSCI4160 Fall 2024, Raft AssignmentJava
代做程序编程语言rs,   make   sure   you   use   a   web   browser   to   login   your   Github   online   and   check   if your   source   codes   have   been   pushed   there   successfully.
To   submit   the   final   version   for   grading, push   the   code   to   the   master   branch   by   git      push origin    go:master    -f if you use Go.    Replace the branch name go   by   java   to   submit   if you   use   Java.
Warning:      DON’T   change   the   file   names,   otherwise   you   get   0   marks
2.5            Your   job
For   Java,   write   your   program   in   ‘src/main/java/RaftRunner.java   ’.      For   Go,   write   your   program   in   ‘main.go   ’.
•   Follow   the    “State”   box   in   Figure   1   to   implement   RaftNode   class   (Java)   /   raftNode struct   (Go).      You   will   need   to   add   a   lot   more   than   what   Figure   1   tells   you   such   as   adding   a   Map   to   be   the   in-memory   key-value   map,   adding   a   server-state   to   store whether   this   node   is   in   “Follower”,   “Candidate”,   or   “Leader”   state,   etc.
•   Function   NewRaftNode:   After   creating   an   instance   of   raftNode, its   initial   state   should be   a   “Follower”,   a   node   shall   create   a   new   thread   to   kick   off   a   new   leader   election   if   it   has   not   heard   from the   leader within   a   period   of   electionTimeout.    To   kick   off the   leader   election,   invoke   the   others’   RequestVote   by   RPC   concurrently.    If   a   remote node   doesn’t   respond   to   your   RequestVote,   don’t   resend   RequestVote,   wait   for   the   next   election.
•   When   a   node   becomes   a    “leader”,   invoke   others’   AppendEntries   RPC   concurrently   based   on   the   heartbeat   interval.
Follow   Figure   1’s   “AppendEntries” box   to   implement   it.    Once   again,   if   a   remote   node doesn’t   respond   to   your   call,   don’t   resend,   wait   for   the   next   heartbeat   interval.
•   RPC   Propose:    Our   grader   will   call   this    RPC   to   put   or   delete   to   update   the   dis-   tributed   replicated   map.Log this operation   using   LogEntry   and   append to the   local   log   of the   leader.    Propose   return   only   after   the   the   log   is   committed   (i.e.,   replicated   to   the   majority   of   other nodes).
•   RPC   GetValue:   Implement   this   to   return   a   value   given   a   key.
•   RPCs   SetElectionTimeout   and   SetHeartBeatInterval:    invoked   by   the   grader   to   set   up   different   test   cases.      On   invoked,   these   functions   (re)-set   the   election   timeout   and   heartbeat   interval   based   on   the   input   values.
After you have finished the   assignment,   compile   it   first   to   check   whether   there   is   any   syntax   error.   You   shall   first   enter   into   yourCode   and   execute:
bash         compile .   sh
A   successful   compilation would   generate the   bin/raftrunner   file.
3          Run   our   grader
After your program can   be   successfully   compiled,   run   scripts/rafttest   . sh to compile the   test   framework   and   check whether you   can   pass the   given test   cases.    The   grader will   launch
5   Raft   instances   as   5   processes.       There   are   10   test   cases:   6   to   test   the   leader   election,   4   to   test   the   log   replication.   For   each   test   case,   it   may
•    Set   different   election   timeouts   and   heartbeat   interval   on   each   node   (process)
•    Delay   or   drop   the   messages   between   the   nodes
•   Invoke   Propose   RPC   to   update   the   distributed   replicated   map
•   Invoke   GetValue   RPC   to   get   value   by   a   key   and   match   it   with   an   expected   value
•    Match   the   messages   between   different   nodes   with   the   expected   messages
•    Note:   there   is   a   running   time   limit   for   each   test   case
All   tests   run   about   200   ~   300   seconds.      If   a   test   case   fails,   the   reason   and   the   expected   messages   (if   any)   will   be   printed   out.   For   example:In   the   above,   the   message    “node    0    <-    2:          RequestVote    --    term:             1,       .   .   . ”       means   node   2   requests   a   vote   from   node   0,   and   the   one   who   sends   the   message   (node   2)   is   in   term   1.   Similarly,   the   message   “node      0    ->    2:          reject,    term:             1”   means   node   0   replies   node 2   with   a   reject,   and   the   one   who   sends   the   message   (node   0)   is   in   term   1.In   the   end   of the   test,   you   will   see   a   summary.   If you   see   something   like   below,   congrat-   ulations.
We   also   provide   a   script   to   test   for   a   specific   test   case.   For   example:   bash         .   .   /scripts/rafttest   _single .   sh    \
testOneCandidateOneRoundElection
Remember   to   run   rafttest   . sh   at   least   once   before   rafttest         single. sh,   otherwise   the   grader   is   not   compiled   and   can’t   be   found.
4            Grading
1.   The   TA   will   fetch   and   grade   your   latest   version   of   master   branch   in   your   repository   as   of the   deadline.   Remember   to   commit   and   push   before   the   deadline!
2.    10   test   cases   in   total.
3.    Passing   one   test   case   will   earn   you   10   marks.
4.    A   test   case   is   regarded   as   PASS   if   and   only   if your   program   always   pass   it.    If   it   fails   once   during   the   grading,   it’s   regarded   as   FAIL.
5.    Part   A   is   leader   election   part   of   Raft.    It   covers   the   first   six   test   cases.    Part   B   is   the   log   replication   part,   which   covers   the   last   four   test   cases.
6.   We’ll   try   to   run   the   grader   as   many   times   as   we   can.      As   far   as   we   know,   10   times   is   not   enough   to   find   all   bugs,   please   run   the   grader   for   more   times   to   fully   test   your   program   before   submission.
   



         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
