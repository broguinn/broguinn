---
layout: post
title: Weekend Recommendations
date: 2017-03-06T11:32:03-06:00
categories: recommendations
---

This weekend was especially productive and I have a bunch of cool things I want to share:

## Uprooted by Naomi Novik

I read Naomi Novik's [_Uprooted_](https://www.goodreads.com/book/show/22544764-uprooted), which The New York Times calls:

> "A very enjoyable fantasy with the air of a modern classic."

It [won the Nebula Award](http://www.sfwa.org/2016/05/nebula-award-winners-announced-3/) and is a deeply satisfying fantasy novel. It's easy to see where Novik's inspiration comes from: medieval lords are generally terrible to serfs, stab each other in the back, and "trade other people's blood for honor", a là Game of Thrones or the also quite excellent [The Goblin Emperor](https://www.goodreads.com/book/show/17910048-the-goblin-emperor). Its beautiful prose is reminiscent of Old World mythology with a tight, intricate plot.

Apparently, Ellen DeGeneres is turning it into a movie, which I can't say I'm too thrilled about. Because in my wildest dreams, the rough edges and hard moral truths of Novik's book aren't sheared down to another "Snow White and the Huntsman". They're treated with the same care and patience for beautiful settings and subtle plots that Diana Wynne Jones' _Howl's Moving Castle_ was when it was adapted by Studio Ghibli.

And now that Hayao Miyazaki is out of retirement, who knows? It's easy to imagine Novik's book as a Ghibli film; the protagonist, Agnieszka, is the hard-willed, clever, and sensible heroine Miyazaki has portrayed so well — not only in _Howl's_, but _Castle in the Sky_, _Nausicäa_, and _Princess Mononoke_. I can dream.

## AdvancedTomato: Hacking your Router for Fun and Profit

After discovering that the wifi for the upstairs was being supported by a 2004 Apple Express access point, and forcing my coworkers through some harrowingly choppy video calls, I decided to upgrade my router. Some coworkers strongly recommended that I look into routers compatible with [Advanced Tomato](https://advancedtomato.com/downloads), an open-source firmware that replaces whatever your router is already running.

Advanced Tomato only works on certain routers, specifically ones that use an internal program called _nvram_ to interface with hardware, and that are easily flashable; a term I didn't really know before this weekend, but seems to mean "lets you install new firmware".

Advanced Tomato is a fork of another project called Tomato, but with a prettier UI. Why would you want your own firmware? For one, the router I bought, the Netgear R6400, has a [gaping security flaw](https://arstechnica.com/security/2016/12/unpatched-bug-allows-hackers-to-seize-control-of-netgear-routers/) in its stock firmware that allows malicious parties to run any command against your router as root (yikes). Beyond peace-of-mind, Tomato gives you some really, really cool features. You can:

1. run your own Tor node
2. host your own VPN, if you're using public wifi someplace
3. host a MYSQL server
4. fine-tune Quality of Service[^qos]
5. manage and track historical performance and activity records[^notevil]

And many more cool things. Shout out to my coworker Rasmus for some early Sunday morning assistance with the QoS options.

A few quick takeaways, for those that follow down this path:

1. You can try to [overclock your router](https://www.howtogeek.com/67943/5-tips-for-getting-the-most-out-of-your-tomato-router/) (via `ssh` or `telnet`) by up to 20%, and even if forums users with your exact firmware and router say it's _super_ reliable, it *will* cause your machine to stop serving traffic, making you think it's bricked. Which leads me to:
2. You can reset your nvram settings by holding down the reset button for 30s. This will also completely remove all other settings, including admin and wifi passwords. You will probably do this > 3 times.
3. You can save a copy of your configuration to your machine to upload into Advanced Tomato via the GUI. I recommend this.
4. A safer recommendation for a performance boost: the default signal strength for Tomato is exceptionally weak. If you set it to `0` it will default to the hardware settings, which is both a. definitely sane and b. definitely higher than what Tomato suggests. The speed in my kitchen, some 1000 feet from my router, doubled when I did this.

Overall, my up/down went from 20 mbps / 5 mbps to 190 mbps / 20 mbps. Quite the improvement!

## Death and Taxes: Credit Karma

Not wanting the fun to stop here, I decided to tackle my taxes this weekend. I vaguely recalled [Credit Karma](https://creditkarma.com) does taxes now, so I gave 'em a shot. It was surprisingly easy.

For those that aren't familiar with the website, Credit Karma is a free and simple tool that reports on your credit score without making a hard pull (formal inquiry) from credit bureaus (which negatively impacts your score). I highly recommend it, as it helped me discover an unknown derogatory mark of which I hadn't been aware.

Anyways, they have a simple GUI for walking through both state and federal taxes. In the past I've used TurboTax, like (presumably) millions of Americans, and been generally put off by their chicanery[^chicanery]. So I'm pleased to say Credit Karma was not evil. In fact, it was super simple. My only reservation is for people new to online filing: Often, Credit Karma would not have a good definition of some of the fields it asked for. For instance, I had to google "Retirement Savings Contributions Credit", which I admit not everyone would feel comfortable doing. That being said, it was super simple and I highly recommend it.

_nb_: In order to file online, the US Gov't requires you to confirm your Adjusted Gross Income from the _previous_ tax period; in this case, 2015. I, for the life of me could not find my return from the previous year. Trying to download my return transcript from the IRS was a nightmare, ultimately ending in them suggesting they mail a confirmation form to me in 5 to 10 days. TurboTax snidely suggests they'll give me my old return for some evil lump sum of money by "importing" it into the TurboTax software. However, you can trick them into giving you the AGI. If you click through as though you're agreeing to pay for last year's information, right before the confirmation page, your AGI is listed in the header enumerating all the information you're about to buy. Steal the info, cancel the purchase, and you're riding free.

### References

[^qos]: making sure, for instance, that your roommate downloading the new episode of Legion doesn't interrupt your work meeting.
[^notevil]: note to self: not to be used for evil
[^chicanery]: including, but not limited to: ads, sales pitches, opt-out emails, outright lies and tricks, expensive and unnecessary state return software.