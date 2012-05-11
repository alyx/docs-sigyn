IRC Command Interface
=====================

Sigyn offers a rich interface to the various standardised commands of
the `Internet Relay Chat <http://en.wikipedia.org/Internet_Relay_Chat>`
protocol. Sigyn's core IRC interface implement all commands specified
in `RFC 1459 <http://tools.ietf.org/rfc/rfc1459.txt>`.

.. c:function:: int raw(char * line, ...)

This function takes a printf-style format string (in the line variable)
as well as an undefined amount of additional arguments to be implanted
into the formatter string.

This function then adds the formatted string to the send queue and
returns the length of the finished string.


.. c:function:: void sigyn_introduce_client(char * nick)

This function introduces the Sigyn client to the uplink IRC server in
the method specified by RFC1459. It calls sigyn_hostname() to retreive
the system hostname, then sends the NICK command (Using the nickname
specified in the nick variable), followed by the USER command (Using
the nick variable for 'user', the system hostname for 'host', the uplink
hostname for 'server', and the client's GECOS for 'real name').

IRC Commands
------------

.. c:function:: void irc_pass(const char * password)

This function accepts a string containing a password. This password
will be sent to the server using the PASS command.

The PASS command is specified in RFC1459, section 4.1.1. The IRC
command itself takes on parameter, the password. This command is sent
at client introduction, before any other commands.

