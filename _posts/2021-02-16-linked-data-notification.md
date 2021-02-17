---
layout: post
title: Linked Data Notifications
date: 2021-02-16
---

[Linked Data Notifications](https://www.w3.org/TR/ldn/) [LDN] is a Social Web protocol
created by [Sarven Capadisli](https://csarven.ca/) and a W3C Recommendation
to pass notification messages to and from website. These messages can be referenced
by a URL and be shared and reused in other applications.

![ldn]({{site.baseurl}}/assets/images/ldn20210217/04.png)

Notifications are structured data as JSON-LD messages that get posted by a
_sender_ to the `inbox` of a _target_. These messages can then be read by
_consumer_ applications.

The LDN specification **doesn't** specify which **types of messages** are being sent over
the wire, **nor** what kind of **side effects** happen when a _target_ or _consumer_ receives
these messages. In this respect the LDN specification is more generic than the
[ActivityPub](https://www.w3.org/TR/activitypub/) [AP] specification, which adds
all kind of information flows to support Social Web applications ("tweeting",
federation of messages, following, unfollowing, liking, ...).

## Discovery of LDN endpoints

![discovery]({{site.baseurl}}/assets/images/ldn20210217/01.png)

When _sender_ want to know if a _target_ supports LDN, a `GET` or `HEAD` request
is sent to _target_ URL. In the HTTP response the _target_ adds a `Link` header
which points to the LDN `inbox`.

### Request

```
HEAD /repository HTTP/1.1
Host: my.institute.org
Accept: application/ld+json
```

### Response

```
HTTP/1.1 200 OK
Link: <http://my.institute.org/repository/inbox/>; rel="http://www.w3.org/ns/ldp#inbox"
```

There are more ways to discover the `inbox` (by content-negotiation and reading
  the RDF body, by processing RDFa in an HTML response, ...) you can read about those
  things in the LDN specifications.

## Sending a notification

![sending]({{site.baseurl}}/assets/images/ldn20210217/02.png)


A _sender_ sends a notification by `POST`ing a JSON-LD payload to the `inbox` of
the _target_. What _target_ does with these messages, or who can send messages
with what can of authentication mechanism, is not specified. In a typical scenario
some kind of validation of the message will be done. What ever happens, the
_target_ will mint a new URL for the notification and returns it in the HTTP Header
response (or there will be an HTTP 4XX in case of validation/authentication/other
  errors defined by other protocols that are built on top of LDN).

### Request

```
POST /inbox/ HTTP/1.1
Host: my.institute.org
Content-Type: application/ld+json
Content-Language: en

{
  "@context": "https://library.standards.org/publicationTypes",
  "@id": "",
  "@type": "JournalArticle",
  "title": "My article on Linked Data Notifications",
  "date": "2021",
  "creator": "https://hochstenbach.solidcommunity.net/profile/card#me"
}
```

### Response

```
HTTP/1.1 201 Created
Location: http://my.institute.org/inbox/F314FB2B-1687-4CD7-BFB3-AC3DCA5BA7AA
```

## Reading notifications

![reading]({{site.baseurl}}/assets/images/ldn20210217/03.png)

A _consumer_ can read the inbox (maybe after being authenticated) by issuing
a `GET` request to `inbox` of the _target_. The `inbox` will respond with
a `http://www.w3.org/ns/ldp#contains` predicate in some way or another serialized
as a JSON-LD document. This `contains` provides a list or URLs pointing to
notification messages. It is up to the implementation which notification messages
will be returned and in what format. There are no paging methods
specified by LDN, but they can be implemented using other standards (see:
  [ActivityStreams](https://www.w3.org/TR/activitystreams-core/),
  [Linked Data Platform](https://www.w3.org/TR/ldp/) for more information how
  paging can be implemented).


### Request

```
GET /inbox/ HTTP/1.1
Host: my.institute.org
Accept: application/ld+json
```

### Response

```
HTTP/1.1 200 OK
Content-Type: application/ld+json

{
  "@context": "http://www.w3.org/ns/ldp",
  "@id": "http://my.institute.org/inbox/",
  "contains": [
    "http://my.institute.org/inbox/F314FB2B-1687-4CD7-BFB3-AC3DCA5BA7AA",
    "http://my.institute.org/inbox/52F1A66D-77FD-4597-93FA-8DCE8EF6A79F"
  ]
}
```

By following the links in the `contains` predicate the JSON-LD (or other
  serializations using content negotiation) notifications can be read.
