---
name: PricingService Invocable Scaffold
description: Create a PricingService Apex class with an invocable getPricing method and typed inputs/outputs for Flow.
tags: apex, invocable, flow, scaffold
---

Scaffold a Apex classed named `PricingService` (public with no namespace) with one invocable method `getPricing` with an InvocableVariable wrapper for inputs and outputs. Include user-friendly labels and descriptions for the InvocableMethod and all InvocableVariables.  

## Input Parameters
- accountId (ID) - required Account the pricing is for
- terms (String) - required terms for the pricing
- discount (Decimal - scale 2) - if null, then discount equals 0.
- products (List of Strings) - the required list product names.

## Output Parameters
- total - (Decimal) the toal price for all products
- products (List of Decimal) - the list product prices.
- creditApproved (boolean) - if approved for credit

If input `products` variable is null or empty, return empty output parameters. Use a standard invocable pattern. 

I will provide the implementation later. Do not write unit tests. Deploy the new class to my default org.