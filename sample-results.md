# Sample results

These fictional examples come from n8n runs on 6 November 2026. Every alert was saved as `pending review`.

## Renewal within 30 days

**Account `A022` Clover Pharmacy, score 12**

Reasons: Usage down 33% in 30 days (+3); Only 16% of users active (+2); 9 open tickets (+2); No contact for 64 days (+2); Renewal in 13 days (+3)

**Subject:** Your 19 November renewal

> Hi Clover Pharmacy team,
>
> Your renewal is on 19 November, so this is a good time to step back and look at where things stand.
>
> Would a short written summary of what you use today and what is still open be useful before the renewal? A simple yes is enough.
>
> Best,
> Customer Success

**Internal note:**

> Email topic: their upcoming renewal.
> Before sending: Confirm the renewal date and contract terms in the CRM.
> CSM note: Complained about reporting and asked about contract end date.

## Open tickets

**Account `A011` Tidewater Travel, score 8**

Reasons: Usage down 20% in 30 days (+2); 6 open tickets (+2); No contact for 60 days (+2); Renewal in 57 days (+2)

**Subject:** Your top open support request

> Hi Tidewater Travel team,
>
> Your open support requests are not all equally urgent, and you know best which one matters most right now.
>
> Which one matters most to you at the moment? Just reply with its subject line, and I will flag it to the support team as your priority.
>
> Best,
> Customer Success

## Exactly at the threshold

**Account `A026` Quartz Logistics, score 6:** Usage down 15% in 30 days (+2); Only 29% of users active (+2); 3 open tickets (+2). Alert created.

**Account `A025` Juniper Dental, score 5:** same usage and active users, but 2 open tickets. No alert.

## Before and after

The same account, Zephyr Audio, before and after the email changes.

**Before**

> It's been a little while since we last spoke, and I wanted to reach out. Your usage has dipped by 52% over the last month, so I want to make sure things are still working well for your team.
>
> Is 1 December still the right renewal date on your side?

**After**

> Your renewal is on 1 December, so this is a good time to look at what is working for you today and what is still open.
>
> Would a short written summary of both be useful before then? A simple yes or no is enough.

The new version starts with the customer, does not quote tracking data back to them and ends with a question that takes one word to answer.
