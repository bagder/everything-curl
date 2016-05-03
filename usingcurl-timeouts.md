# Timeouts

Network operations are in their nature rather unreliable or perhaps fragile
operations as they depend on a set of services and networks to be up and
working for things to work. The availability of these services can come and go
and the performance of them may also vary greatly from time to time.

The design of TCP even allows the network to get completely disconnected for
an extended period of time without it necessarily getting noticed by the
participants in the transfer.

The result of this is that sometimes internet transfers take a very long
time. Futher, most operations in curl has no time-out by default!

## Maximum time allowed to spend

Tell curl the maximum time with `-m / --max-time`, in seconds, that you allow
the command line to spend until curl exits with a timeout error code
(28). When the set time has been spent, curl will exit no matter what is going
on at that moment - including if it is transferring data. It really is the
maximum time allowed.

The given maximum time can be specified with a decimal precision; `0.5` means
500 milliseconds and `2.37` equals 2370 milliseconds.

Example:

    curl --max-time 5.5 https://example.com/

## Never spend more than this to connect

`--connect-timeout` limits the time curl will spend trying to connect to the
host. All the necessary steps done before the connection is considered
complete has to be completed within the given time fram. Failing to connect
within the given time will cause curl to exit with a timeout exit code (28).

The given maximum connect time can be specified with a decimal precision;
`0.5` means 500 milliseconds and `2.37` equals 2370 milliseconds.

    curl --connect-timeout 5.5 https://example.com/

## Transfer speeds slower than this means dead

Having a fixed maximum time for a curl operation can be cumbersome, especially
if you for example do scripted transfers and the file sizes and transfer times
vary a lot. A fixed timeout value then needs to be set unnecesary high to
cover for worst cases.

As an alternative to a fixed time-out, you can tell curl to abandon the
transfer if it gets below a certain speed and stays below that threshold for a
specific period of time.

For example, if a transfer speed goes below 1000 bytes per second during 15
seconds, stop it:

    curl --speed-time 15 --speed-limit 1000 https://example.com/

## Keep connections alive

curl enables TCP keep-alive by default. TCP keep-alive is a feature that makes
the TCP stack send a probe to the other side when there's no traffic, to make
sure that it is still there and "alive". By using keep-alive, curl is much
more likely to discover that the TCP connection is dead.

Use `--keepalive-time` to specify how often in full seconds you'd like the
probe to get sent to the peer. The default value is usually set to 7200, which
as you know is two full hours.

Sometimes this probing disturbes what you're doing and then you can easily
disable it with `--no-keepalive`.