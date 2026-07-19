---
title: 'Building my own AI assistant and a home lab from scratch'
description: 'How I turned a mini PC and a Raspberry Pi into a private, self-hosted lab — and built Remi, my own AI assistant to help run it. The wins, the mistakes, and what I learned about doing security properly.'
pubDate: 'Jul 18 2026'
---

Most of what I know about technology, I've learned by building things that break and then figuring out why. This project was no exception. The goal sounded simple: set up my own private lab at home — somewhere to host my own files, run my own services, and practise security without depending on anyone else's cloud. What I actually got was a crash course in networking, Linux, encryption, and my own capacity to delete the wrong thing at the worst moment.

## Starting with the hardware

The lab runs on two machines: a small mini PC that does the heavy lifting, and a Raspberry Pi that handles the supporting infrastructure. The mini PC became the core — it hosts my file storage, media, and a virtual machine I use for security practice. The Pi runs the things that keep an eye on everything: a dashboard, uptime monitoring, and network-level tooling.

The first real lesson came fast. The mini PC is headless — no screen, no keyboard — so everything is done over the network. That's efficient, but it means when the network misbehaves, you're working blind.

## The connectivity crisis

Early on, the core machine kept dropping offline. It would connect, work for a while, then vanish — right in the middle of whatever I was doing. I spent a good while assuming it was a software problem before I actually measured the wireless signal and found the real answer: the signal strength was terrible. The machine's little antenna simply couldn't hold a solid link to the router across the house.

The fix wasn't more configuration — it was physics. I set up a spare router as a Wi-Fi repeater to bridge the gap, and the connection went from constantly dropping to rock solid. Signal strength jumped, packet loss vanished.

> **Lesson:** measure before you theorise. I nearly rewrote working software chasing a problem that was really about where the radio waves could reach. In security and systems work, the obvious-but-boring cause is often the real one.

## Bringing in Remi

The part I'm proudest of is Remi — an AI assistant I built to help me run all of this. Not a chatbot toy: a real assistant with persistent memory, an encrypted password vault, and the ability to safely carry out tasks across my machines when I ask.

The design that mattered most was the security model. Remi exists in two forms. A **full-power version** runs in my terminal, where I'm deliberately in control and it asks before doing anything destructive. A **locked-down version** is reachable from a browser, with only safe, read-only style tools — no shell access, no ability to reveal secrets. The dangerous capabilities stay where I have to consciously reach for them.

Building that taught me more about practical security than any tutorial: least privilege, separating trust levels, and never exposing your most powerful tools to your least trusted surface.

## The mistake I want to be honest about

Here's the part most write-ups leave out. While cleaning up, I ran a delete command on the wrong machine. I meant to remove some old files on my laptop — instead I ran it on the core server and wiped the assistant's memory I'd just carefully moved over.

It stung. But it turned into the most useful lesson of the whole project:

- Losses hurt less when the important things are designed to survive. Remi's core knowledge lived in her code, not just the deleted data — so she still knew who I was.
- The vault only ever held test data, not real secrets — because I'd been cautious about what I trusted it with early on.
- And the very first thing I did afterwards was set up **automated daily backups**, verified them, and confirmed exactly what was inside. No more hoping.

> **Lesson:** everyone talks about backups. It takes deleting something that matters to actually build the habit. Now the system backs itself up every night, and I've tested the restore — because a backup you haven't verified isn't a backup.

## Where it stands

The lab now runs quietly on its own: private file storage that syncs my phone, media streaming, a security-practice environment, and Remi keeping an eye on it all — reachable only over my own private network, never exposed to the open internet. That last choice was deliberate. My personal files don't belong on the public web, and keeping them private is the single biggest security decision I made.

More than the services, what I actually built was understanding. I know how these systems fail, how to recover them, and how to think about what's worth protecting and what isn't — which is really what operational security is about.

Next up: turning the lab into a proper practice ground for offensive and defensive security, and writing those up too.
