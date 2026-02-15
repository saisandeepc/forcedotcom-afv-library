---
name: Trigger Refactor Helper
description: Refactor the Opportunity trigger into a handler pattern with tests
tags: apex, refactor, testing
requires_deploy: true
setup_summary: Deploy baseline trigger to target org before running instructions
---

## Setup
1. Deploy the baseline trigger shown below to your default or scratch org.
2. Confirm the trigger compiles successfully before continuing.

```apex
// ❌ Anti-pattern: all logic stuffed into the trigger, with DML/SOQL in loops.
trigger OpportunityTrigger on Opportunity (before insert, before update, after update) {
    // BEFORE INSERT: validate Closed Won w/ low Amount
    if (Trigger.isBefore && Trigger.isInsert) {
        for (Opportunity o : Trigger.new) {
            if (o.StageName == 'Closed Won' && (o.Amount == null || o.Amount < 1000)) {
                o.addError('Closed Won opportunities must have Amount ≥ 1000.');
            }
        }
    }

    // BEFORE UPDATE: if Stage changed, overwrite Description
    if (Trigger.isBefore && Trigger.isUpdate) {
        for (Opportunity o : Trigger.new) {
            Opportunity oldO = Trigger.oldMap.get(o.Id);
            if (o.StageName != oldO.StageName) {
                o.Description = 'Stage changed from ' + oldO.StageName + ' to ' + o.StageName;
            }
        }
    }

    // AFTER UPDATE: when Stage becomes Closed Won, create a follow-up Task
    if (Trigger.isAfter && Trigger.isUpdate) {
        for (Opportunity o : Trigger.new) {
            Opportunity oldO = Trigger.oldMap.get(o.Id);
            if (o.StageName == 'Closed Won' && oldO.StageName != 'Closed Won') {
                Task t = new Task(
                    WhatId     = o.Id,
                    OwnerId    = o.OwnerId,
                    Subject    = 'Send thank-you',
                    Status     = 'Not Started',
                    Priority   = 'Normal',
                    ActivityDate = Date.today()
                );
                insert t; // ❌ DML in a loop
            }
        }
    }
}
```

## Instructions
Refactor `OpportunityTrigger` into a handler class (or classes) that handles the same behavior using bulk-safe patterns. Ensure the trigger itself delegates and remains behaviorally identical.

## Testing & Reporting
- Create unit tests covering positive and negative paths for each handler method.
- Include a bulk test that updates 50 `Opportunity` records where only half qualify for the `after update` logic.
- Deploy the refactored code and run the tests, then report coverage and key observations.