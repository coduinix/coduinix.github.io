---
title: Learn OIDC - Part 2 - JWT
excerpt: 
  Implement basic JSON Web Tokens (JWT) generation function which could be used in a later stage in a stub OIDC IdP
tags:
- java
- jwt
- oidc
- diy
toc: true
---

## Introduction
For a project I was working on, I was looking for a lightweight OIDC Identity Provider to be used as a _stub_ for our tests.
I couldn't find a lightweight one, so I tried to build one myself.

**WARNING:** The code shown in this series is for educational purposes only and is not meant to be used in production.
{: .notice--warning}

## JWT
One of the important building blocks in OAuth and OIDC is the concept of JSON Web Token or JWT (often pronounced as '_jot_').
A JWT is basically a JSON Web Signature (see: [Learn OIDC - Part 1 - JWS](2024-02-21-oidc-jws)) with a set of claims, represented as a JSON object as payload.
The advantage of JSON format for the claims is that it is fairly easy to parse and human-readable too.


### Format
The most common representation of JWTs is the _JWS Compact Serialization_.
The details of the JWT specification can be found in [RFC 7519](https://datatracker.ietf.org/doc/html/rfc7519).

### Claims
The claims in the JWT can be basically anything, but there is a set of _Registered Claim Names_.
All these registered claim names are shortened to just 3 characters in order to provide a compact representation.
Although all these _Registered Claim Names_ are considered optional for the JWT specification, they are heavily used in OAuth and OIDC implementations.

So for our implementation we're going to use (most of) these registered claim names as well.

- `iss` - the principal that issued the claims, should be a string/uri
- `sub` - the principal that is the _subject_ of the claims, should be a string/uri
- `aud` - an array of _audiences_ that the JWT is intended for (if there's just one audience, it could be a plain string/uri instead of an array)
- `exp` - the _expiration time_ identifies the expiration time after which the JWT must not be accepted for processing, should be a number representing seconds since the epoch
- `nbf` - the _not before_ identifies the time before which the JWT MUST NOT be accepted for processing, should be a number representing seconds since the epoch
- `iat` - the _issued at_ identifies the time at which the JWT was issued, should be a number representing seconds since the epoch
- `jti` - the _JWT ID_ provides a unique identifier for the JWT, should be a string and can be used to prevent detect token replays

Note that none of these are required from the perspective of the JWT spec.
Those are just some registered, commonly used claims.

In the light of OIDC some of these claims are required for proper OIDC implementation with ID tokens, as we'll see in a later post

## Implementation
A straight-forward way to implement a method to generate a JWT could be to re-use the JWS implementation from [part 1](2024-02-21-oidc-jws) and put some manually constructed JSON string as payload.

Something like:

```java
var payload = """
        {
        "iss": "fake-oidc-idp",
        "sub": "johndoe",
        "aud": "app-under-test",
        "exp": 1711285526,
        "nbf": 1711285226,
        "iat": 1711285226,
        "jti": "88771636-b8ec-49b1-bf07-03117f989a7a"
        }""".replace("\n", "");

return Jws.compactRS256(payload.getBytes(StandardCharsets.UTF_8), privateKey, keyId);
```

This will work for some hardcoded payload like this, but for implementing a lightweight OIDC stub we would need a bit more dynamic values.

Specifically the `exp`, `nbf`, and `iat` should be more dynamic. 
This could be done with some clever `String.format` setup, but it will get messy once we try to make the string fields dynamic.
Because then we'd need all kinds of escaping to keep the JSON body properly formatted.

So the best move would be to include some battle-tested JSON library to help with this. 
In this series we'll be using the well-known [Jackson](https://github.com/FasterXML/jackson) JSON library.

### Assembling claims
The access token claims we will be creating for this example should be dynamic. 
For example the `iat` (issued at) should be set to the moment of generation.
Next to that, tokens typically have some _token lifetime_ which is expressed as the timestamp at which the token expires (`exp` claim).
This also requires the current time **plus** the token lifetime.

Given that we already have fields or variables for `privateKey`, `keyId`, `issuer` (which is typically constant for the application) and `tokenLifetimeInSeconds`, the token generation method could look like:

```java
private static final ObjectMapper MAPPER = new ObjectMapper();
static {
    MAPPER.configure(SerializationFeature.INDENT_OUTPUT, false);
    MAPPER.setSerializationInclusion(JsonInclude.Include.NON_NULL);
}

public String createJwt(String subject, String audience) throws GeneralSecurityException, JsonProcessingException {
    var nowSeconds = System.currentTimeMillis() / 1000;
    var claims = Map.of(
            "iss", issuer,
            "sub", subject,
            "aud", audience,
            "exp", nowSeconds + tokenLifetimeInSeconds,
            "nbf", nowSeconds,
            "iat", nowSeconds,
            "jti", UUID.randomUUID().toString()
    );
    var payload = MAPPER.writeValueAsBytes(claims);
    return Jws.compactRS256(payload, privateKey, keyId);
}
```

This code first determines the current time in seconds since epoch and then assembles all the claims we want.
For now, we use a random `UUID` as JWT ID (`jti`), this will be sufficient for all intents and purposes of the stub OIDC server we're going to build.

As "API" for our JWT generation function we'll require just these 2 parameters:
- a `subject` containing some identifier of the user who is represented by the generated token
- an `audience` (application which is supposed to process the token)

## What's next?
Since we now  have some basic building blocks we could look into building the first steps of an OIDC login flow. 