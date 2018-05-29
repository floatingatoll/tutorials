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

IRC channels permit anyone to join, whether public or secret. Restrictions may be configured to secure the channel.

### What's the best approach?

Set a password on the channel so that anyone with the password can join.

### Possible restrictions

Typically most channels use a single restriction only, but these may be combined as needed.

#### Password-protected

People must have the password to join the channel or they will be unable to join. Most clients remember the password and will be able to rejoin until it is changed.

TODO

#### Invite-only

People must be invited to the channel or they will be unable to join. If their client disconnects, they will need to be invited to rejoin. If they have sufficient access levels, they may ask Chanserv to invite them in.

TODO

#### Restricted-members

People must be on the Chanserv access list for the channel, TODO

TODO about insufficient vs. other two

TODO

### Chat bots

Chat bots need to be able to join a channel to interact with it. `Chanserv: FLAGS #example bot_name +Gi` will grant a given `bot_name` permission to use two additional Chanserv commands, `INVITE #example` and `GETKEY #example`.

#### Joining restricted channels

TODO: too much detail, not enough guidance

Ensure that your code is prepared for this scenario by verifying that it can `JOIN #example` successfully. Some IRC frameworks do not throw exceptions when this occurs.

Ensure that your bot is registered with Nickserv and is configured to sign in with Nickserv. Using SASL to do so is strongly encouraged, but the classical `Nickserv: IDENTIFY a_password` method is acceptable.

If the channel is password-protected, the bot may send `Chanserv: GETKEY #example` to retrieve the channel key, and then `JOIN #example the_key` to join the channel. This has a slight possibility of a race condition if the channel key is altered between the `GETKEY` and `JOIN` commands.

If the channel is invite-only, the bot may send `Chanserv: INVITE #example` to be invited into the channel, and then `JOIN #example` to join the channel. The invite lasts for a short period of time, so as long as the `JOIN` is sent in a timely manner the bot should be able to enter.

If the channel is restricted-members, the bot may simply `JOIN #example` to join the channel once it is added to the approved members list by an admin. If other restrictions are applied to the channel, the bot must satisfy those as well.

If a given channel is both password-protected and invite-only, the bot need only use `Chanserv: INVITE #example` to join the channel. While redundant, it's permitted to use both `GETKEY` and `INVITE`.

## TODO

## FAQ

Q: The commands aren't working on my Chanserv and Nickserv.
A: These services can vary between IRC networks. This guide was written for Mozilla's IRC network and may not translate perfectly to others.

Q: Can I just use /msg nickserv instead of opening a query window?
A: You can, but in virtually all IRC clients, a single character typo will send your command to your current conversation. This guide assumes you have two open conversations to Chanserv and Nickserv.
