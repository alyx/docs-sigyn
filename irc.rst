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

.. note::

        Connection registration proceeds as follows:
        1. PASS
        2. NICK
        3. USER

IRC Commands
------------

Connection Registration
^^^^^^^^^^^^^^^^^^^^^^^

.. c:function:: void irc_pass(const char * password)

This function accepts a string containing a password. This password
will be sent to the server using the PASS command.

The PASS command is specified in RFC1459, section 4.1.1. The IRC
command itself takes one parameter, the password. This command is sent
at client introduction, before any other commands. Sending this command
post-registration will result in an ERR_ALREADYREGISTERED response from
the server.

.. c:function:: void irc_nick(const char * nick)

This function accepts a string containing a nickname. The nickname
will be sent to the server using the NICK command.

The NICK command is specified in RFC1459, section 4.1.2. The IRC
command itself takes one parameter, the nickname. This command, when sent
at client introduction, will set the nickname Sigyn attempts to use.
When sent post-introduction, the client will attempt to change its nickname
to the specified option. On error, the server will respond with either 
ERR_NICKNAMEINUSE, when the nick is currently being used, ERR_ERRONEUSNICKNAME,
when the nickname contains disallowed characters, or ERR_NICKCOLLISION, when
two users attempt to switch to the same nickname at the same time, or when a
server introduces a client with the same nickname as an existing client, and it
generates a nickname collision.

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
Use of this command post-registration will solicit an ERR_ALREADYREGISTRED
response from the server.

.. note::

    RFC1495, section 4.1.4 specifies the SERVER command. This is not
    included in Sigyn as it is for servers only, not clients.

.. c:function:: void irc_oper(const char * user, const char * password)

This function accepts two strings. The first is the username to attempt to
obtain operator privilages using, the second being the password used in said
attempt.

The OPER command is specified in RFC1459, section 4.1.5. The IRC
command itself takes two parameters, discussed above. Successful usage
of this command will result in an RPL_YOUREOPER response from the server.
Unsuccessful usage will result in either ERR_NOOPERHOST (If you just aren't
an operator) or ERR_PASSWDMISMATCH (For incorrect passwords).

.. c:function:: void irc_quit(const char * message)

This function accepts one string. This string contains the message
to be sent along with the QUIT command. This message is optional and
may be set to NULL if a specific message is not desired.

The QUIT command is specified in RFC1459, section 4.1.6. The IRC
command itself takes one optional parameter. When QUIT is sent
without a specified quit message (Or, on many IRCds, within X
amount of time after connecting), the quit message is set to a
predefined message set by the IRCd.

.. c:function:: void irc_squit(const char * server, const char * message)

This function accepts two strings. The first specifies which server
to delink from the network; the second specifies a message to be sent
along with the delinking.

The SQUIT command is specified in RFC1459, section 4.1.7. The IRC
command itself takes two parameters, discussed above. SQUIT is only
usable by clients set as IRC operators. When used by a non-oper client,
the server will respond with ERR_NOPRIVILEGES. If SQUIT is used to
attempt to remove a nonexistent server, an ERR_NOSUCHSERVER response
will result.

Channel Operations
^^^^^^^^^^^^^^^^^^

.. c:function:: irc_join(const char * channel, const char * key)

This function takes two strings. The first specifies either a single channel
or a comma-delimited list of channels (e.g., #a,#b,#c). The second either is
NULL, for no channel key, or is a (possibly comma-delimited list of) key(s) for
the respective channel in the first parameter.

The JOIN command is specified in RFC1495, section 4.2.1. The IRC
command itself takes one parameter and one optional parameter, both of which
are discussed above. If the channel is set to be invite-only (the +i channel
mode on many IRCds), the IRCd will response with ERR_INVITEONLYCHAN. If the
channel is set to limit the amount of users (+l channel mode on many IRCds)
and that limit has been reached, the server will respond with an ERR_CHANISFULL
response.
