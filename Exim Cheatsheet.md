Exim Cheatsheet
===

Message-IDs and spool files
---
The message-IDs that Exim uses to refer to messages in its queue are mixed-case alpha-numeric, and take the form of: XXXXXX-YYYYYY-ZZ. Most commands related to managing the queue and logging use these message-ids.

There are three -- count 'em, THREE -- files for each message in the spool directory. If you're dealing with these files by hand, instead of using the appropriate exim commands as detailed below, make sure you get them all, and don't leave Exim with remnants of messages in the queue. I used to mess directly with these files when I first started running Exim machines, but thanks to the utilities described below, I haven't needed to do that in many months.

Files in /var/spool/exim/msglog contain logging information for each message and are named the same as the message-id.

Files in /var/spool/exim/input are named after the message-id, plus a suffix denoting whether it is the envelope header (-H) or message data (-D).

These directories may contain further hashed subdirectories to deal with larger mail queues, so don't expect everything to always appear directly in the top /var/spool/exim/input or /var/spool/exim/msglog directories; any searches or greps will need to be recursive. See if there is a proper way to do what you're doing before working directly on the spool files.

Basic information
---
Print a count of the messages in the queue:

`root@localhost# exim -bpc`

Print a listing of the messages in the queue (time queued, size, message-id, sender, recipient):

`root@localhost# exim -bp`

Print a summary of messages in the queue (count, volume, oldest, newest, domain, and totals):

`root@localhost# exim -bp | exiqsumm`

Print what Exim is doing right now:

`root@localhost# exiwhat`

Test how exim will route a given address:

```
root@localhost# exim -bt alias@localdomain.com
user@thishost.com
    <-- alias@localdomain.com
  router = localuser, transport = local_delivery
root@localhost# exim -bt user@thishost.com
user@thishost.com
  router = localuser, transport = local_delivery
root@localhost# exim -bt user@remotehost.com
  router = lookuphost, transport = remote_smtp
  host mail.remotehost.com [1.2.3.4] MX=0
```

Run a pretend SMTP transaction from the command line, as if it were coming from the given IP address. This will display Exim's checks, ACLs, and filters as they are applied. The message will NOT actually be delivered.

`root@localhost# exim -bh 192.168.11.22`

Display all of Exim's configuration settings:

`root@localhost# exim -bP`

Searching the queue with exiqgrep
---
Exim includes a utility that is quite nice for grepping through the queue, called exiqgrep. Learn it. Know it. Live it. If you're not using this, and if you're not familiar with the various flags it uses, you're probably doing things the hard way, like piping `exim -bp` into awk, grep, cut, or `wc -l`. Don't make life harder than it already is.

First, various flags that control what messages are matched. These can be combined to come up with a very particular search.

Use -f to search the queue for messages from a specific sender:

`root@localhost# exiqgrep -f [luser]@domain`

Use -r to search the queue for messages for a specific recipient/domain:

`root@localhost# exiqgrep -r [luser]@domain`

Use -o to print messages older than the specified number of seconds. For example, messages older than 1 day:

`root@localhost# exiqgrep -o 86400 [...]`

Use -y to print messages that are younger than the specified number of seconds. For example, messages less than an hour old:

`root@localhost# exiqgrep -y 3600 [...]`

Use -s to match the size of a message with a regex. For example, 700-799 bytes:

`root@localhost# exiqgrep -s '^7..$' [...]`

Use -z to match only frozen messages, or -x to match only unfrozen messages.

There are also a few flags that control the display of the output.

Use -i to print just the message-id as a result of one of the above two searches:

`root@localhost# exiqgrep -i [ -r | -f ] ...`

Use -c to print a count of messages matching one of the above searches:

`root@localhost# exiqgrep -c ...`

Print just the message-id of the entire queue:

`root@localhost# exiqgrep -i`

Managing the queue
---


Access control
---


Fix SMTP-Auth for Pine
---


Log the subject line
---


Disable identd lookups
---


Disable Attachment Blocking
---


Searching the logs with exigrep
---


Bonus!
---


Reload the configuration
---


Read The Fucking Manual
---
