---
title: nftables一些常用配置文件
categories:
  - [常用配置]
  - [技巧]
tags:
date: 2019-08-18 17:27:00
translate_title: some-common-configuration-files-for-nftables
keywords:
description:
top_img: 
comments:
cover: 
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
[post cid="23" cover="https:\/\/www.xyz.blue\/usr\/uploads\/2019\/08\/3488367350.png"/]
```
table inet filter {
	chain input {
		type filter hook input priority 0; policy drop;

		# established/related connections
		ct state established,related accept

		# invalid connections
		ct state invalid drop
		
		# loopback interface
		iif lo accept

		# ICMP & IGMP
		ip6 nexthdr icmpv6 icmpv6 type { destination-unreachable, packet-too-big, time-exceeded, parameter-problem, mld-listener-query, mld-listener-report, mld-listener-reduction, nd-router-solicit, nd-router-advert, nd-neighbor-solicit, nd-neighbor-advert, ind-neighbor-solicit, ind-neighbor-advert, mld2-listener-report } accept
		ip protocol icmp icmp type { destination-unreachable, router-solicitation, router-advertisement, time-exceeded, parameter-problem } accept
		ip protocol igmp accept

		# SSH (port 22)
		tcp dport ssh accept

		# HTTP (ports 80 & 443)
		tcp dport { http, https } accept
	}

	chain forward {
		type filter hook forward priority 0; policy drop;
	}

	chain output {
		type filter hook output priority 0; policy accept;
	}

}
```
ssh限流
```
table inet filter {
	chain input {
		type filter hook input priority 0; policy drop;

		ct state invalid drop

		iif lo accept

		# no ping floods:
		ip protocol icmp icmp type echo-request limit rate over 10/second burst 4 packets  drop
		ip6 nexthdr icmpv6 icmpv6 type echo-request limit rate over 10/second burst 4 packets drop

		ct state established,related accept

		# ICMP & IGMP
		ip6 nexthdr icmpv6 icmpv6 type { destination-unreachable, packet-too-big, time-exceeded, parameter-problem, mld-listener-query, mld-listener-report, mld-listener-reduction, nd-router-solicit, nd-router-advert, nd-neighbor-solicit, nd-neighbor-advert, ind-neighbor-solicit, ind-neighbor-advert, mld2-listener-report } accept
		ip protocol icmp icmp type { destination-unreachable, router-solicitation, router-advertisement, time-exceeded, parameter-problem } accept
		ip protocol igmp accept

		# avoid brute force on ssh限流15次/分钟:
		tcp dport ssh ct state new limit rate 15/minute accept

	}

	chain forward {
		type filter hook forward priority 0; policy drop;
	}

	chain output {
		type filter hook output priority 0; policy accept;
	}

}
```
要分TCP、UDP的话使用tcp、udp参数即可
详情可见：https://wiki.debian.org/nftables