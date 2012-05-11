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
command itself takes one parameter, the password. This command is sent
at client introduction, before any other commands.

.. c:function:: void irc_nick(const char * nick)

This function accepts a string containing a nickname. The nickname
will be sent to the server using the NICK command.

The NICK command is specified in RFC1459, section 4.1.2. The IRC
command itself takes one parameter, the nickname. This command, when sent
at client introduction, will set the nickname Sigyn attempts to use.
When sent post-introduction, the client will attempt to change its nickname
to the specified option.

.. c:function:: void irc_user(const char * user, const char * host, const char * server, const char * real)

This function accepts four strings. The first being the username
(AKA, the ident) used by Sigyn. The second being a string containing
the system's hostname. The third string is the hostname the client is
connecting to the server by; the fourth is the GECOS, or real name,
of the client.

The USER command is specified in RFC1459, section 4.1.3. The IRC
command itself takes four parameters, discussed above. In practise, however,
the second and third parameters are generally considered pointless and/or
unused, commonly being replaced with asterisks or various other data.

.. note::

    RFC1495, section 4.1.4 specifies the SERVER command. This is not
    included in Sigyn as it is for servers only, not clients.

.. c:function:: void irc_oper(const char * user, const char * password)

This function accepts two strings. The first is the username to attempt to
obtain operator privilages using, the second being the password used in said
attempt.

The OPER command is specified in RFC1459, section 4.1.5
