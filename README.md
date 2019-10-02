# shalt
shalt is a semi-interactive shell for saltstack.

Features:
1. shell-like interactive "commandline"
2. directory/file tab autocompletion
3. remote file editing using vim
4. run commands in the background and retrieve the results later

## Sample session
Below is a sample session that demonstrates the features of this tool. Pay special attention to the different ways of selecting minions.

```shell
bash-4.1$ shalt -E foo4bar40[012]
Enter your commands as if running in a shell. "exit" or "quit" to stop.
Ctrl-C to cancel a command. Ctrl-G to abort a reverse history search (via Ctrl-R).

To diff files: ffid|ffids <minion:file> <minion:file>
To run a command in the background: bg <command>
To retrieve background jobs: jobs [job id]

To select a subset of minions, use "select [minions] [command]". Commands will only run on this subset.
Select examples:
    select
    select list-of-minions
    select list-of-minions command

List of other available salt modules:
  test.ping
  test.version
  status.loadavg
  status.uptime
  puppet.enable
  puppet.disable
  puppet.noop
  puppet.run
  puppet.status

Type "help" at any time to display this message.

[main] <-E foo4bar40[012]> /root> test.ping
foo4bar401:
    True
foo4bar402:
    True
foo4bar400:
    True
[main] <-E foo4bar40[012]> /root> sel foo4bar405 uname -n
foo4bar405:
    foo4bar405
[main] <-E foo4bar40[012]> /root> sel foo4bar406
[select] <-E foo4bar406> /root>>> test.ping
foo4bar406:
    True
[select] <-E foo4bar406> /root>>> uname -n
foo4bar406:
    foo4bar406
[select] <-E foo4bar406> /root>>> exit
[main] <-E foo4bar40[012]> /root> sel
Select minions by typing their number separated by space:
1:  foo4bar400
2:  foo4bar401
3:  foo4bar402
selection> 2 3
[select] <-L foo4bar401,foo4bar402.> /root>>> test.ping
foo4bar401:
    True
foo4bar402:
    True
[select] <-L foo4bar401,foo4bar402.> /root>>> exit
[main] <-E foo4bar40[012]> /root>
[main] <-E foo4bar40[012]> /root> echo 'testing' > /tmp/foo
foo4bar401:
foo4bar402:
foo4bar400:
[main] <-E foo4bar40[012]> /root> cat /tmp/foo
foo4bar402:
    testing
foo4bar401:
    testing
foo4bar400:
    testing
[main] <-E foo4bar40[012]> /root> vi /tmp/foo
[main] <-E foo4bar40[012]> /root> cat /tmp/foo
foo4bar401:
    testing
    another line
foo4bar402:
    testing
    another line
foo4bar400:
    testing
    another line
[main] <-E foo4bar40[012]> /root> sel foo4bar400 echo 'this host only' >> /tmp/foo
foo4bar400:
[main] <-E foo4bar40[012]> /root> vi /tmp/foo
Remote editing requires that only ONE minion is selected or with MULTIPLE minions if the target files are EXACTLY THE SAME.
[main] <-E foo4bar40[012]> /root> cat /tmp/foo
foo4bar401:
    testing
    another line
foo4bar400:
    testing
    another line
    this host only
foo4bar402:
    testing
    another line
exit
[main] <-E foo4bar40[012]> /root>
[main] <-E foo4bar40[012]> /root> ffids foo4bar400:/tmp/foo foo4bar401:/tmp/foo
testing                           testing
another line                      another line
this host only                  <
[main] <-E foo4bar40[012]> /root>
[main] <-E foo4bar40[012]> /root> sel foo4bar400
[select] <-E foo4bar400> /root>>> bg sleep 5; date; sleep 5; date
[select] <-E foo4bar400> /root>>> jobs
1: sleep 5; date; sleep 5; date
[select] <-E foo4bar400> /root>>> jobs 1
[select] <-E foo4bar400> /root>>> jobs 1
foo4bar400:
    Wed Oct  2 00:12:09 CDT 2019
    Wed Oct  2 00:12:15 CDT 2019
[select] <-E foo4bar400> /root>>> exit
[main] <-E foo4bar40[012]> /root> exit
bash-4.1$

```
