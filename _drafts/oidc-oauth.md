---
title: Learn OIDC - Part 3 - OAuth
excerpt: 
  Dive into basic OAuth as basis for an OIDC server
tags:
- java
- oauth
- oidc
- diy
toc: true
---

## Introduction
In search of a lightweight OIDC mock server, I explored the underlying techniques and protocols like JWS, JWT and OAuth.

**WARNING:** The code shown in this series is for educational purposes only and is not meant to be used in production.
{: .notice--warning}

## OAuth2
The basis for OIDC is essentially the OAuth2 protocol[^1].

[^1]: OAuth2 Specification: [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)