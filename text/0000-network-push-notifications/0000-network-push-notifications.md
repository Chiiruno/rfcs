# Network Push Notifications

- Status: proposed
- Type: new feature
- Related components: vault, client libs
- Start Date: 05-09-2018
- Discussion: (fill me in with link to RFC discussion - shepherd will complete this)
- Supersedes: N/A
- Superseded by: N/A

## Summary

In this RFC, we propose a feature which will allow APPs to send network push notifications to the SAFE network when uploading content, optionally with a unique indentifier specific to this notification.  
APPs can then optionally watch for these notifications, which must contain an array of pointers to the uploaded chunks on the network, and use this data.  
This will allow dynamic user-generated content service, between different APPS in live-time, in an APP-friendly way.

## Conventions

- The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](http://tools.ietf.org/html/rfc2119).

## Motivation

I, personally, am doing this since I am planning on an imageboard<sup>[1]</sup> using the SAFE network and want to add liveposting<sup>[2]</sup>.  
We should be doing this to enable dynamic content that other APPs can watch for and use via the notifications.  
I expect the outcome of this to be APPs that can upload content, send a network push notification, and have other APPs watching for that notification to use it, resulting in beautiful dynamically user-content generated sites, APPs, etc, in a live fashion.  
This will allow great interoperability between different APPs which may depend on each other and/or user-generated content.

### Index

1. An imageboard is like a text BBS, but with images
2. Liveposting is where you type in the post form, and it shows what you're typing, only in the post form, to the other clients

## Detailed design

### Definitions

- **Pointer**: Memory address or network address, whichever is more appropriate
- **Network push notification**: A notification with an optional notification-specific unique identifier and an array of pointers to the location of relevant network chunks

### Data structures

```rust
struct NetworkNotification;
```

Should contain:

- Unique identifer hash, which should be null/nil/none if not specified
- Pointer to notification chunk on the SAFE network
- Pointer array to related chunks on the SAFE network

### Example

APP A uploads content, sends a network push notification containing a pointer to related chunks on the network, with an optional unique identifier specific to this notification, which APP B is watching for, and possible said unique identifier, and uses accordingly.

### Possibilities

Concerning uploads in relation to notifications, there are two options.

1. Automatically send the notification as part of the upload process
2. Optionally send the notification after the upload process is complete

Concerning the notifications themselves, there are three options.

1. They can be anywhere on the network, with a list of pointers to those notifications at some static start location on the network. This list of pointers will need to be dynamically allocated on the network, or have a pointer to the next notification chunk
2. They can be anywhere on the network, with a network-push-notification-specific identifier that APPs can iterate and find over the network
3. They can be on a static start location of the network, where the APP can iterate over them. This space will need to be dynamically allocated, or have a pointer to the next notification chunk

The client libs will need to have the ability to send these notifications, as well as be capable of looking them up.

### Corner cases

**TODO**: Find possible corner cases.

## Drawbacks

Possibility of leaking IP addresses or other users information in the process if not implemented carefully.
Possible extra network load.

## Alternatives

The impact of not doing this is less APP interoperability between different APPs and user-generated content.

## Unresolved questions

Algorithms involved.  
More specific data structures and functions.
