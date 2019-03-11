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
