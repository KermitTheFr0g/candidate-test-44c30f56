# What did I build and why?
I chose to build out the Outbound Webhooks feature in Task C. I felt that this task gave me the opportunity to show my backend knowledge, some forward planning with system design, and a bit of frontend.

The chosen event is `timesheet.approved`. The resason I chose this event was because it posed additional problems that we needed to handle like a user double clicking the approve button, multiple admins approving creating a race condition. This gave us a real example of idempotency that needs solving.

# Key implementation notes
The key implementation of the webhook event feature was for a delivery guarantee so the consumer receives the event at least once. Due to the nature of the events providing the consumer a reliable key to use to de-duple requests was a requirement.

## Postgres Outbox
Once an timesheet has been approved, the `WebhookEvent` row and `WebhookDelivery` row for each of the company's active endpoints are written. The key part of this is that they are written in the same database transaction. This ensures that there can be no race condition for two admins marking a timesheet as approved and if there is an issue with either the `TimesheetEntry` UPDATE or either webhook INSERT then everything within the transaction fails. This is in place to ensure that the a webhook event isn't created for a timesheet that is not updated in the DB and vice versa.

## Idempotency and Signing


## Worker

## Retry Policy

# My workflow
## Planning
Before even looking at the code I spun up the application locally so I could login and understand the flow for the user.

My next f more in depth read in the routes that this feature would require changes to be made to. Here that is the `webhooks`, and `contracts` routes in the backend.

With an idea for how the code currently looks and the requirements for the feature I brainstorm some ideas and how I want the feature to look:
```

```

I then refine these notes and create a Claude Code plan session, I always ensure that the plan is broken down in to multiple steps so that I can easily manage the output from and any new code that is being written.

## Building
Even after creating the plan I will begin to write some "boilerplate" code and classes to how I want them to be written.

Once I had a Claude Code plan created that matched my requirements, the technology, and plan that I had built out the next step is to start allowing Claude to generate code. I like to keep this in stages so that I can read and assess every function that is being written, I don't like to feel disconnected from the code in anyway.

As functions are generated I will read them to ensure they are how I want the code to be written. An example here is the generic `http_post` function request in `backend/webhooks/deliver.py` originally used a broad try except which I didn't like because it meant the customer didn't get the full story for why an event was unsuccessful.

## Testing and Verification

# Next steps
