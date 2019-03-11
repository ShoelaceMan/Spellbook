Block IP
===
`iptables -A INPUT -s $ip -j DROP`

Carriage return LPR
===
`awk '{printf "%s\r\n", $0}'`

Copy partition table
===
`sfdisk --force $NEWDRIVE < <(sfdisk -d $OLDDRIVE)`

Deleted files disk usage
===
`for process in $(lsof|awk '/([d]eleted)/{print $1" "$8}'|grep -v "0$"|sort|awk '{print $1}'|uniq);do lsof|grep "$process.*(deleted)"|awk '{sum+=$8}END{sum=sum/1073741824}END{print $1" "sum"G"}';done`

Delete Apache semaphores
===
`if [[ $(ipcs -s|grep apache|wc -l) -gt 500 ]]; then for i in $(ipcs -s|grep apache|awk '{print $2}');do ipcrm -s $i;done; fi`

Grep for IP addresses
===
`grep -oE "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b"`

Find hacked PHP
===
`find /home/*/public_html/ -ctime -100 -type f -iname "*.php" -exec awk 'FNR==1 && /eval/ || /strlen/ || /strto/ || /auth_pass/ || /_dl/ || /GLOBALS/  { print FILENAME  }; FNR>1 {nextfile}' {} +`

Kill zombie processes
===
`kill $(ps -A -ostat,ppid | awk '/[zZ]/ && !a[$2]++ {print $2}')`

Whois on IP addresses accessing server live
===
`while read line; do whois $line | grep OrgName | sed "s/OrgName/$line/"; done < <(tailf ACCESSFILELOCATION | grep --line-buffered -oE "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b")`

MySQL backup
===
`mkdir /home/mysql.back && for i in $(mysql -BNe 'show databases'| grep -v _schema);do echo $i; mysqldump $i > /home/mysql.back/$i.sql ; done`

Show top IP accessing server
===
`netstat -tn|grep ":80\|:443"|sed 's/::ffff://g'|awk '{print $5}'|grep -oE "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b"|sort|uniq -c|sort -rn|head`

Restart all crashed services (SysV-init only)
===
`for s in $(for i in /etc/rc3.d/S*;do $i status;done|grep stopped|awk '{print $1}');do service $s start;done`

Start a queue run in Qmail
===
`kill -ALRM $(ps ax | awk '/[q]mail-send/ {print $1}')`

Rescan all drive hosts for new hardware
===
`for i in /sys/class/scsi_host/host*/scan; do echo $i; echo "- - -" > "$i";done`

Split FLAC+CUE into several FLAC files
===
`for dir in ./* ; do cd "$dir"; pwd; cuebreakpoints *.flac | shnsplit -o flac *.flac; cuetag.sh *.cue split-track*.flac; cd ..; done`

Show HTTP traffic from TCPDUMP
===
`tcpdump -s 0 -i INTERFACE -A host HOST and tcp port http`

Unblock IP address (This is interactive)
===
`echo -e "IP?"; read IP; grep $IP /var/log/lfd.log /usr/local/cpanel/logs/cphulkd.log /etc/csf/csf.* /etc/hosts.allow; grep "deny" /etc/hosts.allow | grep -v "#"; /usr/local/cpanel/scripts/cphulkdwhitelist $IP; csf -a $IP ; csf -tr $IP; whmapi1 flush_cphulk_login_history_for_ips ip=$IP`

Update all docker images
===
`for i in $(docker images|grep -v ^REPOSITORY|awk '"'"'{print $1":"$2}'"'"'); do docker pull $i;done`