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
