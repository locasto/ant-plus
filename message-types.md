Based on Rev5.1 of the document D00000652_ANT_Message_Protocol_and_Usage_Rev_5.1.pdf
(April 2014) Appendix A

The ANT+ protocol includes a number of message categories:

 - configuration messages
 - notifications
 - control messages
 - data messages
 - channel event / response messages
 - requested response messages
 - test mode

Of these, the most immediately interesting from a security perspective
are the control messages, as well as messages from other categories
that either (a) claim to provide some type of security feature or (b)
provide arbitrary read/write primitives.

[High Level Observations]

 * The protocol supports variable length messages and message types
 * A number of messages include length parameters
 * A few messages support the ability for aribtrary read/write of the client memory space

[Examples of Dangerous Messages and Message Features]

This is a current list of messages that seem weird or risky to me;
because I don't yet fully understand the protocol, I may be wrong in
singling out these messages. This list is a work in progress.

Config Message. Set Encryption Key. Length 17. Id: 0x7E. 
weirdness: Includes 16 byte key in plaintext.

Config Message. Load/Store Encryption Key. Length 3 or 18. Id: 0x83.
weirdness: includes an index into Non-volatile memory.
weirdness: includes a final field that is *either* a volatile key index *or* an encryption key

Config Message. Config Encryption Id List. Length: 3. Id: 0x5A. 
weirdness: list size and list type specified in message

Control Message. Reset System. Length: 1. Id: 0x4a.
weirdness: unauthenticated (but ignorable).

Control Message. Open Channel. Length 1. Id: 0x4b.
weirdness: unauthenticated (but ignorable?).

Control Message. Close Channel. Length 1. Id: 0x4c.
weirdness: unauthenticated (but ignorable?).

Control Message. Request Message. Length: varies. Id: 0x4d.
weirdness: variable length, based on ???
weirdness: no idea what it does
weirdness: one use is the NVM request.

Control Message. Open Rx Scan Mode. Length: varies. Id: 0x5B.
weirdness: variable length, based on ???
weirdness: unauthenticated?

Data Messages. Broadcast data. Length: varies. Id: 0x4e
Data Messages. Acknowledged data. Length: varies. Id: 0x4f
Data Messages. Burst transfer data. Length: varies. Id: 0x50
weirdness: optional payload of extended data (optional feature. can has fingerprint?)

Data Messages. Advanced Burst data. Length: varies. Id: 0x72.
weirdness: payload is N bytes, N unspecified or specified in another message (inter-msg dependency!)

Requested Response Messages.
weirdness: 0x3e (Ant version) and 0x54 length varies. Ant version is "N" bytes.
weirdness: 0x78 has 2 interpretations, each of different length. Includes 'max packet length' value.

Requested Response Messages. User NVM. Length: varies. Id: 0x7C. (See also 0x4D)
weirdness: variable length
weirdness: request arbitrary client (user-level) memory content!
doc: "The starting address and size of the data correspond to the
values specified in the Request Message. Read block size may be
arbitrary up to the device specific maximum. It is not possible to
read beyond any address in the user space."
doc: for msg 0x4D: "These arguments are only used when requesting the
User NVM message. The combination of addr and size must not exceed the
user NVM space of the device"


Requested Response Messages. Encryption Mode Params. Length: varies. Id: 0x7D.
weirdness: 3 valid lengths