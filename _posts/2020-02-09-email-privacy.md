---
layout: post
title: Email privacy
date: 2020-02-09 14:35:10
author: Peter Stevenson
summary: Privacy focused email inbox with a custom domain
categories: sysadmin
thumbnail:
tags:
 - mail server
 - Ionos
---

# Email privacy

In this article I detail how I setup my privacy focused email inbox with a custom domain to enable flexibility in case I ever want to move mail servers.

I have some redacted information which is designated in strike through text. This revolves around my reverse engineering process of PTR validation. I left it here in case people wanted to see the evidence that allowed me to come to a conclusion on how PTR is validated by the mail server software.

I was considering either [Posteo](https://posteo.de/en) or [mailbox.org](https://mailbox.org) because of following resources

* [privacytools.io/providers/email](https://www.privacytools.io/providers/email/)
* [Chris Were Digital - Posteo - Email Service Review](https://www.youtube.com/watch?v=paTrSZ-girM)
* [Tom Spark Reviews - Posteo Email Review - Is it Good?](https://www.youtube.com/watch?v=7tJw-1Rnguw)
* [Tom Spark Reviews - mailbox.org Review - Is it Good?](https://www.youtube.com/watch?v=Pa5BWVB28II)

I decided on mailbox.org.

## The main reasons I did not go with Posteo

* Their domain could be confusing to type for some people.
* They didn't have external domain support
* They wanted 1 year payment upfront.

## My setup

I went with Ionos aka 1&1 for the domain provider.

I choose mailbox.org as the mail server.

1. Buy domain and ensure private whois is enabled (default in EU/UK at time of writing). For this example I am using: `my-domain.co.uk`
2. Buy mailbox.org package. Check usage on your current mail provider to gauge how much storage you need.
3. Add external domain as per their instructions here: [Using custom domains](https://kb.mailbox.org/display/MBOKBEN/Using+e-mail+addresses+of+your+domain)
4. Add MX as per their instructions (you may need to click "disable service" "web hosting" on the DNS entry if using Ionos).
5. Add SPF as per their instructions although I suggest tailing it with `-a` to ensure no one can spoof you.
6. DKIM improves spam score with Google so I enabled that as per mailbox.org instructions.
7. ~~Add PTR as per below instructions.~~

Autodiscover is only useful if the custom domain is the primary one on mailbox.org.

DMARC I don't think adds any benefit unless you are good at data analytics's. So ignore it.

5, 6 ~~and 7~~ are only essential if you are sending emails from this domain or want to prevent someone spoofing you. For receive only it's not a concern.

### PTR

~~Google/Gmail doesn't seem to worry about PTR but it may help spam score.~~

I had some mail being flagged by 365 spam filters.

365 spam headers don't explicitly state the reason for marking it as spam and it seems to lumber a lot under the generic header `SFV:SPM` see: [Anti-spam message headers](https://docs.microsoft.com/en-us/microsoft-365/security/office-365-security/anti-spam-message-headers)

~~Change the A record to point to mailbox.org's IP address so we can workaround the whole PTR issue. It takes a while to propagate.~~

#### ~~PTR work around example~~

~~`my-domain.co.uk -> 80.241.60.194 -> lookup PTR -> mailbox.org - > 80.241.60.194  = FCrDNS (Forward Confirmed Reverse DNS)`~~

~~The A record IP must be a static domain/IP for this to work.~~

PTR and rDNS are the same thing just different names. PTR must be set on the server side.

#### How the mail server checks PTR

[What are PTR records?](https://onlinehelp.cloud.telenet.be/article.php?id=1291)

There isn't a tonne of documentation on how the email server does the PTR check. 

~~It doesn't seem to care if my domain here doesn't match the PTR domain.~~

~~It only seems to care if the PTR of the IPv4 points to a domain and if that domain points to the same IPv4.~~

~~This is presumably because my domain could have multiple origin IPs due to multiple outbound SMTP server cluster.~~

In this context "client" means the sending SMTP server: `mout-u-204.mailbox.org` and "sender" means the senders domain: `my-domain.co.uk`

It probably depends how strict the mail server is. It may check PTR of the "client" and the "sender" ~~which appears to be the case for 365~~. My email stopped getting flagged as spam once I made these changes. **After further investigation detailed below and examination of email headers I see that the SPF record didn't propagate to exchange servers at the time I was having issues**. So the fact I made my "sender" domain have a reverse PTR was a false positive and had no effect I now believe. So I will make my domains point to my web server which unfortunately has a no FCrDNS IP but it shouldn't cause me any problems.

Looking at the [postfix/postconf man pages](https://linux.die.net/man/5/postconf) 

Of course exchange servers don't run postfix but it's probably the most popular mail server.

* `reject_unknown_client_hostname` mailbox.org cover this.
* `reject_unknown_reverse_client_hostname` mailbox.org cover this.
* `reject_unknown_helo_hostname` mailbox.org cover this.
* `reject_unknown_recipient_domain` Out of our control if recipient mail server is setup incorrectly.
* `reject_unknown_sender_domain` We have an A record and a MX record.

**It looks like mail servers don't perform PTR on "sender" domain. They only perform this on the "client" domain**.

## Testing

Run domain through the testing tools listed below.

### Example fully configured received headers and source

I have removed most text and left the relevant parts.

```
Authentication-Results: spf=pass; dkim=pass (signature was verified); dmarc=bestguesspass action=none;compauth=pass reason=109
Received-SPF: Pass
Received: from mout-u-107.mailbox.org (91.198.250.252)
Received: from smtp1.mailbox.org (smtp1.mailbox.org [IPv6:2001:67c:2050:105:465:1:1:0])
X-Virus-Scanned: amavisd-new at heinlein-support.de
DKIM-Signature: v=1; a=rsa-sha256; c=relaxed/relaxed;
Received: from smtp1.mailbox.org ([80.241.60.240])
X-Forefront-Antispam-Report:
 CIP:91.198.250.252;IPV:;CTRY:DE;EFV:NLI;SFV:NSPM;SFS:(10001)(189003)(199004)(956004)(1096003)(6916009)(5660300002)(336012)(3480700007)(9686003)(26005)(558084003)(58800400005)(8676002)(7116003)(246002)(356004)(7636002)(7596002)(86362001)(35100500004)(220243001)(558944008);DIR:INB;SFP:;SCL:1;SRVR:AM0PR02MB5025;H:mout-u-107.mailbox.org;FPR:;SPF:Pass;LANG:en;PTR:mout-u-107.mailbox.org;MX:1;A:1;
```

### Example 365 header flagged as spam ~~before I setup PTR~~ before SPF propagated and before DKIM

```
Authentication-Results: spf=pass; dkim=none (message not signed); dmarc=bestguesspass action=none;compauth=pass reason=109
Received-SPF: Pass
Received: from mout-u-204.mailbox.org (91.198.250.253)
Received: from smtp2.mailbox.org (smtp2.mailbox.org [IPv6:2001:67c:2050:105:465:1:2:0])
X-Virus-Scanned: amavisd-new at heinlein-support.de
Received: from smtp2.mailbox.org ([80.241.60.241])
X-Forefront-Antispam-Report:
 CIP:91.198.250.253;IPV:;CTRY:DE;EFV:NLI;SFV:SPM;SFS:(10001);DIR:INB;SFP:;SCL:5;SRVR:DB8PR02MB5833;H:mout-u-204.mailbox.org;FPR:;SPF:None;LANG:en;CAT:SPM;
```

Analysing the X-Forefront-Antispam-Report message we see the following interesting header fields

* `SFV:SPM` The message was marked as spam by the content filter.
* `SPM: Spam` The category of protection policy, applied to the message.
* `SCL:5` An SCL rating of 5 or 6 is considered suspected spam.
* `SPF:None` This was the red herring that I didn't notice earlier. Further up the headers is two SPF pass results from other servers so I assumed it was good. It turns out the SPF wasn't fully propagated to exchange servers and I started going down a rabbit hole assuming it was a "sender" domain PTR issue as there was nothing else to blame.

Nothing here is suggesting that 365 does a PTR against my "sender" domain.

## Issues

**2020-02-04 18:11:** mailbox.org calendar wont send email notifications unless you set the timezone in the settings to match that of the event's time zone. So do that first and it becomes the default for new events.

**Update: 2020-02-04 19:41:** After I exported the old and new events `.ics` files to `diff`. It appears changing the primary alias and removing the old one will break email notifications for any calendar events created before that.

**Update: 2020-02-07 19:30:** I am still having issues getting calendar to reliably email me event notifications.

**Update: 2020-02-19 12:03:** The email subject line of the reminder is always in CET despite all my settings being in GMT and my mail body is GMT. 

**Update: 2020-02-19 12:03:** The plus addressing folder organisation is case sensitive given email addresses should be regarded as case insensitive. This isn't mentioned here at time of writing: [Can I use plus-aliases in the format](https://kb.mailbox.org/display/MAILBOX/What+is+an+alias+and+how+do+I+use+it) However it is here: [How to use mail extensions](https://kb.mailbox.org/display/MBOKBEN/How+to+use+mail+extensions)

**Update: 2020-04-24 15:57:** Mailbox.org: "We have opened a bugreport with Open Xchange".

### Greylisting 

Had me confused for a while why my emails weren't getting through. Eventually they all came through once the sending server retries. See: [Spam protection through greylisting](https://kb.mailbox.org/m/mobile.action#page/1181556) 

### I accidentally deleted the mailbox.org security key

This isn't a problem once the custom alias is already added. It's only used to prove that you own the domain.

You will need to re add a security key for adding another alias email on that domain. They provide this on screen if you try to add one.

### ~~Can you still use HTTP redirects with A record on Ionos?~~

~~1&1 HTTP redirect points the A record to `http://clienthosting.eu/` so it's not possible to use this in conjunction with the PTR work around.~~

## My final DNS entries look like this

![screenshot](/blog/assets/2020-02-09/ionos-final-dns-entries.png)

## Links

### Related blog posts

* [preventing-365-exchange-spoofing.md](https://github.com/2E0PGS/blog/blob/master/_drafts/preventing-365-exchange-spoofing.md)
* [mail-server-spoofing-using-hmail.md](https://github.com/2E0PGS/blog/blob/master/_drafts/mail-server-spoofing-using-hmail.md)

### Useful tools for testing

* [mxtoolbox MX lookup](https://mxtoolbox.com/SuperTool.aspx?action=mx)
* [mxtoolbox SPF lookup](https://mxtoolbox.com/SuperTool.aspx?action=spf)
* [mxtoolbox A record lookup](https://mxtoolbox.com/SuperTool.aspx?action=a)
* [debouncer PTR check](https://www.debouncer.com/reverse-dns-check)
* [google toolbox messageheader](https://toolbox.googleapps.com/apps/messageheader/)

### mailbox.org useful knowledge base articles

* [What data do they store and for how long](https://kb.mailbox.org/display/MBOKBEN/Which+data+do+we+store+and+for+how+long)
* [Can I trust the staff at mailbox.org](https://kb.mailbox.org/display/MBOKBEN/Can+I+trust+the+staff+at+mailbox.org)
* [Mailbox encryption](https://kb.mailbox.org/display/MBOKBEN/The+Encrypted+Mailbox)
* [Encryption of calendar and address book](https://kb.mailbox.org/display/MBOKBEN/Encryption+of+calendar+and+address+book)
* [Move away from Gmail to mailbox.org - step-by-step](https://kb.mailbox.org/display/MBOKBEN/Move+away+from+Gmail+to+mailbox.org+-+step-by-step)

### Other useful resources

* [PayPal sharing my email address](https://www.paypal-community.com/t5/About-Protections/sharing-my-email-address/td-p/1304609#)
* [Office 365 email anti-spam protection](https://docs.microsoft.com/en-us/microsoft-365/security/office-365-security/anti-spam-protection)

### PTR resources

* [PTR Records and Why They Matter for Emails](https://blog.mailtrap.io/ptr-record/)
* [How Mail Servers Check the PTR Record](https://www.hostgator.com/help/article/reverse-dns-record-ptr-pointer-record)