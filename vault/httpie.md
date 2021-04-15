---
title: httpie
category: CLI
layout: 2017/sheet
weight: -3
updated: 2020-07-05
description: |
  $ http POST http://example.com name="John" Host:example.com — JSON, cookies, files, auth, and other httpie examples.
---

### Introduction
{: .-intro}

[HTTPie](https://httpie.org/) is a command-line HTTP client.

- [HTTPie website](https://httpie.org/) _(httpie.org)_
- [HTTPie documentation](https://httpie.org/docs) _(httpie.org)_
- [Try it online](https://httpie.org/run) _(httpie.org)_

### Parameters

```bash
$ http POST http://example.com/posts/3 \
    Origin:example.com \  # :   HTTP headers
    name="John Doe" \     # =   string
    q=="search" \         # ==  URL parameters (?q=search)
    age:=29 \             # :=  for non-strings
    list:='[1,3,4]' \     # :=  json
    file@file.bin \       # @   attach file
    token=@token.txt \    # =@  read from file (text)
    user:=@user.json      # :=@ read from file (json)
```

### Forms

```bash
$ http --form POST example.com \
    name="John Smith" \
    cv=@document.txt
```

### Raw JSON

```bash
$ echo '{"hello": "world"}' | http POST example.com/post
```

### Options

#### Printing options

```bash
-v, --verbose            # same as --print=HhBb --all
-h, --headers            # same as --print=h
-b, --body               # same as --print=b
    --all                # print intermediate requests
    --print=HhBb         # H: request headers
                         # B: request body
                         # h: response headers
                         # b: response body
    --pretty=none        # all | colors | format
    --json | -j          # Response is serialized as a JSON object.
```

#### Authentication

```bash
    --session NAME
-a, --auth USER:PASS
    --auth-type basic
    --auth-type digest
```

#### Session

```bash
    --session NAME       # store auth and cookies
    --session-read-only NAME
```

#### Downloading

```bash
-d, --download           # like wget
-c, --continue
-o, --output FILE
```

#### Others

```bash
-F, --follow             # follow redirects
    --max-redirects N    # maximum for --follow
    --timeout SECONDS
    --verify no          # skip SSL verification
    --proxy http:http://foo.bar:3128
```

### References

* <https://github.com/jakubroztocil/httpie>


http [flags] [METHOD] URL [ITEM [ITEM]]

Custom HTTP method, HTTP headers and JSON data:

$ http PUT pie.dev/put X-API-Token:123 name=John
Submitting forms:

$ http -f POST pie.dev/post hello=World
See the request that is being sent using one of the output options:

$ http -v pie.dev/get
Build and print a request without sending it using offline mode:

$ http --offline pie.dev/post hello=offline
Use GitHub API to post a comment on an issue with authentication:

$ http -a USERNAME POST https://api.github.com/repos/httpie/httpie/issues/83/comments body='HTTPie is awesome! :heart:'
Upload a file using redirected input:

> http pie.dev/post < files/data.json

Download a file and save it via redirected output:

$ http pie.dev/image/png > image.png
Download a file wget style:

$ http --download pie.dev/image/png
Use named sessions to make certain aspects of the communication persistent between requests to the same host:

$ http --session=logged-in -a username:password pie.dev/get API-Key:123
$ http --session=logged-in pie.dev/headers