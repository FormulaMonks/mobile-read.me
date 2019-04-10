# Commit Style

Try to commit self-contained units of work that are in a stable state, if possible.

Information about commits is divided into two sections: 

### Subject

The commit subject is the main item describing the object of the changes being commited. Keep it short so that it can be entirely displayed in most code versioning tools. The format should be:

`[<Ticket Number>] - <Summarized description of the changes>`

For example:

`[CM-47] - Added Firebase Cloud Messaging pod`

### Detailed Description

The detailed description is minimally used to include the title of the ticket that is being worked on. Additional information can be provided below the ticket's title if necessary:

```
[<Ticket Title>]

Additional description
```

For example:

```
[Register for Push Notifications]

Added Firebase/Messaging to podfile and Firebase initialization code.
```
