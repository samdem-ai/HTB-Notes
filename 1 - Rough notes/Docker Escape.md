
2025-09-02 14:48

Status:

Tags: [[docker]], [[docker-escape]], [[postgres]],[[exploits]]


# Docker Escape

to do you should be root in the container 
and to get to root i used a postgres vulnerability (CVE-2019-9193) to become the postgres user and then it contained an suid in /bin/bash

to escape docker i used these two articles :
- https://secnigma.wordpress.com/tag/docker-escape/
- https://github.com/HackTricks-wiki/hacktricks/blob/master/src/linux-hardening/privilege-escalation/docker-security/docker-breakout-privilege-escalation/README.md
- https://exploit-notes.hdks.org/exploit/container/docker/docker-escape/


# References
