---
layout: post
title: "How to Pick an Apple Friendly TLD"
date: 2013-04-21 16:35
tags: [NSDataDetector]
---

Can't find the .com or .net [top-level domain][4] (TLD) for your website? Thinking about a country code TLD for your site like .io or possibly a domain hack? Great, there are plenty of short url's available for other TLD's besides the traditional .com's and .net's. You can filter by price or how it looks with your particular URL but you should also consider *data detectors*.

Apple's data detector [NSDataDetector][3] combs user text views such as notes and emails looking for anything from mailing addresses, to email addresses and even tracking numbers for packages. But most importantly for your website it looks for URL's. Now normally it will pick just about any TLD as long as it is prepended with `www.` such as `www.github.io`. However it doesn't pick up all non-prepended TLD's. For example, if you email someone with the text `github.io`, it will not turn that nice shade of blue indicating that it is a tappable link. So which non-prepended TLD's does it pick up? I asked [stackoverflow][1] but no one seemed interested so I ran the names myself with this [text file from ICANN][2].

### Uppercase

    example.COM
    example.EDU
    example.EU
    example.GOV
    example.LY
    example.NET
    example.ORG

### Lowercase

    example.ar
    example.at
    example.au
    example.be
    example.br
    example.ca
    example.ch
    example.cn
    example.de
    example.dk
    example.edu
    example.es
    example.eu
    example.fi
    example.fr
    example.gov
    example.gr
    example.hk
    example.hu
    example.il
    example.is
    example.it
    example.jp
    example.kr
    example.lu
    example.ly
    example.ma
    example.mx
    example.net
    example.nl
    example.no
    example.nz
    example.org
    example.pt
    example.ru
    example.se
    example.sg
    example.th
    example.tn
    example.tr
    example.tw
    example.ua
    example.uk
    example.us

Interestingly enough the data detector is case sensitive. So it looks like if you can't find a traditional TLD you should try to go with `.ly` (the country code TLD for Libya) since NSDataDetector picks up both uppercase and lowercase URL's. One more thing to consider is the TLD keyboard shortcut on iOS. A user can hold down the .com button to quickly fill in .net, .edu, .org, .us, or .com of course. Also remember that all this was tested on the US locale and ymmv on a different locale.

[1]: http://stackoverflow.com/q/16088329/142358
[2]: http://data.iana.org/TLD/tlds-alpha-by-domain.txt
[3]: https://developer.apple.com/library/mac/#documentation/Foundation/Reference/NSDataDetector_Class/Reference/Reference.html
[4]: http://en.wikipedia.org/wiki/Top-level_domain