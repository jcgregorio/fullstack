# Chapter 1 - HTTP

When you use a browser, the browser uses a protocol, HTTP, to talk to the
server. HTTP can become very complex, but it starts out being very simple.
First, the client (browser) sends some simple lines of text to the server
followed by a blank line. The server response with some simple lines of text,
followed by a blank line, and then the body of the response from the server.

Let's start with the `netcat` command, this is a simple tool for talking to a
server over the network.

## Exercise 1:

Use `netcat` to talk to Google. The following command launches `netcat` and
directs it to talk to port 80 on the `google.com` server.

```
echo -e "GET / HTTP/1.0\n\n"  | nc google.com 80

```

[Explain Shell](https://explainshell.com/explain?cmd=echo+-e+%22GET+%2F+HTTP%2F1.0%5Cn%5Cn%22++%7C+nc+google.com+80).

What you get back should look something like:

```
HTTP/1.0 200 OK
Date: Sun, 08 Aug 2021 20:10:04 GMT
Expires: -1
Cache-Control: private, max-age=0
Content-Type: text/html; charset=ISO-8859-1
P3P: CP="This is not a P3P policy! See g.co/p3phelp for more info."
Server: gws
X-XSS-Protection: 0
X-Frame-Options: SAMEORIGIN
Set-Cookie: 1P_JAR=2021-08-08-20; expires=Tue, 07-Sep-2021 20:10:04 GMT; path=/; domain=.google.com; Secure
Set-Cookie: NID=220=Hp-nEC1xW1hM-S9VDJlNXC5U-Y635TOsr2QtXd5NZ_N80oVO-iTFUBrZWFvls7IALnhCCdfGAreRJJjkvUyAslOLaks4XFcA2sDOMiG9ligYyYk3Xv2TFX0W7Syom7MsQXqUZ-rGxm0jy0u_IF4t8KXKI7sZ_aXcDAc_r6PEIsI; expires=Mon, 07-Feb-2022 20:10:04 GMT; path=/; domain=.google.com; HttpOnly
Accept-Ranges: none
Vary: Accept-Encoding

<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="en"><head><meta content="Search the world's information,...
```

Note that we only sent a single line to `google.com` and it returned a web page.

That first line is called the Request line and it contains the Method to use,
the address being requested, and the version of HTTP being used. We can
optionally add more headers after the Request line.

The response from the server looks similar, it has a Response line, which
contains the version of HTTP being used, a status code, and then a status
string, followed by headers:

```
HTTP/1.0 200 OK
Date: Sun, 08 Aug 2021 20:10:04 GMT
Expires: -1
```

But wait, what is `google.com` and what is a port (80)?

The `nc` command talks TCP/IP, which is a way of sending information between two
machines and for it to work we need the IP address of the server we want to talk
to.

## Exercise - Look up the address of `google.com`.

```
nslookup google.com
```

Which should return something like:

```
Server:         127.0.0.1
Address:        127.0.0.1#53

Non-authoritative answer:
Name:   google.com
Address: 142.251.45.110
Name:   google.com
Address: 2607:f8b0:4004:80a::200e
```

Now re-run our command above, substituting in the IP address for `google.com`:

```
echo -e "GET / HTTP/1.0\n\n"  | nc 142.251.45.110 80

```

You should get the same response as in the first example, we've just helped `nc`
out by doing the DNS lookup of `google.com`, i.e. translating the name
`google.com` into an IP address.

And what is a port? We'll a single server might want to host a bunch of
different services on a single machine, such as a web server, an SSH server,
email server, etc. And each of those services speak a different protocol, so
TCP/IP allows you to separate traffic to a server to go to different ports. How
many ports are there? 65535.

For HTTP the default port is 80.

## Exercise - Do a Google search using `netcat`.

```
 echo -e "GET /search?q=rfc+2616 HTTP/1.0\n\n"  | nc google.com 80
```
