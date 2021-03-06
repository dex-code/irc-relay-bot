# Links
* relay-bot 
* http://sf.net/projects/relay-bot/ 
* https://github.com/freiheit/irc-relay-bot 
* http://freshmeat.net/projects/relay-bot/ 
* http://fandanta.org/ 

# NOTE

This has not been used by the main author in almost a decade. The key
library it depends on has also been abandoned a decade ago. This is not so
much code you can run as it is an interesting bit of archaelogy.

## REASON

Relay-bot is a program we initially wrote because our group of friends that
hang out on one channel could never seem to all stay on EFnet, and even if
we could EFnet was splitting so badly at the time that it was unusable. 
However, we couldn't readily switch to another network because not everybody
is always online and a few people wanted to stick with EFnet because there
were other friends of theirs still there.  So, relay-bot was born.  It
allowed us to split our channel across 2 (actually, 3 or 4) different IRC
networks fairly effectively.


# INSTALL

Relay-bot is written in Perl, so you'll need a reasonably recent copy of
Perl.  You'll also need a copy of Net::IRC.

http://search.cpan.org/~apeiron/Net-IRC-0.79/
https://sourceforge.net/projects/net-irc/
https://sourceforge.net/projects/relay-bot/files/net-irc/0.73-patched/

There's currently no real installation routine; just untar the tarball, edit
"relay-bot.config" and run "relay-bot.pl".


# USAGE

run relay-bot.pl, possibly in a loop like `while sleep 10 ; do
./relay-bot.pl; done`.  Or `nohup ./relay-bot.pl >> /var/log/relay-bot.log
2>&1`.  Or combine the two into: 
```
  nohup sh -c "while sleep 10 ; do ./relay-bot.pl >> /var/log/relay-bot.log 2>&1 ; done" &
```

It will sign onto all the servers in %hosts and join all the channels in
@relay_channels.

For any given channel, it will then relay all messages from one server to
all other servers, prefixing with the nick (and network name) of the the
person speaking.

So, for channel #foochan on EFnet (abbreviated as 'ef') and
irc.openprojects.net (abbreviated as 'op'), with Bob on ef and Sue on op,
here's a conversation through a bot named after the channel:

From EFnet:
```
<Bob> Hi everybody, how's it going?
<foochan> <Sue@op> Oh, I'm doing good.  How about you?
<Bob> Just fine.  Love the relay bot.
<foochan> <Sue@op> Yeah, me too
```

From openprojects:
```
<foochan> <Bob@ef> Hi everybody, how's it going?
<Sue> Oh, I'm doing good.  How about you?
<foochan> <Bob@ef> Just fine.  Love the relay bot.
<Sue> Yeah, me too
```

Sue can private message Bob by sending a message to foochan that starts with
"<Bob@ef>", like "/msg foochan <Bob@ef> hi there".  The <>s are optional or
they can be like >this< instead.  If you leave out the @net part the message
will get sent to that nick on every network the bot is on, except the one it
came from.

In the channel, you can say "/names" to get a list of who's on the channel
on other networks.

If the bot has ops, when somebody changes the topic, it will set that topic
on the other servers (and send a message to the channel regarding the topic)

People joining the channel, leaving the channel or signing off of IRC are
all passed through, too.


## SPECIFICS

There's a primitive command set for a very limited amount of things.

/command will give public (in channel) results

^command will give private results

"/names" (or ^names) in a channel will list who's on that channel in all other
networks  "/say /names" might be what you need to actually use in your IRC
client.

"/join #channel net" will join #channel on the network abbreviated as "net".
(you'll probably want to do that in a private message, ie, 
"/msg relaybot /join #channel net" or "/say /join #channel net".

"/join #channel" will join #channel in all networks.

"/part #channel" (private or public) or "/part #channel net"; leaves
channel.

"/quit" will cause it to sign off of IRC and exit.

"/restart" will cause it to sign off of IRC and then restart itself. 
(particularly useful for testing new configurations, etc.)
