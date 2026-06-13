---
layout: post
title: "Google QUIC: a Better Web Protocol?"
date: 2016-08-27
tags: [networking, protocol, performance]
blogger_url: https://johnlonergan.blogspot.com/2016/08/google-quic-better-web-protocol.html
---

Google's QUIC protocol — reliable UDP over HTTPS — is worth watching. A colleague reminded me I'd done something similar a decade earlier at a financial institution, which made this particularly interesting to read about.

## The Common Problem

TCP slow start is brutal for bursty, low-latency traffic, especially over WANs. Both Google and I ended up at the same conclusion: reliable UDP is a better fit for this kind of traffic pattern.

## What Google Did vs. What I Did

My solution was narrower in scope. No built-in security, no multiplexing, no fairness mechanisms. But both approaches used forward error correction (FEC): dump large numbers of datagrams onto the network optimistically, use FEC to absorb loss, and fall back to a resend protocol if needed.

That works well on corporate networks where conditions are reasonably predictable. It degrades as the network gets noisier — which is presumably why Google invested heavily in the full QUIC stack with proper congestion control and fairness.

## Why Google Can Actually Move This Forward

The difference is scale. Google controls Chrome and a large chunk of the web. They can deploy QUIC on both sides and run experiments at a scale no one else can match. That's how a protocol like this actually gets traction: you need a browser and a web service in the same hands.

Chrome already has QUIC support. Open-source implementations are appearing. Worth keeping an eye on — this could change the transport layer assumptions a lot of software currently makes.

The [Caddy webserver](https://caddyserver.com/) is one to watch on the server side.
