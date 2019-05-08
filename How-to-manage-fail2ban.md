## Description

fail2ban can block ssh login attempts coming from IP addresses that are suspicious of attacking the server. SSH is just a service they protect, a _jail_ in their terms.

## Context

This article is motivated by myself being banned to login to a local linux container. Instead of unbanning myself straight away, I decided to learn how to use fail2ban and understand it better.

This morning, I tried to do an ssh to my container with 2 different usernames, with different certificates. The result was that the 6th attempt was refused immediately with a "Connection refused" message, instead of the characteristic delay of a "legit" failed attempt and some nicer message like "Permission denied (publickey).". So I noticed I had banned myself.

## Configuration

In Debian, configuration files can be found in `/etc/fail2ban/`.
The definition of "suspicious" is a combination of a number of failed auth attempts `maxretry` within a certain period of time `findtime`. These and many more parameters are configured in `/etc/fail2ban/jail.conf`.

However, the command line will give us the current value of the parameters and the number of jailed ip addresses. Let's start:

```bash
# fail2ban-client status
Status
|- Number of jail:      1
`- Jail list:   sshd
```

```bash
# fail2ban-client status sshd
Status for the jail: sshd
|- Filter
|  |- Currently failed: 0
|  |- Total failed:     5
|  `- File list:        /var/log/auth.log
`- Actions
   |- Currently banned: 1
   |- Total banned:     1
   `- Banned IP list: 10.0.3.1
```

## Ban and unban

How does fail2ban manage to ban IP addrs?

```bash
fail2ban-client get sshd action iptables-multiport actionban
<iptables> -I f2b-<name> 1 -s <ip> -j <blocktype>
 fail2ban-client get sshd action iptables-multiport actionunban
<iptables> -D f2b-<name> -s <ip> -j <blocktype>
```
So as we may have known before, it does it by mangling with iptables. Let's see what we have there (list all rules):
```bash
# iptables -L

Chain INPUT (policy ACCEPT)
target     prot opt source               destination
f2b-sshd   tcp  --  anywhere             anywhere             multiport dports ssh

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination

Chain f2b-sshd (1 references)
target     prot opt source               destination
REJECT     all  --  10.0.3.1             anywhere             reject-with icmp-port-unreachable
RETURN     all  --  anywhere             anywhere
```
We can see that fail2ban is capturing the tcp traffic with destination port ssh, and redirecting it through an iptables chain called f2b-sshd. To focus on this one, let's do:

```bash
# iptables -L f2b-sshd
Chain f2b-sshd (1 references)
target     prot opt source               destination
REJECT     all  --  10.0.3.1             anywhere             reject-with icmp-port-unreachable
RETURN     all  --  anywhere             anywhere
```
So this means that all traffic from 10.0.3.1 that gets to this chain (incoming ssh), is rejected  with ICMP port unreachable message, as if there was no ssh server listening.

### To unban

Retrieving what we saw above, fail2ban will execute a certain command to ban an IP address for the jail sshd. The hint it gives us is `<iptables> -D f2b-<name> -s <ip> -j <blocktype>`. In our case, this translates to:
```bash
iptables -D f2b-sshd -s 10.0.3.1 -j REJECT
```
That is, delete `-D` from chain `f2b-sshd` the rule that is defined by the parameters "source ip" and "target action". If such a rule exists, iptables will effectively delete it.

To check it out, let's list again the rules from f2b-ssh chain:
```bash
# iptables -L f2b-sshd
Chain f2b-sshd (1 references)
target     prot opt source               destination
RETURN     all  --  anywhere             anywhere
```
Bingo! :)

### To ban
If this has been such long that fail2ban has already forgiven you, you can ban yourself this time via iptables directly, executing the same action as fail2ban does. The template in this case is `<iptables> -I f2b-<name> 1 -s <ip> -j <blocktype>`
```bash
iptables -I f2b-sshd 1 -s 10.0.3.1 -j REJECT
```
As you can see, the command is almost the same. The iptables command is now insert `-I`. Insert to chain f2b-sshd, "1", the rule defined by these "source ip" and "target action". Looking into `man iptables`, this 1 is the `rulenum`:
```
-I, --insert chain [rulenum] rule-specification
              Insert  one  or  more rules in the selected chain as the given rule number.  So, if the
              rule number is 1, the rule or rules are inserted at the head of  the  chain.   This  is
              also the default if no rule number is specified.
```
## logs

Apart from fail2ban-client, we can also extract valuable information from the logs. Let's see first what happened this morning from the perspective of fail2ban:

**/var/log/fail2ban.log**
```
2019-05-08 08:51:53,305 fail2ban.filter         [216]: INFO    [sshd] Found 10.0.3.1
2019-05-08 08:52:02,418 fail2ban.filter         [216]: INFO    [sshd] Found 10.0.3.1
2019-05-08 08:52:24,894 fail2ban.filter         [216]: INFO    [sshd] Found 10.0.3.1
2019-05-08 08:52:35,158 fail2ban.filter         [216]: INFO    [sshd] Found 10.0.3.1
2019-05-08 08:54:32,837 fail2ban.filter         [216]: INFO    [sshd] Found 10.0.3.1
2019-05-08 08:54:33,684 fail2ban.actions        [216]: NOTICE  [sshd] Ban 10.0.3.1
```
So, 5 failed login attemps from IP 10.0.3.1 were "Found". At the 5th, this IP gets banned. Let's see how sshd saw these same events (notice the time of the entries):

**/var/log/auth.log**
```
May  8 08:51:53 lxc-hostname sshd[6896]: Invalid user admin from 10.0.3.1
May  8 08:51:53 lxc-hostname sshd[6896]: input_userauth_request: invalid user admin [preauth]
May  8 08:51:53 lxc-hostname sshd[6896]: Connection closed by 10.0.3.1 port 38930 [preauth]
May  8 08:52:02 lxc-hostname sshd[6899]: Invalid user www-data from 10.0.3.1
May  8 08:52:02 lxc-hostname sshd[6899]: input_userauth_request: invalid user www-data [preauth]
May  8 08:52:02 lxc-hostname sshd[6899]: Connection closed by 10.0.3.1 port 38940 [preauth]
May  8 08:52:24 lxc-hostname sshd[6901]: Invalid user www-data from 10.0.3.1
May  8 08:52:24 lxc-hostname sshd[6901]: input_userauth_request: invalid user www-data [preauth]
May  8 08:52:24 lxc-hostname sshd[6901]: Connection closed by 10.0.3.1 port 38946 [preauth]
May  8 08:52:35 lxc-hostname sshd[6904]: Invalid user root from 10.0.3.1
May  8 08:52:35 lxc-hostname sshd[6904]: input_userauth_request: invalid user root [preauth]
May  8 08:52:35 lxc-hostname sshd[6904]: Connection closed by 10.0.3.1 port 38948 [preauth]
May  8 08:54:32 lxc-hostname sshd[6921]: ROOT LOGIN REFUSED FROM 10.0.3.1
May  8 08:54:32 lxc-hostname sshd[6921]: ROOT LOGIN REFUSED FROM 10.0.3.1 [preauth]
May  8 08:54:32 lxc-hostname sshd[6921]: Connection closed by 10.0.3.1 port 38960 [preauth]
```
So we apart from the "attacking" IP address, we can see which username they tried. In this case I faked the log I show you, but they were 2 different usernames that actually didn't exist yet. The last attempt (at 08:54:32), is with root username, which was disabled via sshd config. So it's refused because of that, and also makes fail2ban decide it's enough and ban the IP address.

Have a safe login!