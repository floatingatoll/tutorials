# IRC Channel Quickstart

Last updated: May 2018

## Open Chanserv and Nickserv conversations.

Open a private conversation to the automated IRC user `Chanserv`.

### Sending commands 

In this guide, `Chanserv: INFO #channel` indicates that you should enter the string `INFO #channel` into the private conversation to Chanserv.

## Find out if anyone owns and/or uses the channel 

`Chanserv: INFO #example` will either return `isn't registered` or `Founder: someone`. If it isn't registered, you're okay to proceed. If it is, you'll need to reach out to either the founder or the IRC admins to proceed.

## Assumptions checkpoint

Make sure you can join the channel, and that it's not in use for a different purpose. IRC admins can often reclaim an abandoned channel, but they generally do not repurpose active channels.

Your IRC client must be signed in to Nickserv from this step onward. Setting this up is beyond the scope of the channel guide.

## Register the channel

`Chanserv: REGISTER #example` will either succeed or fail. If it succeeds, you are now the channel admin.

### Already registered?

Contact the founder shown by `INFO` above and ask them to run the command `Chanserv: QOP ADD your_irc_handle`. If they are not available or responsive, ask the IRC admins to make you the founder.

## Add channel admins

Once you have control of the channel, you may specify multiple founders using `Chanserv: QOP ADD irc_nickname`. If this fails for any given IRC nickname, they'll probably need to register with Nickserv.

### List channel admins

If you're taking over an existing channel, or if time has passed since you took ownership of a channel, you'll want to look and see who has elevated privileges.

`Chanserv: FLAGS #example LIST` will return a numbered list of channel access grants. The `Mask` column will typically be IRC nicknames.

### Remove an admin

To remove someone from this list, `Chanserv: FLAGS #example MODIFY one_mask -*` will clear all entries for a given `Mask` from the list.

## Restricted channels

There are a lot of possible channel restrictions. We typically use two forms of restriction, which may be combined if desired.

### Invite-only

People must be invited to the channel or they will be unable to join. TODO

### Password-protected

TODO

### Chat bots

Chat bots need to be able to join a channel to interact with it. `Chanserv: FLAGS #example bot_name +Gi` will grant a given `bot_name` permission to use two additional Chanserv commands, `INVITE #example` and `GETKEY #example`.

When the bot needs access to an invite-only channel, TODO

## TODO

## FAQ

Q: The commands aren't working on my Chanserv and Nickserv.
A: These services can vary between IRC networks. This guide was written for Mozilla's IRC network and may not translate perfectly to others.

Q: Can I just use /msg nickserv instead of opening a query window?
A: You can, but in virtually all IRC clients, a single character typo will send your command to your current conversation. This guide assumes you have two open conversations to Chanserv and Nickserv.
