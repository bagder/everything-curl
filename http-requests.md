# Modify the HTTP request

As described earlier, each HTTP transfer starts with curl sending a HTTP
request. That request consists of a request line and a number of request
headers, and this chapter details how you can modify all of those.

## Request method

The first line of the request includes the *method* - sometimes also referred
to as "the verb". When doing a simple GET request as this command line would
do:

    curl http://example.com/file

…the initial request line looks like this:

    GET /file HTTP/1.1

You can tell curl to change the method into something else by using the `-X`
or `--request` command-line options followed by the actual method name. You
can, for example, send a `DELETE` instead of `GET` like this:

    curl http://example.com/file -X DELETE

This command-line option only changes the text in the outgoing request, it
does not change any behavior. This is particularly important if you, for
example, ask curl to send a HEAD with `-X`, as HEAD is specified to send all
the headers a GET response would get but *never* send a response body, even if
the headers otherwise imply that one would come. So, adding `-X HEAD` to a
command line that would otherwise do a GET will cause curl to hang, waiting
for a response body that won't come.

When asking curl to perform HTTP transfers, it will pick the correct method
based on the option so you should only very rarely have to explicitly ask for
it with `-X`. It should also be noted that when curl follows redirects like
asked to with `-L`, the request method set with `-X` will be sent even on the
subsequent redirects.

## Request target

In the example above, you can see how the path section of the URL gets turned
into `/file` in the request line. That is called the "request target". That's
the resource this request will interact with. Normally this request target is
extracted from the URL and then used in the request and as a user you don't
need to think about it.

In some rare circumstances, user may want to go creative and change this
request target in ways that the URL doesn't really allow. For example, the
HTTP OPTIONS method has a specially define request target for magic that
concerns *the server* and not a specific path, and it uses `*` for that. Yes,
a single asterisk. There's no way to specicy a URL for this, so if you want to
pass a single asterisk in the request target to a server, like for OPTIONS,
you have to do it like this:

    curl -X OPTIONS --request-target "*" http://example.com/

## Anchors or fragments

A URL may contain an anchor, also known as a fragment, which is written with a
pound sign and string at the end of the URL. Like for example
`http://example.com/foo.html#here-it-is`. That fragment part, everything from
the pound/hash sign to the end of the URL, is only intend for local use and
will not be sent over the network. curl will simply strip that data off and
discard it.

## Customize headers

In a HTTP request, after the initial request-line, there will typically follow
a numbre of request headers. That's a set of `name: value` pairs that ends
with a blank line that separates the headers from the following request body
(that sometimes is empty).

curl will by default and on its own account pass a few headers in requests,
like for example `Host:`, `Accept:`, `User-Agent:` and a few others that may
depend on what the user asks curl to do.

All headers set by curl itself can be overriden, replaced if you will, by the
user. You just then tell curl's `-H` or `--header` the new header to use and
it will then replace the internal one if the header field matches one of those
headers, or it will add the specified header to the list of headers to send in
the request.

To change the `Host:` header, do this:

    curl -H "Host: test.example" http://example.com/

To add a `Elevator: floor-9` header, do this:

    curl -H "Elevator: floor-9" http://example.com/

If you just want to delete an internally generated header, just give it to
curl without a value, just nothing on the right side of the colon.

To switch off the `User-Agent:` header, do this:

    curl -H "User-Agent:" http://example.com/

Finally, if you then truly want to add a header with no contents on the right
side of the colon (which is a rare thing), the magic marker for that is to
instead end the header field name with a *semicolon*. Like this:

    curl -H "Empty;" http://example.com

## Referer

TBD

## User-agent

Default value is 'curl/7.54.1', as in 'User-Agent: curl/7.54.1'. Change that for your program's version.

You can use any value you like, using the option -A or --user-agent
plus the string to use or, as it's just a header,
-H 'User-Agent=string_to_use'.

## --time-cond

TBD
