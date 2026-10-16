# FindIt

FindIt is a free web app that helps people recover missing belongings by connecting those who find lost items with their rightful owners.

> **Status:** Planning. This README defines the scope of version one so we only build what we've agreed to build.

## How It Works

1. **Report**: someone who finds an item submits a report. The item gets a unique ID and appears in the public feed.
2. **Browse**: someone who lost something searches the feed for a match.
3. **Claim**: if their item has been reported, they submit a claim on that item with a description of it and their contact details.
4. **Verify**: the reporter gets a mail with the item description and compares the claim against the item found to confirm it's a match.
5. **Reunite**: once verified, reporter can then reach out to the claimer.

If a lost item hasn't been reported, there's nothing to claim. You can only claim an item that's already in the feed.

## Version One Scope

### Pages

1. **Feed**: browse and search reported items, and start a claim.
2. **Report**: submit a found item.

No login or accounts. Anyone can report or claim.

### Reporting a Found Item

Fields:

- **Item name**
- **Description**: what was found and, optionally, where. This is public.
- **Contact details**: email required .

Every report gets a unique ID.

### The Feed

- Filter by date
- Filter by item
- Search bar

Once a claim on a report is confirmed as a match, that report no longer appears in the feed.

### Claiming an Item

- Can only be done on an item that has been reported.
- The claimant provides a description of their item and their contact details (email required, phone optional).
- Every claim is linked to the report it was made on, so a report is always connected to all the claims made on it. This is how claims and reports are tied together without user IDs.
- Claims are **in reporter's mailbox**. Nobody else can see them.

### Email Fields

Email is how contact gets exchanged, so a typo means the exchange fails. Both the report form and the claim form therefore:

- Ask for the email **twice** (enter it, then confirm it).
- Show an alert reminding the user to make sure their email is typed correctly.

### Reporter

- View all claims linked to the report that the reporter made.
- Compare each claim to the found item and mark it as a match or not.

### Contact Exchange
the reporter gets the claimant's contact automatically and gets to reach out if their claim matches what was found

## Out of Scope for Version One

- User accounts or login
- Anything not listed above

If an idea isn't in this document, it doesn't go in version one.

## Open Questions

- **Emails that still fail.** Even with the double entry, an email could bounce. Should the admin be able to see whether the emails were sent?
- **Status page vs. email.** Should people be able to check a claim's status with an ID on a page? That would be a third page, which breaks the two-page rule. For now, email only.

